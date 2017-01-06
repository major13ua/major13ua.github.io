---
layout: post
title: Simple Pomodoro Technique Timer application
description: Simple Pomodoro Technique timer application is a very simple .NET application created in C# WPF as a tool to time management and productivity at office.
keywords: pomodoro technique, time management, timer application, c# wpf
tags: [Productivity, CSharp, WPF]
comments: true
---

Talking about Time Management at work, there is one of popular time management life hacks used today, it's known as [Pomodoro Technique](https://en.wikipedia.org/wiki/Pomodoro_Technique). It was created in the 1980s by Francesco Cirillo. The Pomodoro Technique is a time management philosophy that aims to provide the user with maximum focus and creative freshness, thereby allowing them to complete projects faster with less mental fatique.

The technique uses a timer to break down work into intervals traditionally 25 minutes in length, separated by short breaks (eg. 5 minutes). Each 25-minute work period is called a "pomodoro", named after the Italian word for tomato. Francesco Cirillo used a kitchen timer shaped like a tomato as his personal timer, and thus the method's name. You may [google](https://www.google.com/search?q=pomodoro+technique) around if you want to know or learn more about this Pomodoro Technique.

### Using Pomodoro Technique timer application

Inspired from an [open source Pomodoro Technique Timer application](http://github.com/Mellen/Pomodoro) originally developed by [Matthew Ellen](http://www.matthewellen.co.uk/) in 2010, so I forked the source code, [make some changes](https://github.com/heiswayi/Pomodoro) for my own use.

![Screenshot1](http://i.imgur.com/JPmbbLb.png)

_Figure 1 (above): This is the normal look of the application on startup or before pressing "START WORKING" button._

![Screenshot2](http://i.imgur.com/d2Uqbvs.png)

_Figure 2 (above): When the user pressed "START WORKING" button, the application will start the Pomodoro timer and minimize into the Windows tray panel._

![Screenshot3](http://i.imgur.com/1vh1MjD.png)

_Figure 3 (above): After 25-minute period has passed, the application will beep once and show up with a "Take a break!" message._

If the user finished his/her break time, he/she can continue the Pomodoro timer again by pressing "START WORKING" button. Just repeat the process.

> **Tips:** After four "pomodoros" has passed (100 minutes of work time with 15 minutes of break time), you then can take a 15-30 minute break.

This application is a very simple Pomodoro timer application, but a quite helpful to me since my day job is working with PC almost all the time. At least, it can help me to focus better in completing my job tasks.

### Downloads

The latest **source code** is available on [GitHub](https://github.com/heiswayi/Pomodoro) repository or directly visit [**release page**](https://github.com/heiswayi/Pomodoro/releases) to download the latest binary package (executable file). Simple Pomodoro Technique timer app is a freeware and open source.

### License

[MIT](http://heiswayi.github.io/mit-license)
