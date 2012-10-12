---
layout: post
title: Image Caching for a WPF Desktop Application
date: 2009-12-12 21:49
comments: true
---
WPF is a great technology and in my opinion is miles better than WinForms. Unfortunately, like any technology there are always going to be shortfalls that you have to work around yourself. My company is presently developing a WPF Desktop Application that fetches images from a web server. One of the shortfalls of WPF is that client side caching is not implemented, but you can do it yourself using custom bindings.

Consider the following code where the binding *ImageAddress* points to an image on some web server (www.someserver.com/image.jpg):

    <Image Source="{Binding ImageAddress}"/>

Each time the Window in which this code resides is loaded the image will be fetched from the web server.

**Every time you say?!** *Every time...*

Considering the application we are working on will be displaying a lot of images, this is just not acceptable. I kept thinking that there has to be a way to turn on caching, but after Googling (the good kind) until my hands were sore I discovered there was no switch I could just turn on. I [asked a question](http://stackoverflow.com/questions/1878060/how-do-i-cache-images-on-the-client-for-a-wpf-application) on StackOverflow and it was suggested that I just save the images to a local directory. Unfortunately that was all he said.

After some searching I discovered that using the [Binding.Converter](http://msdn.microsoft.com/en-us/library/system.windows.data.binding.converter.aspx) attribute I could pass the value of the Source attribute to a Converter and then back again. To create a converter you create a class that implements [IValueConverter](http://msdn.microsoft.com/en-us/library/system.windows.data.ivalueconverter.aspx) requires that you implement two methods:

    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)

**Convert** will be used to convert the data you pass to it into the desired format to display on the form. **ConvertBack** is the reverse, where any changes the user makes to the data displayed on the form may need to be converted back to a format that you require for processing (eg. to send to a database). Here is an example as provided on msdn:
 > The following example shows the implementation of a date converter that converts the date value passed in so that it only shows the year, the month, and the day. When implementing the IValueConverter interface, it is a good practice to decorate the implementation with a [ValueConversionAttribute](http://msdn.microsoft.com/en-us/library/system.windows.data.valueconversionattribute.aspx) to indicate to development tools the data types involved in the conversion, as in the following example:

	[ValueConversion(typeof(DateTime), typeof(String))]
	public class DateConverter : IValueConverter
	{
	  public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
	  {
	    DateTime date = (DateTime)value;
	    return date.ToShortDateString();
	  }
	
	  public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
	  {
	    string strValue = value as string;
	    DateTime resultDateTime;
	    if (DateTime.TryParse(strValue, out resultDateTime))
	    {
	      return resultDateTime;
	    }
	    return DependencyProperty.UnsetValue;
	  }
	}

As you can see, in Convert *value* is cast to a DateTime object and then returned as a string using *ToShortDateString()*. Then in ConvertBack (which will be passed a string from the form) the string is parsed into a date using *DateTime.TryParse()* and then returned if it completes successfully.

So, onto the specific problem of caching an image on the local machine. I start with an empty class called *ImageCacher* implementing stub methods from IValueConverter. As we are grabbing the web address from a database, we are not interested in sending the local address back to the database, so we can simply complete ConvertBack returning a null value.

	//Not needed ├ö├ç├┤ return null
	public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
	{
	  return null;
	}

We then add the ValueConversionAttribute to indicate to development tools the data types involved in the conversion. This is not compulsory, but I think it also makes understanding what the converter is doing easier in a single glance. Our goal is to grab the string from the Source attribute and then pass one back with the updated image location (after we save it locally). So in this case, we are converting from a string to a string.

	[ValueConversion(typeof(string), typeof(string))]
	class ImageCacher : IValueConverter
	{

Let us define where our images will be saved.

	static private readonly string AppDataDirectory = String.Format(├ö├ç┬ú{0}/ImageCacher/", Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData));

Here we use the Environment class to return the path to the Application Data folder on the local machine via the GetFolderPath method with the SpecialFolder.ApplicationData enum. I have simply appended that directory with the directory ImageCacher, as that is where we will store our images.

First thing in Convert is to check to see if our directory already exists, and create it if it doesn├ö├ç├ût.

	public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
	{
		//Check to see if the directory in AppData has been created
		if (!Directory.Exists(AppDataDirectory))
		{
			//Create it
			Directory.CreateDirectory(AppDataDirectory);
		}

Since we are working with a web address, we will create a Uri from our string. Once we have that, we can use the Segments property of the Uri class to grab the name of the image in the web address, and we'll use that to define the local path where the image will be stored (or is already stored, if it has been downloaded before).

	//Cast the string into a Uri so we can access the image name without regex
	var uri = new Uri((String)value);
	//The full path of the image on the local computer
	var localFile = String.Format(├ö├ç┬ú{0}{1}", AppDataDirectory, uri.Segments[uri.Segments.Length-1]);

Next, we check to see if the image has been stored locally already, and if not, we fetch it using a HttpWebRequest and grab the response using HttpWebResponse.

	if (!File.Exists(localFile))
	{
		var request = HttpWebRequest.Create(uri);
		var response = (HttpWebResponse)request.GetResponse();

Because we used the HttpWebResponse class, we can check the ContentType of the response to make sure that we are in fact fetching an image. If it isn├ö├ç├ût, we throw an exception.

	//check the content type to assert that the file in the uri is an image
	if (!response.ContentType.StartsWith(├ö├ç┬úimage"))
	{
		throw new FileFormatException(uri, String.Format(├ö├ç┬úUri passed to ImageCacher does not return an image. Content is of type {0}.", response.ContentType));
	}

Now that we have asserted that the response is actually an image, we can load it from GetResponseStream into the Image class (System.Drawing) and then save it to the path we defined earlier.

		//load the image from the stream
		var image = Image.FromStream(response.GetResponseStream());
		//save it
		image.Save(localFile);
	}

Finally, return the path to the local file.

		return localFile;
	}

Then in XAML, reference our converter using an xmlns tag and add our converter in the resources area.

	<Window x:Class="MyImageCacher.Window1"
		xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
		xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
		xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
		xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
		xmlns:con="clr-namespace:MyImageCacher.ImageCacher">
	<Window.Resources>
	<con:ImageCacher x:Key="imageCacher"/>

Then we add the Converter attribute to the Binding of our original Image control.

	<Image Source="{Binding ImageAddress, Converter={StaticResource imageCacher}}"/>

And its as simple as that. Each time an address is passed to the control, it will run it through the converter first. It will check to see if the image exists locally, and download it if it hasn├ö├ç├ût been. The solution could be more polished but this is working for us **right now**.
