---
layout: post
title: First Ubiquity Command - quote-to-quick-reply
date: 2009-05-13 10:15
comments: true
categories: []
---
I have discovered the <a href="http://labs.mozilla.com/projects/ubiquity/">Ubiquity</a> extension for Firefox and I am loving it. So much that I <a href="http://twitter.com/deevus/status/1771246973">tweeted</a> about it and have decided I will try to develop some commands for it in the future. I have been using <a href="http://www.humanized.com/enso/launcher/">Enso Launcher</a> for a while now and Ubiquity seems like the perfect companion for the web.

I use several phpbb forums and some of those have the little <span style="font-weight: bold;">"Quick Reply"</span> boxes at the bottom which I tend to use a lot. As far as I know you can't quote posts in the thread without following the quote link on the post you want to actually quote. I wouldn't call this command that useful but it was a good learning experience on how to create commands for Ubiquity.

[js]CmdUtils.CreateCommand({
  name: &quot;quote-to-quick-reply&quot;,
  homepage: &quot;http://SimonHartcher.com&quot;,
  author: {name: &quot;Simon Hartcher&quot;, email: &quot;simon@simonhartcher.com&quot;},
  license: &quot;GPL&quot;,
  description: &quot;Quotes the selected text to the Quick Reply box.&lt;br&gt;Only works for phpbb forums with Quick Reply enabled.&quot;,
  execute : function() { 
    //get selected text
    var selection = CmdUtils.getSelection();
    //get the node of the selection text
    var parent = CmdUtils.getWindow().getSelection().anchorNode;
    {
      //traverse the nodes until the parent table is found
      while (parent != null &amp;&amp; parent.className != &quot;tablebg&quot;) {
	parent = parent.parentNode;
      }
    }
    //find the name of the post author from within the table node
    var author = jQuery(&quot;.postauthor&quot;, parent.innerHTML).html();
    //construct the  quote text
    var quotetext = &quot;[quote=\&quot;&quot;+ author + &quot;\&quot;]&quot;+ selection +&quot;[/quote]&quot;;
    //place it quote text into quick reply box
    var doc = CmdUtils.getDocument();
    var quickReply = jQuery(&quot;#message&quot;, doc);
    quickReply.val(quickReply.val() + quotetext);
  }
});
[/js]

With this command you simply select the text that you want to quote and execute the <span style="font-weight: bold;">quote-to-quick-reply</span> command in Ubiquity. It will put the quoted text (including the author of the post) into the quick reply box. You can do this as many times as needed (if you need to quote more than one post for example).
Its still in its infancy at the moment so there are many ways you can break it (such as using it on text that isn't within a post OR using it anywhere other than a phpbb forum with quick reply enabled).

<a href="http://SimonHartcher.com/ubiquity/verbs/quote-to-quick-reply">Subscribe</a>
