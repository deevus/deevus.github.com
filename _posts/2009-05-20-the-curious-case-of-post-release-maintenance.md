---
layout: post
title: The Curious Case of Post-Release Maintenance
date: 2009-05-20 20:02
comments: true
categories: []
---
Generally, the <strong>Software Development Process </strong>is pretty clear cut.
<ul>
	<li>Requirements</li>
	<li>Specification</li>
	<li>Architecture</li>
	<li>Design</li>
	<li>Construction/Testing/Documentation</li>
	<li>Release and Maintenance</li>
</ul>
When it comes to post-release maintenance, it is an iterative process that goes over all these steps in a smaller scale.Â  This is often referred to as <a href="http://en.wikipedia.org/wiki/Iterative_development">iterative and incremental development</a>. As a developer that has only been part of my current project post-release, its hard to say what the process was prior to the initial release,Â  but I can only imagine, and it isn't good. 

The code is riddled with 1000 line routines and little gems like this:

[pascal]
try
	//Hah, try to debug this code!
	//Nice try, schmuck.
except end;
[/pascal]
Database connection dropped out? This exception code will handle it...NOT. You'll just be plugging away doing some work and for some reason it will stop working. Its not as bad as it used to be though.

Or this:

[pascal]
if (someExpression) then begin
	if (anotherExpression) then begin
		if (yetAnotherExpression) then begin
			//and so on!
		end;
	end;
end;
[/pascal]

Nothing beats debugging some arrowhead code in the morning. I'd even go on to wager that my example above has better variable names than the code I'm trying to imitate. Its always fun trying to debug an aforementioned 1000 line routine that uses a boolean named DataOK about 17 times. In DataOK's defence, it has probably had a longer life that I have.

As you can imagine, that's not all. There are things like ADOTable objects that load all the data in a table and aren't even used in the code, and I don't even want to go into how the business logic and data access is done all in the form code...
