---
layout: post
title: HOWTO: Query MSSQL And Send HTML Email Using Powershell
date: 2009-08-13 11:31
comments: true
categories: []
---
Before I start this tutorial I must preface this with a warning: <strong>The amount of coding in this tutorial is so minimal it is almost sexually exciting. </strong>I have found that <a href="http://www.microsoft.com/windowsserver2003/technologies/management/powershell/default.mspx">Powershell</a> is such a powerful scripting tool that you can accomplish tasks that would take 100's of lines in a lot less. This tutorial is an amalgamation of several different tutorials I found online when I wanted to have a crack at emailing a report to a client. As it turned out it is now run daily as part of our scheduled tasks.

Powershell is what you would call the successor of Command Prompt but more is akin to a *nix command line tool like bash. As it seems that <em>everything</em> is going .NET these days it should be no surprise that Powershell also leverages the <a href="http://en.wikipedia.org/wiki/.NET_Framework">.NET Framework</a>. If you have used C# or VB.NET before the kind of objects you will be working with will be quite familiar. I am running Powershell 2.0 on the Windows 7 RTM (which comes preinstalled) but Powershell 1.0 will also work with this tutorial.

<strong>Getting A Working Connection String</strong>

The first thing we want to do is form our SQL query which will be the data that we will email to our client. To do that in Powershell we are going to need a connection string. A handy little tool that I use to grab a connection string is the Data Link Properties file. You can just create a new file somewhere called:

[sourcecode]Test.udl[/sourcecode]

You will need to be able to <a href="http://www.fileinfo.com/help/windows-show-extensions.html">see all file extensions</a> to create the file. Open the file using Windows Explorer and go to the Provider tab. Once there you need to select â€œMicrosoft OLE DB Provider for SQL Serverâ€.<img style="display: block; float: none; margin-left: auto; margin-right: auto; border-width: 0px;" title="Data Link Properties - Provider Tab" src="http://www.simonhartcher.com/wp-content/uploads/2009/08/image_thumb1.png" border="0" alt="Data Link Properties - Provider Tab" width="381" height="477" />

Once that has been completed we need to go to the Connection tab and enter our server details. If you have entered the correct details (Server Name, User and Password) you should now be able to click the â€œSelect the database on the serverâ€ checkbox which will show a dropdown list of available databases. For the purpose of this tutorial I have chosen the AdventureWorks database.

<a href="http://www.simonhartcher.com/wp-content/uploads/2009/08/image1.png"><img style="display: block; float: none; margin-left: auto; margin-right: auto; border-width: 0px;" title="Data Link Properties - Connection" src="http://www.simonhartcher.com/wp-content/uploads/2009/08/image_thumb2.png" border="0" alt="Data Link Properties - Connection" width="381" height="477" /></a>

<a href="http://www.simonhartcher.com/wp-content/uploads/2009/08/image15.png"><img style="display: inline; margin-left: 0px; margin-right: 0px; border-width: 0px;" title="Data Link Dialog Box" src="http://www.simonhartcher.com/wp-content/uploads/2009/08/image_thumb3.png" border="0" alt="Data Link Dialog Box" width="244" height="111" align="right" /></a>

Please also note that I have checked â€œAllow saving passwordâ€ that will make it so we donâ€™t have to enter it manually later. In my case, since the script is sitting on a work server there is no risk in leaving the password as plain text in the file.

<a href="http://www.simonhartcher.com/wp-content/uploads/2009/08/image16.png"></a>

Once you have completed all of the above, click Ok. You will get a warning about saving the password to which you can just click yes. Obviously in a less secure environment on a production server you may want to consider other options for storing your password, but that is beyond the scope of this tutorial.

<a href="http://www.simonhartcher.com/wp-content/uploads/2009/08/image16.png"></a>

Now that the file (Test.udl) is saved if you open it in your favourite text editor you will see something like this (I have removed my server password for obvious reasons):

[sourcecode language="text"][oledb]
; Everything after this line is an OLE DB initstring
Provider=SQLOLEDB.1;Password=*******;Persist Security Info=True;User ID=sa;Initial Catalog=AdventureWorks;Data Source=DEEVUS-PC\SQLEXPRESS[/sourcecode]

All we actually need is the text on line 3 excluding the first segment â€œProvider=SQLOLEDB.1;â€ which gives us our working connection string!

[sourcecode language="text"]&quot;Password=*******;Persist Security Info=True;User ID=sa;Initial Catalog=AdventureWorks;Data Source=DEEVUS-PC\SQLEXPRESS&quot;[/sourcecode]

<strong>Querying SQL Using Powershell</strong>

Now that we have a working connection string, we can begin coding our Powershell script. The first thing we need to do is create our connection object:

[sourcecode language="python"]
#Connection Object
$cn = New-Object System.Data.SqlClient.SqlConnection(
    &quot;Password=********;Persist Security Info=True;User ID=sa;Initial Catalog=AdventureWorks;Data Source=DEEVUS-PC\SQLEXPRESS&quot;
)
[/sourcecode]

Then we create our query string:

[sourcecode language="python"]$q = &quot;SELECT TOP 50 * FROM HumanResources.vEmployee ORDER BY LastName&quot;[/sourcecode]

We also need an Adapter and a DataSet to gather and hold our data. Once you have gathered the data using the <a href="http://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqldataadapter.aspx">SqlDataAdapter</a> you put the results into the <a href="http://msdn.microsoft.com/en-us/library/system.data.dataset.aspx">DataSet</a>:

[sourcecode language="python"]
#Data Adapter which will gather the data using our query
$da = New-Object System.Data.SqlClient.SqlDataAdapter($q, $cn)
#DataSet which will hold the data we have gathered
$ds = New-Object System.Data.DataSet
#Out-Null is used so the number of affected rows isn't printed
$da.Fill($ds) | Out-Null
#Close the database connection
$cn.Close()
[/sourcecode]

<strong>Creating The HTML Email</strong>

So now we have the data that we want to email we need to format it into HTML. Powershell has an inbuilt function called <a href="http://www.microsoft.com/technet/scriptcenter/topics/msh/cmdlets/convertto-html.mspx">ConvertTo-HTML</a><strong> </strong>which automagically turns data into HTML! It is really quite simple:

[sourcecode language="python"]
$emailbody = $ds.Tables[0] |
     Select-Object LastName, FirstName, JobTitle, Phone, EmailAddress, AddressLine1, City, PostalCode |
     ConvertTo-HTML
[/sourcecode]

<strong><a href="http://technet.microsoft.com/en-us/library/dd315291.aspx">Select-Object</a></strong> will simply grab the members from the DataSet that we need. As you can see in the above snippet we have grabbed some general information about the employees at AdventureWorks. If youâ€™ve gotten to this point and want to see what the outcome might look like, you can add <strong><a href="http://technet.microsoft.com/en-us/library/dd315303.aspx">Out-File</a> test.html</strong> to the end of the last line so that it looks like this:

[sourcecode language="python"]
ConvertTo-HTML | Out-File test.html
[/sourcecode]

<a href="http://www.simonhartcher.com/wp-content/uploads/2009/08/image6.png"><img style="display: inline; margin-left: 0px; margin-right: 0px; border-width: 0px;" title="Output in browser" src="http://www.simonhartcher.com/wp-content/uploads/2009/08/image_thumb4.png" border="0" alt="Output in browser" width="244" height="196" align="right" /></a>This will save the results to a html file which you can freely view in your preferred browser.

As you can see from your html file (or the screenshot provided) it is not the prettiest markup on the internet. However, there are ways to improve the look and feel of the generated html via the <strong>â€“head</strong> parameter of ConvertTo-HTML. The head parameter will essential pass whatever you give it to the &lt;head&gt; tags of the HTML. The head of a HTML markup can be used to shape the look and feel of the body of the document, so this way we can pass it some information to make it look nicer.

[sourcecode language="python"]
#HTML Email Styles
$style = &quot;&lt;style type='text/css'&gt;&quot;
$style = $style + &quot;BODY{background-color:#FFFFFF;font-family:Verdana;}&quot;
$style = $style + &quot;TABLE{border-width: 1px;border-style: solid;border-color: black;border-collapse: collapse;font-size:12px;}&quot;
$style = $style + &quot;TH{border-width: 1px;padding: 0px;border-style: solid;border-color: black;background-color:#D3D3D3 }&quot;
$style = $style + &quot;TD{border-width: 1px;padding: 0px;border-style: solid;border-color: black;background-color:#FFFFFF }&quot;
$style = $style + &quot;&lt;/style&gt;&quot;
[/sourcecode]

And then the last line will look like this to implement the styles we just created:

[sourcecode language="python"]
ConvertTo-HTML -head $style | Out-File test.html
[/sourcecode]

<a href="http://www.simonhartcher.com/wp-content/uploads/2009/08/image11.png"><img style="display: block; float: none; margin-left: auto; margin-right: auto; border-width: 0px;" title="Output in browser with styles" src="http://www.simonhartcher.com/wp-content/uploads/2009/08/image_thumb5.png" border="0" alt="Output in browser with styles" width="604" height="484" /></a>

<strong>Sending The Email</strong>

For the final stage of this tutorial we need to actually send the email. First things first we should create all the variables pertaining to sending an email â€“ From, To and the Subject

[sourcecode language="python"]
$emailFrom = &quot;deevus@isp.com.au&quot;
$emailTo = &quot;someone@anotherisp.com&quot;
$subject = &quot;AdventureWorks - Current Employees&quot;
[/sourcecode]

Now we form the message using the <a href="http://msdn.microsoft.com/en-us/library/system.net.mail.mailmessage.aspx">Net.Mail.MailMessage</a> class.

[sourcecode language="python"]
$message = New-Object Net.Mail.MailMessage($emailFrom, $emailTo, $subject, $emailbody)
#This is needed to make sure it interprets the email as HTML
$message.IsBodyHTML = $true
[/sourcecode]

And the SMTP server using the <a href="http://msdn.microsoft.com/en-us/library/system.net.mail.smtpclient.aspx">Net.Mail.SmtpClient</a> class.

[sourcecode language="python"]
$smtpServer = &quot;smtp.isp.com.au&quot;
$smtp = New-Object Net.Mail.SmtpClient($smtpServer)
[/sourcecode]

Finally, like magic, we send the email!

[sourcecode language="python"]
$smtp.Send($message)
[/sourcecode]

Before we run the script, make sure you remove or comment out <strong>| Out-File test.html</strong>, otherwise the email will be blank.

[sourcecode language="python"]
ConvertTo-HTML -head $style #| Out-File test.html
[/sourcecode]

Then all you need to do is run the script from the Powershell console.

<a href="http://www.simonhartcher.com/wp-content/uploads/2009/08/image12.png"><img style="display: block; float: none; margin-left: auto; margin-right: auto; border-width: 0px;" title="Powershell console" src="http://www.simonhartcher.com/wp-content/uploads/2009/08/image_thumb6.png" border="0" alt="Powershell console" width="889" height="207" /></a>

If all is well, your recipient should receive a nicely formatted email sent using Powershell with data from SQL.

<strong>Great Success!</strong>

Here is the Powershell script in its entirety:

[sourcecode language="python"]
#Connection Object
$cn = New-Object System.Data.SqlClient.SqlConnection(
        &quot;Password=********;Persist Security Info=True;User ID=sa;Initial Catalog=AdventureWorks;Data Source=DEEVUS-PC\SQLEXPRESS&quot;
    )

$q = &quot;SELECT TOP 50 * FROM HumanResources.vEmployee ORDER BY LastName&quot;

#Data Adapter which will gather the data using our query
$da = New-Object System.Data.SqlClient.SqlDataAdapter($q, $cn)
#DataSet which will hold the data we have gathered
$ds = New-Object System.Data.DataSet
#Out-Null is used so the number of affected rows isn't printed
$da.Fill($ds) | Out-Null
#Close the database connection
$cn.Close()

#HTML Email Styles
$style = &quot;&lt;style type='text/css'&gt;&quot;
$style = $style + &quot;BODY{background-color:#FFFFFF;font-family:Verdana;}&quot;
$style = $style + &quot;TABLE{border-width: 1px;border-style: solid;border-color: black;border-collapse: collapse;font-size:12px;}&quot;
$style = $style + &quot;TH{border-width: 1px;padding: 0px;border-style: solid;border-color: black;background-color:#D3D3D3 }&quot;
$style = $style + &quot;TD{border-width: 1px;padding: 0px;border-style: solid;border-color: black;background-color:#FFFFFF }&quot;
$style = $style + &quot;&lt;/style&gt;&quot;

$emailbody = $ds.Tables[0] |
     Select-Object LastName, FirstName, JobTitle, Phone, EmailAddress, AddressLine1, City, PostalCode |
     ConvertTo-HTML -head $style #| Out-File test.html

$emailFrom = &quot;deevus@isp.com.au&quot;
$emailTo = &quot;someone@anotherisp.com&quot;
$subject = &quot;AdventureWorks - Current Employees&quot;

$message = New-Object Net.Mail.MailMessage($emailFrom, $emailTo, $subject, $emailbody)
#This is needed to make sure it interprets the email as HTML
$message.IsBodyHTML = $true

$smtpServer = &quot;smtp.isp.com.au&quot;
$smtp = New-Object Net.Mail.SmtpClient($smtpServer)

$smtp.Send($message)
[/sourcecode]

<a href="http://www.simonhartcher.com/?attachment_id=163">Download</a>
