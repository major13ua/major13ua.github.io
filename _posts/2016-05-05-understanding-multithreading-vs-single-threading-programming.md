---
layout: post
title: Understanding multithreading vs single threading programming
description: Check out the sample source code of Windows Console application in C# demonstrating multithreading programming.
keywords: multithreading, single threading, sample source code, demonstration, threadpool, task, backgroundworker, windows console application
tags: [CSharp, Multithreading]
comments: true
---

Multithreading is a widespread programming and execution model that allows multiple threads to exist within the context of one process. They share the process's resources, but are able to execute independently while the single threading is the processing of one command at a time.

The purpose of threading is to allow computer to do more than one thing at a time. In a single core computer, multithreading won't give much advantages for overall speed. But for computer with multiple processor cores (which is common nowadays), multithreading can take advantage of additional cores to perform separate instructions at the same time or by splitting the tasks between the cores.

For example, check out the sample source code below written for Windows Console application in C# that will demonstrate multithreading programming vs single threading programming. You may copy-and-paste the source code into your IDE (eg. Visual Studio) to have a try.

```csharp
using System;
using System.ComponentModel;
using System.Diagnostics;
using System.Threading;
using System.Threading.Tasks;

namespace MultithreadingVsSingleThreading
{
    class Program
    {
        #region Fields

        static int selectedMode;
        static bool usingSingleThread;

        // The number of threads to be spawned.
        private const int threadCount = 1000;

        // The total number of spins the actual work is carried out repeatedly.
        private const int totalCount = 100000;

        #endregion

        #region Methods

        static void Main(string[] args)
        {
            Console.Title = "Multithreading vs Single Threading Example";
            start:
            usingSingleThread = false;
            Console.WriteLine("Select which mode to run:-");
            Console.WriteLine("  (1) Multithreading");
            Console.WriteLine("  (2) Single Threading");
            Console.WriteLine("  (3) Threadpool");
            Console.WriteLine("  (4) Task");
            Console.WriteLine("  (5) BackgroundWorker");
            var input = Console.ReadLine();
            if (input.Length == 0)
            {
                Console.Clear();
                goto start;
            }
            else if (Convert.ToInt32(input) == 0 || Convert.ToInt32(input) >= 6)
            {
                Console.Clear();
                goto start;
            }
            else
            {
                selectedMode = Convert.ToInt32(input);
            }

            Thread.CurrentThread.Priority = ThreadPriority.Highest;
            Stopwatch watch = new Stopwatch();
            Console.Clear();
            Console.WriteLine("Initiating work...");
            watch.Start();

            try
            {
                HandleMode(selectedMode);

                watch.Stop();
                Console.WriteLine("Work done!");
                Console.WriteLine("Time elapsed: {0}", watch.Elapsed);
                Console.WriteLine(Environment.NewLine);
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("Press Enter key to test again or Esc to quit");
                Console.ResetColor();
                var key = Console.ReadKey();
                if (key.Key == ConsoleKey.Enter)
                {
                    Console.Clear();
                    goto start;
                }
                if (key.Key == ConsoleKey.Escape) Environment.Exit(0);
            }
            catch (Exception e)
            {
                Console.WriteLine(e.Message);
            }
            Console.ReadLine();
        }

        /// <summary>
        /// Draws a graphical progress bar.
        /// </summary>
        /// <param name="complete"></param>
        /// <param name="maxVal"></param>
        /// <param name="barSize"></param>
        /// <param name="progressCharacter"></param>
        private static void DrawProgressBar(int complete, int maxVal, int barSize, char progressCharacter)
        {
            Console.CursorVisible = false;
            int left = Console.CursorLeft;
            decimal perc = (decimal)complete / (decimal)maxVal;
            int chars = (int)Math.Floor(perc / ((decimal)1 / (decimal)barSize));
            string p1 = String.Empty, p2 = String.Empty;

            for (int i = 0; i < chars; i++) p1 += progressCharacter;
            for (int i = 0; i < barSize - chars; i++) p2 += progressCharacter;

            Console.ForegroundColor = ConsoleColor.Green;
            Console.Write(p1);
            Console.ForegroundColor = ConsoleColor.DarkGreen;
            Console.Write(p2);

            Console.ResetColor();
            Console.Write(" {0}%", (perc * 100).ToString("N2"));
            Console.CursorLeft = left;
        }

        /// <summary>
        /// Performs the task based on user defined option.
        /// </summary>
        /// <param name="mode"></param>
        private static void HandleMode(int mode)
        {
            switch (mode)
            {
                case 1:
                    RunThreadMode();
                    break;
                case 2:
                    usingSingleThread = true;
                    ComplexWork(totalCount);
                    break;
                case 3:
                    RunInThreadPool();
                    break;
                case 4:
                    RunTaskMode();
                    break;
                case 5:
                    RunInBackgroundWorker();
                    break;
                default:
                    break;
            }
        }

        #region Work Task
        /// <summary>
        /// Performs CPU intensive task.
        /// </summary>
        /// <param name="n"></param>
        private static void ComplexWork(int n)
        {

            for (int j = 0; j < n; j++)
            {
                for (int i = 1; i < 100; i++)
                {
                    Fac(i);
                }

                if (usingSingleThread) DrawProgressBar(j, n, Console.WindowWidth, 'X');
            }

        }

        /// <summary>
        /// Calculates the factorial of a number.
        /// </summary>
        /// <param name="n"></param>
        /// <returns></returns>
        private static double Fac(double n)
        {
            if (n > 1)
            {
                return n * Fac(n - 1);
            }
            else
            {
                return 1;
            }
        }

        #endregion

        /// <summary>
        /// Spawns new threads based on the thread count and starts the activity.
        /// </summary>
        private static void RunThreadMode()
        {
            Thread[] t = new Thread[threadCount];

            for (int i = 0; i < threadCount; i++)
            {
                t[i] = new Thread(() =>
                {
                    ComplexWork(totalCount / threadCount);
                });
                t[i].Priority = ThreadPriority.Highest;
                t[i].Start();
                DrawProgressBar(i, threadCount, Console.WindowWidth, 'X');
            }

            // Waits for all the threads to finish.
            foreach (var ct in t)
            {
                ct.Join();
            }
        }

        /// <summary>
        /// Executes the task in a thread pooling context.
        /// </summary>
        private static void RunInThreadPool()
        {
            using (CountdownEvent signaler = new CountdownEvent(threadCount))
            {
                for (int i = 0; i < threadCount; i++)
                {
                    ThreadPool.QueueUserWorkItem((x) =>
                    {
                        ComplexWork(totalCount / threadCount);
                        signaler.Signal();
                    });
                    DrawProgressBar(i, threadCount, Console.WindowWidth, 'X');
                }
                signaler.Wait();
            }

        }

        /// <summary>
        /// Creates a new task based on the TPL library.
        /// </summary>
        private static void RunTaskMode()
        {
            Task[] taskList = new Task[threadCount];
            for (int i = 0; i < threadCount; i++)
            {
                taskList[i] = new Task(new Action(() =>
                {
                    ComplexWork(totalCount / threadCount);
                }));
                taskList[i].Start();
                DrawProgressBar(i, threadCount, Console.WindowWidth, 'X');
            }
            Task.WaitAll(taskList);
        }

        /// <summary>
        /// Starts BackgroundWorker to perform the same action.
        /// </summary>
        private static void RunInBackgroundWorker()
        {
            BackgroundWorker[] backgroundWorkerList = new BackgroundWorker[threadCount];
            using (CountdownEvent signaler = new CountdownEvent(threadCount))
            {
                for (int i = 0; i < threadCount; i++)
                {
                    backgroundWorkerList[i] = new BackgroundWorker();
                    backgroundWorkerList[i].DoWork += delegate (object sender, DoWorkEventArgs e)
                    {
                        ComplexWork(totalCount / threadCount);
                        signaler.Signal();
                    };
                    backgroundWorkerList[i].RunWorkerAsync();
                    DrawProgressBar(i, threadCount, Console.WindowWidth, 'X');
                }
                signaler.Wait();
            }

        }

        #endregion
    }
}
```

### Screenshot

![Multithreading vs Single Threading Example](http://i.imgur.com/YsFg92A.png)

_Figure 1 (above): A Windows Console app demonstrating multithreading vs single threading programming._

### Footnotes

The `Thread` class is used for creating and manipulating a [thread](http://msdn.microsoft.com/en-us/library/windows/desktop/ms684841%28v=vs.85%29.aspx) in Windows.

A `Task` represents asynchronous operation and is part of the [Task Parallel Library](http://msdn.microsoft.com/en-us/library/dd460717%28v=vs.110%29.aspx), a set of APIs for running tasks asynchronously and in parallel.

The `ThreadPool` class manages a group of threads in which tasks are added to a queue and automatically started when threads are created.

The `BackgroundWorker` class executes an operation on a separate thread.
