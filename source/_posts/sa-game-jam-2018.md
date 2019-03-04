---
title: SA Game Jam 2018 - Cannon Bowls
date: 2019-02-25 20:45:34
tags:
categories:
---

Last year September, I managed to get out the house for the weekend and participate in the [SA Game Jam 2018](http://makegamessa.com/discussion/5375/sa-game-jam-2018)   (thanks to my amazing wife for looking after our kids). What is a [game jam](https://en.wikipedia.org/wiki/Game_jam)? Basically when people get together, either physically or across the wires, to make games together - video games, board games, whatever tickles your fancy.

If you want to skip all the detail and get to the juicy bits with a **video** and **stuff that I learnt**, then [look no further](#The-Finished-Entry).

Someone from the [Make Games SA forum](http://makegamessa.com/) organized a venue for the weekend.  It was on the 25th and top floor of an old apartheid-era office building composed of face-brick and concrete, that looks like it was once a fortress protecting government secrets, but is now occupied by a few hipster startups - like the friendly dudes that hosted us - thank you [New Reality](https://newreality.co.za/) for all the coffee and beer!!!

{% asset_img 25-owl-street-view.jpg No 25 Owl Street View %}
> There was a great view awaiting if you needed to get out for some air.

We arrived on Friday from all corners of Johannesburg and proceeded to meet people who we may or may not have bumped into on the forums. The theme was announced at 6pm: Escalating Possibilities, and we all set about wracking our brains and offering strong opinions on our interpretations (and why our interpretation was the right one). The game jam hosts - [Free Lives](https://freelives.net/) - released a [not-so-short video clip](https://youtu.be/Kr9ndYeE_s4) to explain the theme if you want to check it out here.

{% asset_img 25-owl-street-interior.jpg %}
> The entrance and the office interior - there weren't a ton of us there, but that made for a nice vibe.

A bit later, someone had the smart idea of having a little pow wow to introduce ourselves and our skills, and discuss our burgeoning ideas. I had initially planned to rock up at the venue and form a team with some other peeps, but there didn't seem to be anyone available that had the skills I needed (this turned out to a be a mistake), so I decided to soldier on alone.

## Development begins... 

Having worked as a creative for many years, I know that idea generation can suck up a lot of time if you're not careful - there's nothing wrong with exploring ideas, but when you've got a tight deadline and nothing new is coming to you, it's usually best just to go with your first idea, which, after a couple hours of brainstorming and inspiration seeking, is what I did.

My basic idea was to have some sort of bomb that you could launch at enemies.  Properties relating to the explosion of the bomb, the physics of the bomb capsule and launching the bomb - such as force, bounciness, weight, angle, power, etc. - would evolve as the game continued.

At some point the bomb became an exploding bowling ball launched from a cannon and enemies became bowling pins. What I loved most about this idea was that it took a violent concept and made it quite benign (I'm not a big fan of violent games), it also immediately gave me a title - **Cannon Bowls!**

The bowling idea came from a friend that I met at the jam, and this is where I began to learn the lesson that teams are better for these things.  My new friend was not only full of good ideas and feedback on the game, he also ended up modelling up the cannon and the bowling pins for me, and sourcing and tweaking all the sounds (more on sound later).

I ended up using a wave-based survival mechanic for progression in the game. After surviving each timed wave you can choose a random "upgrade" which changes one of the aforementioned properties. You survive the wave but not letting any enemies (bowling pins) past the line where you cannon if firing - however this functionality didn't ever actually make it into the game, and so you literally cannot lose!

Properties like *Tilt* and *Power* are applied to the cannon and allow the player to control those aspects, whereas properties like *Bounce* and *LandMine* change the way the bomb behaves. The order in which you receive the upgrades determines the order in which they interact.

I liked the random rewards so that players had a different game experience in each play-through, but because a play-through ended up being quite long (about 15mins), this idea didn't really work. Most people would only play the game through once - if they even finished it at all.

[//]: # (Gif of random color-changing buttons.)

One of the funnier things that ended up happening is that my overpowered cannon would flatten about 1000 skittles and all these skittles would then lie around on the floor, making it impossible to actually play the game. I had to do some clever but terrible code to allow standing skittles to push past "dead" ones. Then, at the last minute I had to implement *The Amazing Rotating Floor* to clear all the skittles from the previous wave. I quite like how this looks, it ended up giving the game a little character.

{% asset_img bowling-pin-mess.jpg %}
> Bowling pins filling up the floor space

[//]: # (Include pics of cluttered floor and rotating floor gif.)

From the moment I started coding it was hard to go to bed - partly for fear of not finishing, and partly because it was so rewarding to see the progress.  I spent about 43 (check hours) hours that weekend developing the game - and I captured my screen throughout the process. Here is the video of my development - which can be a bit boring in parts... at least I find it interesting 😉

{% youtube 4E9M1mzXz18 %}

----
<br>

## The Finished Entry

After all that, I didn't make the Sunday evening cut-off, and so the following day, amidst watching the kids in the garden, changing nappies and etc., I did manage to submit a 3-day entry with a [playable version of the game](https://jamotaylor.itch.io/cannon-bowls-sa-game-jam-2018).

Here is a short gameplay video of the final thing:

{% youtube OsUEctoJolU %}

----
<br>

## What I Learnt

### 1. Don't be fancy, be fast

Initially I tried to design an amazing bomb property applicator class using an inheritance hierarchy and an abstract factory and a whole bunch of fancy programmery stuff. That was a terrible waste of time because I couldn't resolve the design in time and I kept coming up against the Unity class structure.

In the end, I decided to embrace Unity's architecture and just duplicate a bit of code, and lose some type-safety, and drop the whole factory idea - which really isn't so bad when you're doing a game jam.  If I had done this from the beginning, I might have saved myself 6 hours or even more.

So, don't be fancy, embrace the limitations of your chosen engine or framework and focus on producing fast code, not good code. This goes against all my principles, but represents the tension between the urgency of functionality versus the importance of structure, and it was a good exercise for me to have to focus on the former.

### 2. Set up for success

Don't try to create a spawning pool at the jam. Don't try to create a sound manager at the jam. Don't try to create a UI framework at the jam. Of course not - who would be so ridiculous? Well, me.

There is nothing wrong with using 3rd party tools or code that you have written previously. No one expects every bit of code in your game to be written at the jam - if we were to apply that logic, then game engines would be out too, and we'd all end up producing very bad versions of pong in a weekend. That said, it takes some time and preparation to gather these tools and check that they are working before the jam starts.

**I would recommend setting up the following beforehand:**

1. **Update your game engine of choice**
2. **Create a basic startup project** - this should have:
    * a basic UI system with at least a main menu and an exit button
    * a spawning pool
    * a character with a control system
    * a couple of camera systems - 2D, first person, 3rd person etc.
    * a sound manager (possibly even some generic sounds as placeholders)
    * a few simple visual effects (that can be updated with custom artwork)
3. **Test that builds work** - make sure that you can build to your desired platform/s and that the any 3rd party libraries that you're using don't break.
4. **Create a web page** that you're going to be publishing to (such as [itch.io](https://itch.io)) - that's assuming you're publishing online. You can add details of your team and the jam itself even before you know the theme.

Whether you end up using all the parts of you startup project or none of it doesn't matter. If you're using a game engine like Unity can could take this quite far and have a basic 2D template and a basic 3D template with some basic geometry, tags and layers set up already.

Bear in mind that any third-party or previously created assets are most likely going to disqualify from a prize in that category, but as long as you state it in your entry, it should be fine to use whatever royalty-free assets you can find.

### 3. Balance and feeling

Leave time to balance and tune your game. This is a fine art and one that I have only scraped the surface of - the big studios take balance very seriously (even more so since professional e-sports), using all kinds of mathematics that goes above my head.  I should probably have left 4 hours or more at the end just to work on this, playing through the game over and over, tweaking parameters and polishing the experience here and there - and this is what I would recommend in a 48 to 72-hour jam.

<!-- ## Hook up and have fun

Don't be too protective of your ideas or your project - be that code or artwork. You have a much better chance of putting up something in time, and having fun doing it, if you work in a team - and you might make a buddy in the process. In my case, I went solo partly to prove a point to myself that I could do it all from artwork to code to finished product. But, seeing as I ended up needing help anyway, I now see that working in a team would probably have been a lot more fun if I had just had let go a little. -->

## // TODO

There's a lot that I would still love to polish in the game - first and foremost the horrible menu that had to go in as is 🤦‍, and then it needs some serious juice with the art and effects... oh and the sound is so horrible - should probably just remove that! What I need is a follow-up game jam - an uninterrupted 48 hours to dive in again. 😎

I think the concept has potential - the most positive feedback I got was that it was fun to play and people genuinely didn't want to stop... until they stopped and went to look at someone elses game. I can see it being quite fun as a casual game on tablet.

If you love to code but you've never done a game jam before, I highly recommend it. And if you've never made a game before, then why not rock up at the meetup and work through a start-to-finish game tutorial such as the ones Unity make available ([like this](https://unity3d.com/learn/tutorials/s/roll-ball-tutorial), [and this](https://unity3d.com/learn/tutorials/s/space-shooter-tutorial), [and also this](https://unity3d.com/learn/tutorials/s/survival-shooter-tutorial)). That way, you have some more experienced game developers around to help if you get stuck. You can even submit your finished tutorial if the rules allow it.