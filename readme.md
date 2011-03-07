socleaf
=======

Introduction
------------
SOCLEAF is a different way of thinking, in terms of programming.

Many people lament the fact that computers aren't as smart as people, and often a "simple" problem for a human is a program-crashing bug for a program.

I looked at the reasons for this, and noted that we humans start out like this. We only know as much as we prepare ourselves for - what we forsee - and as such, anything that we don't quite understand or have knowledge for can also be a problem for us.

I think there are currently a few things that separate us from machines:

* We almost never do something the same way twice - it's our nature that we aren't exact enough to do this. But this is a blessing - in this we can, by accident or through experimentation, come up with a better or easier method of doing something. This leads to a continuous cycle of refinement that can't be done with programs.

* Whether we realise it or not, we have a vast array of possible solutions to many problems that we can attempt before we deduce that something is beyond our capabilities and that we can't proceed further. This is taught to us in general life lessons in a way that we rarely realise. One simply has to look at a baby unable to find the solution to pushing a square object into a square hole to see that life has taught us already a fundamental solution to this problem - rotate the object so that it fits. As children we learn this through experimentation, even it was an accident we came to the solution. Science is filled with many such "happy accidents" that lead to important scientific discoveries.

* We have others to ask to for help. This is a core theory to SOCLEAF, and goes hand in hand with:

* We rarely prepare for every single possible outcome. When you buy a car, or drive it for the first time, it is unlikely you know every inch of that car, what can go wrong, and what to do when it does. In this single way we can be likened to programs - we only know as much as we have forseen, guessed at, and prepared for. However, this is rarely a problem for us, as there is always a roadside-assistance service or mechanic we can call if things go really bad.

This brings me to the point of the introduction: Only once we realise how we differ, can we work towards something that brings us closer together.

The rest of this document will demonstrate real-world examples which closely resemble a typical programming problem, and how we can benefit from using the same approach a human would use to overcome issues.

A person's outlook
-------------------
To understand the points outlined above a bit better, let's look at things from a programmer, or program's, point of view.

I want to establish where humans and programs differ, so that I can propose ways that both can work in a similar fashion and lead to similar results.

First, let's look at how a person approaches a situation:

1. We identify an end-goal, in this example driving a car from point A to point B

2. We prepare ourselves for a list of known problems and solutions (this is a very simplified list):
> * If the fuel alert indicator comes on, we find the nearest fuel station and refuel
> 
> * If our car breaks down, we notify someone that can help (in this instance, our parents)

3. In the event of a problem, we choose the best solution:
> * See fuel alert indicator
>
> * Anything else, we'll notify our parents and ask for assistance

4. With these preparations in place, we're ready to embark!

A program's outlook
-------------------
When a person is writing a program, they usually start off with an end-goal, but the biggest problem is the large number of unforseen issues that can arise that we have no way of preparing for.

In these situations, the following is what usually happens:

1. We identify an end-goal, driving a car from point A to point B

2. We know that if we get low on fuel, that we should head to the nearest fuel station and refuel

3. Having identified the possible issues, we now assume that no further issues could arise (an over-generalisation, but common practice for those new to programming) and put nothing in to handle unexpected events

4. We're ready to embark (deploy) !

What's wrong with this picture?
-------------------------------
As a person identifying a common problem, you'll probably be quick to point out that the former approach covers a much broader range of problems and solutions than the latter approach, and allows for unforseen issues to be handled. I speculate that this is mainly due to the fact that you know about cars and the potential problems that can occur and perhaps some solutions to those.

Experienced programmers will also note these issues (having dealt with them before), and implement fault tolerance of a sort for known issues and for everything else, try/catch blocks, fail-on-error conditions, etc. But even with these in place, a relatively simple service could be unable to process data if a simple problem arises - perhaps the programmer never thought that data might come back in the wrong encoding format (a real-world example is web servers that insist the encoding format is UTF-32 when in fact it's ASCII, causing your conversion code to convert to invalid data.)

Your program then fails to read the data (since it's now meaningless gibberish) and returns an error code. Maybe the program didn't fail and cause loss of data, but the fact remains that it failed to properly handle the issue, something we humans can easily diagnose and find the correct solution for.

In all the cases I've participated in that has this sort of issue, I've continued to improve the way I handle it:

* At first, there was no handling - an error was generated and the program died.

* Later, we identified the issue and added try/catch handling or some other form of recovery.

* Later still, we again identified that sometimes what a web server says and what it delivers are not the same thing - so we now attempt conversion, but if the end result looks like nonsense, we abandon that and go with the original version.

The problem is that this all required human intervention over several iterations to identify and solve. And we were able to solve it because we instantly were able to identify the source input as ASCII when we viewed it ourselves.

So what can we do?
------------------
Perhaps one of the simplest things we can do, is notify someone and ask them for direction. As humans, we do this all the time.

If we're unsure of something, or need help operating something, the best solution is to ask someone who may or may not know more. They can supply us a few potential solutions:

* With the information we need - in which case we amend our processing methods accordingly

* Direct us to someone who may or may not know more (this could continue for a while of course)

* Tell us flat out that no known solution exists ("I don't know, and don't know anyone who does") in which case we can try other people, or wait for a solution to be produced

In all the programs I've worked with, none have taken this approach. At best, it's been "log the error so that someone can come along later and look at the error log to find out what happened. Then we might add more debugging to find out what to do."

One important step that is left out of the above that is useful, is a conversation.

If you report an issue to someone, they may instantly know the cause and solution, or it may require a bit more digging:

* A: There is a problem with XYZ

* B: Can you elaborate?

* A: Feature A of XYZ isn't working (debug dump)

* B: Can you give me a readout of M?

* A: Sure, it's FooBar (debug output of some sort)

* B: I suggest trying ZXY1

* A: That didn't work, and now I have a readout on M2 of FooBarNeedsUpgrade

* B: Try running /bin/UpgradeFooBar

* A: Readout M is now Upgrading

* B: Wait a bit

* A: XYZ seems to be working again!

* B: I'll notify the others to do the same thing so they won't run into this problem.

This is rather involved, but highlights a few things:

* Further conversion may be needed to fully identify a problem, past what was initially conceived - wheras the error reporting may simply be a debug dump of the error, we may need to also know the information that caused the request to fail in the first place. In current systems, we generally need to add that additional debugging to the code and wait for the problem to occur again, meaning it takes longer to identify and solve the issue if this process needs to be repeated a few times.

* We need the ability to do more than what was originally conceived - that is, run arbitary code on the server to give us more insight into what is occuring, and check some things to further identify the issue, or to attempt to solve it as it happens. This could be an upgrade script, service reboot, etc.

* The ability to reply with a "success" or "failure" notification to mark something as the required solution, and notify everyone else to do this so they won't run into it also.

In this way, a fault tolerent system can "call back home" to notify of issues, get more detailed reports, and if a solution is available, attempt it. If the solution works, then notify everyone else using that code to do that so they won't run into the issue.

What's your point?
------------------
So far, this reads very much like the typical way software is produced and maintained. But it also means that it can take several long iterations to finally identify, reproduce, and solve an issue.

However, much of this process can be automated, to a certain degree. Eventually, human intervention will be required, but if we can gather enough information through automated services then we can reach a solution quicker.

We can further reduce time it takes to resolve an issue if we stick to a method of creating simple functions and methods that do one thing, and do it well. These methods should be reused everywhere, rather than being incorporated into large functions.

Eventually, this should mean that a problem identified and fixed will mean it will never be a problem again, if programmed correctly.

This does mean we'd have to move away from the current often-used method of writing large functions that do a lot of processing and such, to a more centralized "database" of functions.

There are a few programming languages that lean towards this paradigm (my favourite being Erlang), and it is something often overlooked - especially by new programmers.

However, if implemented correctly, problems can be fixed in a simple manner that reverberates across everything that uses those functions.

This is however not in the scope of what I wish to discuss, though I may cover it later.

There are still ways we can work with the current common way of programming that does not utilize the aforementioned programming paradigms, though they are what I'd like to use later.

---
This article is unfinished, and I shall continue it later.