---
layout: post
title: Update All Hg Repositories Using Powershell
date: 2012-07-05 11:48
comments: true
categories: []
---
<h1>Developer Morning Ritual</h1>

<p>As part of my morning ritual I need to update all of my source control repositories so that I have the latest code to start the day. For the record, I'm using Mercurial (hg).</p>

<p>For the longest time I was going through each repository using <em>TortoiseHg Workbench</em> and doing a manual <code>hg pull</code> using the GUI. The amount of repositories has gotten to a ridiculous number so I decided I would automate the process.</p>

<h1>Yay, Powershell!</h1>

<div style="text-align:center;">
  <img src="http://i.imgur.com/hxnBnl.png" alt="Creating Jobs" />
</div>

<p>Given that I am working on Windows I decided to use <a href="http://en.wikipedia.org/wiki/Windows_PowerShell">Powershell</a>, which I feel is very powerful and exactly what we <small>(Windows developers)</small> needed in a command line.</p>

<h1>Teh Codez</h1>

<p>[gist id=1506832]</p>

<p>There's not a lot going on here:</p>

<ol>
<li>Get all the directories in the same directory as the script that contain Hg repositories</li>
<li>Start a job to update each repository with a unique name (HgUpdate plus the name of the directory)</li>
<li>Wait for all the jobs to complete</li>
<li>Print the result of all the jobs to the screen</li>
<li>Wait for user input</li>
</ol>

<p>The cool thing happening in this script is that all the repositories will be pulled asyncronously using the <a href="http://technet.microsoft.com/en-us/library/hh849698.aspx"><code>Start-Job</code></a> command. This way we don't have to wait for one to finish before the next one can start. The result is a morning filled with the latest codez for no effort :)</p>

<h1>What About Git?</h1>

<p>Using minimal changes you should be able to get this script working with Git. I haven't provided a Git version because I don't need it, and you smart developers should be able to make the changes yourself. Changing the <code>-filter</code> to look for a Git repository and the <code>hg pull --update</code> line to get the latest Git changes should do it. If you do it, let me know!</p>

