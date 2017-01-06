---
layout: post
title: Embedding .DLL files into a single .EXE file
description: Some tricks to create a single binary .EXE file with embedded external .DLL files using C# WinForms.
keywords: embedded dll, visual studio
tags: [WinForms, CSharp]
comments: true
---

This method is unusual and rarely people do it, but it still works for me when I want to hide one or two external library binaries like .DLL files into my single .EXE application file. I used to build most of my Windows application in .NET C# by using Visual Studio.

Let's say I want to compile a simple application, for example a Mini Browser, and I have used these following 5 external .DLL files in my project:-

- DevExpress.BonusSkins.v13.2.dll
- DevExpress.Data.v13.2.dll
- DevExpress.Sparkline.v13.2.Core.dll
- DevExpress.Utils.v13.2.dll
- DevExpress.XtraEditors.v13.2.dll

So, I put them in `lib` folder and include all of them in my project as shown in the picture below:

![Include in Project Tree](http://i.imgur.com/n3QaSRh.png)

This is my old project where I used **C# WinForms** to create a pretty simple Mini Browser application.

Alright, after we included all of the files, we need to change **Build Action** of each file's Properties into `Embedded Resource` as shown in the picture below:

![Properties of each file](http://i.imgur.com/rbmrZRc.png)

Then, add all of them to **References** since we're going to use them in our project as shown in the picture below:

![Add to References](http://i.imgur.com/U2PwPzY.png)

Next, we need to make sure all of our external DLL files also included in `Properties\Resources.resx` file as shown in the picture below:

![Include in Resources.resx](http://i.imgur.com/9orEOLE.png)

If nothing's there, click on **downward arrow** at the right side of **Add Resource** and select **Add Existing File...**, navigate to `lib` folder in our project, change file extension to **All File (\*.\*)** then select all of our DLL files.

![Add Resource](http://i.imgur.com/Ok7olQ8.png)

After we're done with these files setup, go to our main application file constructor code, for example in my case is `Form1()` from `Form1.cs` file as shown in the picture below:

![Constructor Code](http://i.imgur.com/Ap2RIJD.png)

Then, add this anonymous function code on the top of our application constructor, as my case, before `InitializeComponent();`:

```csharp
AppDomain.CurrentDomain.AssemblyResolve += (sender, args) =>
{
	string resourceName = new AssemblyName(args.Name).Name + ".dll";
    string resource = Array.Find(this.GetType().Assembly.GetManifestResourceNames(), element => element.EndsWith(resourceName));

    using (var stream = Assembly.GetExecutingAssembly().GetManifestResourceStream(resource))
    {
		Byte[] assemblyData = new Byte[stream.Length];
        stream.Read(assemblyData, 0, assemblyData.Length);
        return Assembly.Load(assemblyData);
	}
};
```

The above code requires `using System.Reflection;` namespace.

That's it! When we build/compile our application, on the output folder (e.g. `bin`), we should have our single .EXE application file without any external assembly (.DLL) file.

**Warning:** Please note that, embedding .DLL files into our main application .EXE file will result the our main application file size become bigger as it sum up all file size of each external .DLL file.
