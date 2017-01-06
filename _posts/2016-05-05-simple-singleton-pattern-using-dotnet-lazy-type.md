---
layout: post
title: Simple singleton pattern using .NET 4's Lazy type
description: This singleton pattern is very simple and perform well, best used with time-consuming operation class in C# programming.
keywords: singleton pattern, .net 4, laziness, lambda expression
tags: [Singleton, CSharp]
comments: true
---

Nowadays, most of the Microsoft .NET based applications are using .NET 4 or higher. When talking about singleton pattern, this way of implementation is my favorite - using the `System.Lazy<T>` type. All you need to do is pass a delegate to the constructor which calls the single constructor, which is done most easily with a lambda expression.

Let say I have a class called `SayHello.cs` with the singleton pattern:

```csharp
using System;

namespace SingletonExample
{
    public sealed class SayHello
    {
        private static readonly Lazy<SayHello> lazy = new Lazy<SayHello>(() => new SayHello());
        public static SayHello Instance { get { return lazy.Value; } }

        private SayHello()
        {
        }

        public void Test()
        {
            Console.WriteLine("Hello hello hello!");
        }
    }
}
```

And this is my main program that will execute a function from `SayHello.cs` class:

```csharp
using System;

namespace SingletonExample
{
    class Program
    {
        static void Main(string[] args)
        {
            SayHello.Instance.Test();

            Console.ReadLine();
        }
    }
}
```

Here's the output:

![SingletonExample](http://i.imgur.com/Vnbsea3.png)

This kind of singleton pattern is simple and perform well, best used with time-consuming operation class.
