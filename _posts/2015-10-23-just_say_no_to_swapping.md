---
layout: post
title: "Just say no to swapping"
description: 
modified: 2015-10-23
category: Linux
tags: [Linux]
comments: true
mathjax: 
---
{% highlight linenos %}
{% endhighlight %}

Rencently，I find a great essay about swap from mikemccandless.
Original link:http://blog.mikemccandless.com/2011/04/just-say-no-to-swapping.html

Imagine you love to cook; it's an intense hobby of yours. Over time, you've accumulated many fun spices, but your pantry is too small, so, you rent an off-site storage facility, and move the less frequently used spice racks there. Problem solved!

Suddenly you decide to cook this great new recipe. You head to the pantry to retrieve your Saffron, but it's not there! It was moved out to the storage facility and must now be retrieved (this is a hard page fault).

No problem -- your neighbor volunteers to go fetch it for you. Unfortunately, the facility is ~2,900 miles away, all the way across the US, so it takes your friend 6 days to retrieve it!

This assumes you normally take 7 seconds to retrieve a spice from the pantry; that your data was in main memory (~100 nanoseconds access time), not in the CPU's caches (which'd be maybe 10 nanoseconds); that your swap file is on a fast (say, WD Raptor) spinning-magnets hard drive with 5 millisecond average access time; and that your neighbor drives non-stop at 60 mph to the facility and back.

Even worse, your neighbor drives a motorcycle, and so he can only retrieve one spice rack at a time. So, after waiting 6 days for the Saffron to come back, when you next go to the pantry to get some Paprika, it's also "swapped out" and you must wait another 6 days! It's possible that first spice rack also happened to have the Paprika but it's also likely it did not; that depends on your spice locality. Also, with each trip, your neighbor must pick a spice rack to move out to the facility, so that the returned spice rack has a place to go (it is a "swap", after all), so the Paprika could have just been swapped out!

Sadly, it might easily be many weeks until you succeed in cooking your dish.

Maybe in the olden days, when memory itself was a core of little magnets, swapping cost wasn't so extreme, but these days, as memory access time has improved drastically while hard drive access time hasn't budged, the disparity is now unacceptable. Swapping has become a badly leaking abstraction. When a typical process (say, your e-mail reader) has to "swap back in" after not being used for a while, it can hit 100s of such page faults, before finishing redrawing its window. It's an awful experience, though it has the fun side effect of letting you see, in slow motion, just what precise steps your email reader goes through when redrawing its window.

Swapping is especially disastrous with JVM processes. See, the JVM generally won't do a full GC cycle until it has run out of its allowed heap, so most of your heap is likely occupied by not-yet-collected garbage. Since these pages aren't being touched (because they are garbage and thus unreferenced), the OS happily swaps them out. When GC finally runs, you have a ridiculous swap storm, pulling in all these pages only to then discover that they are in fact filled with garbage and should be discarded; this can easily make your GC cycle take many minutes!

It'd be better if the JVM could work more closely with the OS so that GC would somehow run on-demand whenever the OS wants to start swapping so that, at least, we never swap out garbage. Until then, make sure you don't set your JVM's heap size too large!


##Just use an SSD...

These days, many machines ship with solid state disks, which are an astounding (though still costly) improvement over spinning magnets; once you've used an SSD you can never go back; it's just one of life's many one-way doors.

You might be tempted to declare that this problem is solved, since SSDs are so blazingly fast, right? Indeed, they are orders of magnitudes faster than spinning magnets, but they are still 2-3 orders of magnitude slower than main memory or CPU cache. The typical SSD might have 50 microsends access time, which equates to ~58 total miles of driving at 60 mph. Certainly a huge improvement, but still unacceptable if you want to cook your dish on time!


##Just add RAM...

Another common workaround is to put lots of RAM in your machine, but this can easily back-fire: operating systems will happily swap out memory pages in favor of caching IO pages, so if you have any processes accessing lots of bytes (say, mencoder encoding a 50 GB bluray movie, maybe a virus checker or backup program, or even Lucene searching against a large index or doing a large merge), the OS will swap your pages out. This then means that the more RAM you have, the more swapping you get, and the problem only gets worse!

Fortunately, some OS's let you control this behavior: on Linux, you can tune swappiness down to 0 (most Linux distros default this to a highish number); Windows also has a checkbox, under My Computer -> Properties -> Advanced -> Performance Settings -> Advanced -> Memory Usage, that lets you favor Programs or System Cache, that's likely doing something similar.

There are low-level IO flags that these programs are supposed to use so that the OS knows not to cache the pages they access, but sometimes the processes fail to use them or cannot use them (for example, they are not yet exposed to Java), and even if they do, sometimes the OS ignores them!


##When swapping is OK

If your computer never runs any interactive processes, ie, a process where a human is blocked (waiting) on the other end for something to happen, and only runs batch processes which tend to be active at different times, then swapping can be an overall win since it allows that process which is active to make nearly-full use of the available RAM. Net/net, over time, this will give greater overall throughput for the batch processes on the machine.

But, remember that the server running your web-site is an interactive process; if your server processes (web/app server, database, search server, etc.) are stuck swapping, your site has for all intents and purposes become unusable to your users.


##This is a fixable problem

Most processes have known data structures that consume substantial RAM, and in many cases these processes could easily discard and later regenerate their data structures in much less time than even a single page fault. Caches can simply be pruned or discarded since they will self-regenerate over time.

These data structures should never be swapped out, since regeneration is far cheaper. Somehow the OS should ask each RAM-intensive and least-recently-accessed process to discard its data structures to free up RAM, instead of swapping out the pages occupied by the data structure. Of course, this would require a tighter interaction between the OS and processes than exists today; Java's SoftReference is close, except this only works within a single JVM, and does not interact with the OS.


##What can you do?

Until this problem is solved for real, the simplest workaround is to disable swapping entirely, and stuff as much RAM as you can into the machine. RAM is cheap, memory modules are dense, and modern motherboards accept many modules. This is what I do.

Of course, with this approach, when you run out of RAM stuff will start failing. If the software is well written, it'll fail gracefully: your browser will tell you it cannot open a new window or visit a new page. If it's poorly written it will simply crash, thus quickly freeing up RAM and hopefully not losing any data or corrupting any files in the process. Linux takes the simple draconian approach of picking a memory hogging process and SIGKILL'ing it.

If you don't want to disable swapping you should at least tell the OS not to swap pages out for IO caching.

Just say no to swapping!