---
layout: post
title: Use your web browser as Notepad
description: Turning your web browser into a Notepad by pasting this data URI hack into the web browser URL bar.
keywords: web browser as notepad, data uri hack
tags: [Data URI]
comments: true
---

We all use Notepad or sticky note or some editors to take immediate notes on something like code snippets, ideas, blog content or to-dos. Well, I just found this hack is quite interesting to use where we can just turn our web browser to be just like a Notepad. The trick actually hacks around the [Data URI scheme](http://en.wikipedia.org/wiki/Data_URI_scheme) and HTML [contenteditable](http://www.w3schools.com/tags/att_global_contenteditable.asp) attribute.

The real trick is just type the following code into our web browser URL bar:

```html
data:text/html,<html contenteditable>
```

And the code above will make your page editable just like the Notepad. This is the foundation of the hack! However, to make it looks more interesting, I expand the code and add some CSS into it so it will look better than the basic. Here is **my version of code**:

```html
data:text/html,<html contenteditable><title>Browser Text</title><style>html,body,div,span,iframe,h1,h2,h3,h4,h5,h6,p,blockquote,pre,a,abbr,address,cite,code,del,dfn,em,img,ins,kbd,q,samp,small,strong,sub,sup,var,dl,dt,dd,ol,ul,li,fieldset,form,label,legend,table,caption,tbody,tfoot,thead,tr,th,td,article,aside,canvas,details,embed,figure,figcaption,footer,header,hgroup,menu,nav,output,ruby,section,summary,time,mark,audio,video{margin:0;padding:0;border:0;font-size:100%;font:inherit;vertical-align:baseline;}article,aside,details,figcaption,figure,footer,header,hgroup,menu,nav,section{display:block;}body{line-height:1;}ol,ul{list-style:none;}blockquote,q{quotes:none;}blockckquote:before,blockquote:after,q:before,q:after{content:'';content:none;}table{border-collapse:collapse;border-spacing:0;}body{font-family:monospace;font-size:12px;line-height:20px;background:linear-gradient(white,#fff 19px,#ddd 19px,#ddd 20px);background-size:20px 20px;padding:20px 30px;white-space:pre-wrap;word-wrap:break-word}</style></html>Edit this text and start type yours...
```

Go ahead, copy the code and paste it into your web browser! You should see a better look of it. Now, you can start typing whatever you want there.

Okay, to **save** the content, just do the usual web browser save; `CMD+S` for OSX or `Ctrl+S` for Windows. It will save your content as `.html` file. You can also **bookmark** the above data URL to make it easier.

Have a try and good luck!
