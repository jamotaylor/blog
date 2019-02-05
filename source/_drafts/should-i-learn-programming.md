---
title: should-i-learn-programming
---

### I get a lot of people asking me if they should learn to program. Or sometimes, they've already decided it's important, and are asking how to get started.

This article serves as an answer to that question - SO THAT I NEVER HAVE TO ANSWER IT AGAIN!!!  Jokes, I really don't mind the INCESSANT QUESTIONING!!!

I know what it's like to be the guy thinking that coding is something I should probably know how to do.  Coming from an Industrial Design background, I had always been focussed on getting really good at the software I was using - 3D modelling and 2D drawing software - without really thinking about the need to make software myself.  Eventually, towards the end of my studies, I decided that the 3D modelling software that we used was just not good enough - and I set about designing a new 3D modelling program - this essentially involved filling up sheet after sheet of A2 drawing paper with mind-maps and drawings of the UI and the viewport - my idea was to replace the mouse with a stylus - that would allow you to draw the contours of a 3D object instead of manipulating bezier curves until you finally got the desired shape. I still think this is a pretty cool idea - maybe one day I'll be lucky enough to get it done.

I think a lot of us get this idea into our heads that coders are essentially the rock stars of our society and that, if only we had those skills, we too could build a successful tech startup or finally realise those ideas we have in our heads about the perfect app.  

As someone who essentially switched careers at 30 - I can tell you that if you are going to learn to code - 

If you are going to learn to code - and you want to be really good at it - then you're going to end up as a coder. If you want to learn to code to help you with your current career - then you don't need to know that much. 


I also got really into the idea of doing user-interface design.  For one of my University projects.

 the first time I really needed to code was to help me to automate design tasks or have more control

*This is Italic*

### Let's get down to raw data - because that's what programming is. It's just data that get's wrapped in convention and standards.  

As you've probably heard a thousand times, it all comes down to binary - because that's all a computer knows (except quantum cmputer but I don't even know where to start on that topic). So another way to put it is, it all comes down to **numbers**. Binary just happens to be the only number system that computers can work with.

Let's do into the core of the computer, way down to it's brain, it's processor, it's Central Porcessing Unit, or CPU for short. At this very low level (and this is vastly simplified), a computer works like this:

There are 3 boxes that can hold information: 2 boxes for input, and another box for output. You put 2 values in in 2 of the boxes, then the computer does a calculation and spits out a 3rd value - the result - into the 3rd box. CPUs are specially designed to do calculations with binary numbers - they can add, subtract, multiply and do some other things like XOR and OR operations that I won't go into here - but the basic principle is that all they can do is take 2 numbers, do a calculation, and output a result.  These caclulations happen so fast, that the limitation of only being able to calculate 2 numbers at a time, is not really much of a limiation. A Pentium 3 - which was the most awesome thing to have when I was 15 - can do [2,054,000,000 instructions per second](https://en.wikipedia.org/wiki/Instructions_per_second) (2,054 MIPS). So adding up the ages of every person in the world on a Pentium 3 would take 0.04 seconds. These days, our laptops are processing at 34??? times that speed.

Ok, so computers are very fast, but they are still doing these tiny calculations and each 'core' of a CPU can only do one caculation at a time. It's kinda like everything in life - don't let the speed, size and complexity of it all overwhelm you - just remember it comes down to taking it one step at a time ;P

So how do the binary numbers get to the CPU so that the CPU can do his work? Well that an explanation for another day.

For now, just realise that everything on a computer has to eventually become a number. Everything. From pictures to music to videos to video games to Google Maps directions.  So you already knew this, right, but I'm just reminding you.

And these numbers are processed 1 instruction at a time.

Now imagine if, when i wanted to do something simple on a computer -  like adding up the ages of the world's population - that i had to type out a list of every age on the planet, then load each age in turn into the input, then tell the computer to add that age to the last answer, then load the next age, and so on and so on...  At that point it would actually just be easier to use a calculator. This is where programming languages come in.  They take the raw data - the numbers that the computer needs to do it's job - and wrap it in what we call objects.  Hence the name: 'Object-Oriented Programming', which i'm sure you've heard of before. (Not all programming languages are object-oriented - the really "low-level" languages are involved with giving instructions to the machine, and object-oriented languages are built on top of them.)

As we move away from the 3 boxes down at the CPU level - programming languages become more and more *abstract* - meaning they become less and less like the numbers and the instructions that the CPU understands, wrapping the data in more and more "layers". Let's take C# as an example:

C# code is written by human --> C# built and compiled to MSIL --> MSIL Compiled at runtime by CLR into C++ --> C++ Complied at runtime into assembler --> assembler compiled at runtime into machined code --> instructions given line by line to CPU


```graph
A[Square Rect] -- Link text --> B((Circle))
A --> C(Round Rect)
B --> D{Rhombus}
C --> D
```