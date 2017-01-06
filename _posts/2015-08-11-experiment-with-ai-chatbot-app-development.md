---
layout: post
title: Experiment with AI chatbot app development
description: Developing artificial intelligence application always be an interesting thing to do. AI chatbot is one of fun stuffs to experiment with.
keywords: artificial intelligence, chatbot, chatter bot, aiml, siml, elizabot.js
tags: [Artificial Intelligence, Chatbot]
comments: true
---

### AI chatbot using mIRC script

10 years ago I had been using a chatting program called [mIRC](http://www.mirc.com/) and it was quite popular during that time. This [IRC](https://en.wikipedia.org/wiki/Internet_Relay_Chat) client program however had one very interesting feature that grabbed my attention which called remote. It allowed user to write some scripts in order to expand the capability of the application features beyond the limit or to customize the application to make it better in chatting experience. Usually the person who wrote the scripts is called a IRC scripter.

Being a IRC scripter had made me to become more creative person as there are so many things we can do with scripting. For example, a script that can protect the channels from user spamming or flooding, listen to MP3 while chatting, or run a quiz bot. Anything you can imagine can be realized if you know how to write the script or code. One of my favorites was building a IRC chatbot. Unfortunately, there is no any markup language implemented into the chatbot. Just a simple knowledge base with one to one input-output responses. Just enough to get fun with IRC scripting!

### AI chatbot using .NET Framework and SQLite database

Below are some snapshots of my chatbot app experiment developed in .NET C# and using SQLite as its database. By using SQLite to save each input and output response made it looked like a standard knowledge base program as I need to key in each possible input and its response respectively.

[![WAYI v1](http://i.imgur.com/F1n1W0N.png)](http://i.imgur.com/F1n1W0N.png)

[![WAYI v1](http://i.imgur.com/IkAKC9p.png)](http://i.imgur.com/IkAKC9p.png)

[![WAYI v1](http://i.imgur.com/kMdAEpk.png)](http://i.imgur.com/kMdAEpk.png)

Not as natural as I expected it to be, to be called "artificial intelligence" chatbot. So, I need to find a better way to do it...

After I had done some researches on Internet, I found one so called the "first framework" I can start with. It was called [Artificial Intelligence Markup Language (AIML)](http://www.alicebot.org/aiml.html), a standard XML format markup language for defining the responses from the chatbot. AIML was developed by **Richard S. Wallace** and a worldwide free software community between 1995 and 2002. AIML formed the basis for what was initially a highly extended [Eliza](https://en.wikipedia.org/wiki/ELIZA) called ["A.L.I.C.E." (Artificial Linguistic Internet Computer Entity)](https://en.wikipedia.org/wiki/Artificial_Linguistic_Internet_Computer_Entity) which won the annual [Loebner Prize Competition](https://en.wikipedia.org/wiki/Loebner_Prize) in Artificial Intelligence three times and was also the Chatterbox Challenge Champion in 2004.

I have used [AIMLbot.dll](http://aimlbot.sourceforge.net/) as the library to my chatbot app while I got myself to learn more about the markup language structures used in AIML from [this research paper](http://arxiv.org/ftp/arxiv/papers/1307/1307.3091.pdf). AIML provided much better way of defining the knowledge database of my chatbot and makes it more natural look to call as "artificial intelligence" chatbot. Other than that, I can expand my chatbot features such as executing commands or tasks based on the captured inputs.

[![WAYI v1](http://i.imgur.com/UJjTodD.png)](http://i.imgur.com/UJjTodD.png)
_WAYI v1_

[![WAYI v2](http://i.imgur.com/3mkEzII.png)](http://i.imgur.com/3mkEzII.png)
_WAYI v2_

### AI chatbot using .NET Framework and SIML

After some time, I had found another chatbot markup language known as [Synthetic Intelligence Markup Language (SIML)](http://simlbot.com/), which was more powerful than AIML. SIML provided much better features than AIML. So, I changed my chatbot markup language from AIML to use SIML as SIML already provided their own chatbot studio program called Syn Chatbot Studio.

Based on their site, Syn Chatbot Studio offered a comprehensive collection of tools to develop intelligent chatbots that targeted desktops, mobile and web platforms. It had Code Analysis, AIML to SIML converter, JavaScript Editor, Regex Tester and smooth Auto-Complete. More interestingly, it had ability to execute JavaScript function from its routine of responses. That was very cool thing I told you.. as a lot of things we can do here with JavaScript.

**Links:** [Wiki](http://wiki.syn.co.in/) / [GitHub](https://github.com/SynHub)

### AI chatbot using JavaScript

Introducing "W4Y1", one of my latest AI chatbot experiments that uses JavaScript and accessible online. It's not a really chatbot where people can chat for any topic, but it's more to so called a memory program to represent a part of myself as a fragment of knowledge in my mind.

If you have ever watched [I, Robot (2004)](http://www.imdb.com/title/tt0343818/) movie, yes, this is inspired from Dr. Alfred Lanning's hologram device used to leave his message for Spooner's investigation. While mine is just in the form of terminal layout, text format interaction and hosted online.

Technically, I created W4Y1 based on [elizabot.js](http://www.masswerk.at/elizabot/) by **Norbert Landsteiner** as its interpretation engine for AI markup language and processing, [jQuery Terminal Emulator plugin](http://terminal.jcubic.pl/) by **Jakub Jankiewicz** for the program interface (GUI), and [particles.js](http://vincentgarreau.com/particles.js/) by **Vincent Garreau** for the particles effect in the background.

[![W4Y1](http://i.imgur.com/7emX4MU.png)](http://i.imgur.com/7emX4MU.png)
_The Screenshot_

**Live Demo:** [http://heiswayi.github.io/w4y1](http://heiswayi.github.io/w4y1)

Note: W4Y1 is just an experimental AI app, so expected not much features for the moment. However, there are some my to-do things I might want to add into W4Y1 in the future development such as:-

* Integrate with [math.js](http://mathjs.org/) for mathematical computations.
* Implement AJAX request for accessing PHP files, so more features can be performed by PHP.
* Centralize the knowledge database somewhere and secure it.

As I grow older, my memory becomes weaker. Perhaps, an app like this in future may capture everything of my knowledge and remember it for me one day once I forgot...

### Conclusion

Artificial Intelligence and the technology are one side of the life that always interest and surprise us with the new ideas, topics, innovations, products …etc. AI is still not implemented as the films representing it (i.e. intelligent robots), however there are many important tries to reach the level and to compete in market, like sometimes the robots that they show in TV. Nevertheless, the hidden projects and the development in industrial companies.

At the end, we’ve been in this research through the AI definitions, brief history, applications of AI in public, applications of AI in military, ethics of AI, and the three rules of robotics. This is not the end of AI, there is more to come from it, who knows what the AI can do for us in the future, maybe it will be a whole society of robots.
