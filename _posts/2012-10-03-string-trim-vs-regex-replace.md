---
layout: post
title: String.Trim vs Regex.Replace
date: 2012-10-03 17:40
comments: true
categories: [string, regex, regex-replace]
---
I encountered this line in some code while I was doing a review:

`userName = Regex.Replace(userName, "[ ]", "");`

It struck me as odd that the programmer would use this in place of `String.Trim()` to "remove whitespace from either side of a string":

`userName = userName.Trim(); //this is what I would have done`

The programmer's response was that he didn't know about `String.Trim()` and just used what he knew.

![My head is full of F#$%](http://i.imgur.com/8Kn4F.png)

I would argue that <code>String.Trim</code> is much clearer to someone reading your code (especially if you didn't know regex), but in his case it wouldn't be because he had no idea what <code>Trim</code> was. The other side effect is that the regex code in this instance would remove <strong>all the whitespace</strong>, not just leading or trailing.

What baffles me the most is that a programmer can be oblivious of string manipulation methods that are present in practically every language. To cite a few:

<a href="http://docs.oracle.com/javase/6/docs/api/java/lang/String.html#trim()">Java</a>:

`String trim() - Returns a copy of the string, with leading and trailing whitespace omitted.`

<a href="http://msdn.microsoft.com/en-us/library/t97s7bs3.aspx">C#</a>:

`Trim() - Removes all leading and trailing white-space characters from the current String object.`

<a href="http://docs.python.org/library/stdtypes.html#str.strip">Python</a>:

`str.strip([chars])
Return a copy of the string with the leading and trailing characters removed. The chars argument is a string specifying the set of characters to be removed. If omitted or None, the chars argument defaults to removing whitespace. The chars argument is not a prefix or suffix; rather, all combinations of its values are stripped`

I'm sure that I could go on. Surely, there would be a similar method in every language that I could think of. I know that I'm picking on <code>Trim</code> here but the idea is that those key string manipulation methods are staples:



<ul>
<li>ToLower</li>
<li>ToUpper</li>
<li>Format (printf)</li>
<li>SubString</li>
<li>EndsWith</li>
<li>StartsWith</li>
<li>Etc...</li>
</ul>