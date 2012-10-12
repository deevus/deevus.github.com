---
layout: post
title: Sublime Text - A Text Editor For Programmers - Part 1 - Package Control
date: 2012-07-20 20:46
comments: true
categories: []
---
<h2>Come With Me If You Want To Live</h2>

<p>The text editor is a programmer's most valuable tool. Without it, you can't write any code at all. It is your life-blood. You need it to survive. It will save you, just like Arnie did. There are plenty of text editors out there that solve one problem or another. There is also a couple of editors that are popular for certain platforms:</p>

<ul>
<li>Linux users have <a href="http://www.vim.org/">vim</a> </li>
<li>Mac users have <a href="http://macromates.com/">TextMate</a> </li>
<li>And Windows users have <a href="http://notepad-plus-plus.org/">Notepad++</a></li>
</ul>

<p><img src="http://i.imgur.com/cBmpo.png" alt="vim" /></p>

<p>There are others (of course) like <a href="http://www.gnu.org/s/emacs/">Emacs</a>, <a href="http://www.pnotepad.org/">Programmer's Notepad</a>, or even plain old <a href="http://en.wikipedia.org/wiki/Notepad_(software)">Notepad.exe</a>.</p>

<h2>How Did I Get Here?</h2>

<p>I have tried a few editors in the last few years.</p>

<ul>
<li>I learned vim which was a good experience, but I felt it didn't fit in with the Windows ethos. Being able to edit completely without the help of a mouse was interesting, but I just couldn't break completely free of it.</li>
<li>Notepad++ is a solid editor which served me well for several years, but it just isn't nice to look at. It is powerful, but does not have a bustling community making extensions for it. It can also be a bit sluggish to use.</li>
</ul>

<h2>The Saviour</h2>

<p><a href="http://www.sublimetext.com/2">Sublime Text</a> is amazing! You only have to watch the demonstrations on <a href="http://www.sublimetext.com/">their homepage</a> to see why. Unlike all the above editors, <a href="http://www.sublimetext.com/buy">it is not free</a>, but after the 30 day trial I was sold.</p>

<p><a href="http://i.imgur.com/ixGzs.png" target="_new" /><img src="http://i.imgur.com/ixGzsl.png" alt="Sublime Text 2" /></a></p>

<h2>TextMate Compatibility</h2>

<p>One of the major drawcards for me (despite not having used TextMate) is that ST2 is 100% compatible with <a href="http://wiki.macromates.com/Main/Bundles">TextMate Bundles</a>. This gives you access to a massive range of add-ons from the super popular Mac editor. Ones that I use are the Pascal and CacheManifest syntax bundles. I also have native packages to <a href="https://github.com/bgreenlee/sublime-github">create Github Gists</a>, <a href="https://github.com/joelpt/sublimetext-print-to-html">print to HTML</a> and <a href="https://github.com/ericmartel/Sublime-Text-2-Stackoverflow-Plugin">search Stack Overflow</a> to name a few.</p>

<h2>Package Control</h2>

<p>Packages can be installed using <a href="http://wbond.net/sublime_packages/package_control">Sublime Package Control</a>. It is a package itself which grants package management right inside Sublime Text. Once installed - you can install, upgrade and uninstall packages using the Command Palette (<code>Ctrl + Shift + P</code> on Windows).</p>

<p><strong>Installing</strong></p>

<p>To install Package Control we are going to run Python code from the console. Copy the following code and paste it into the Sublime Text console, accessed using <code>Ctrl + `</code> and run it by pressing <code>Enter</code>.</p>

    import urllib2,os; pf='Package Control.sublime-package'; ipp=sublime.installed_packages_path(); os.makedirs(ipp) if not os.path.exists(ipp) else None; urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler())); open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read()); print 'Please restart Sublime Text to finish installation'

<p>There are <a href="http://wbond.net/sublime_packages/package_control/installation#Manual_Instructions">manual instructions</a> if the console install does not work for you.</p>

<p>Once installed, check out the <a href="http://wbond.net/sublime_packages/package_control/usage">Usage</a> page to get started. Try typing your favourite language into the <code>Package Control: Install Package</code> search query.</p>

<p>Stay tuned for more Sublime Text tips :)</p>

