# Random Pager Messages in GTA 1 Coding

Source: https://projectcerbera.com/gta/1/tutorials/random-pager

This tutorial will show you how to make the random pager messages we all love from the original GTA 1 maps. The best way to start is to explain quite how they work.

There are a network of `TRIGGER` objects around the city with a range of four blocks. When the player runs over these, it starts a routine which waits for a couple of minutes, then makes the pager message. The reason it waits so long is so that you never really know quite where the pager message got triggered from, thus making them seem random. It is a neat feature.

## Finding the Code

I will be using San Andreas as the example here as it is pager messages are in capitals so are easy to find and bloody hilarious! So open up the original `mission.ini` file, `SanA.cmp` in Junction25 1.6 and the original `English.fxt` file to view the text.

To locate the text we will be working with, search the `.fxt` file (using an editor like FXTPad by Delfi) for:

```text
[2610]Man gives birth to dog. Shocked wife has kittens. Film at eleven
```

Now to find the `TRIGGER` that will set it off. We have to reverse-engineer this from the message back through the code to the object. It is a very common technique for finding out how the mission file ticks.

If you search the mission file for:

```ini
P_BRIEF 0 -1 0 0 2610
```

That will find the line that makes the pager show that message because `2610` is the message number. Easy.

If you look around that line you will see the code:

```ini
28300 DISABLE 9700 0 0 0 0
28301 KEEP_THIS_PROC 0 0 0 0 0
28310 SURVIVE 0 0 0 2500 0
28320 P_BRIEF 0 -1 0 0 2610
```

## Understanding the Code

Now to work through this code. The `DISABLE 9700` part means that it was `TRIGGER 9700` that started this procedure. We will bear that in mind.

The `KEEP_THIS_PROC` is used so that the pager routine will not get stopped. It would get killed if you complete a mission in this time because the end of the mission does a `RESET`.

The `SURVIVE 0 0 0 2500 0` makes the command processor wait there for `2500` frames. GTA runs at 25 frames per second, so we can do a bit of algebra:

```text
Let:
f = frames
s = seconds

So:
f ÷ s = f ÷ s
    s = f ÷ (f ÷ s)
```

From that algebraic rule we can do the arithemetic sum for this `SURVIVE` command:

```text
Let:
    f = 2500
    s = ?
f ÷ s = 25

So:
  s = f ÷ (f ÷ s)
  s = 2500 ÷ 25
100 = 2500 ÷ 25
```

To describe that sum in language:

```text
2500 frames being drawn at 25 frames per second takes 100 seconds.
```

The `-1` parameter in the `P_BRIEF` statement is put there to stop the command processor from falling out of the bottom of the routine. Not something you want to happen!

## Finding the Object

Now that we understand the code we can go have a look at where this object is. To search for this trigger sucessfully, we can take two approaches. We can search the whole document for its object number and hope that we can figure out which object `9700` it is, or we can search for this:

```ini
TRIGGER 28300 4
```

This is likely to work better because we only want trigger objects which point to line `38300` and also we know that the radius will be `4`.

Search for that and it will jump straight to the right line:

```ini
9700 (77,10,1) TRIGGER 28300 4
```

We see that its coordinates are `(77,10,1)` and if you go check this on the `SanA.cmp` map you will see this trigger is located in Richman and covers and area which only includes half the width of the east-west road. You will find this happen a lot, because the programmers figured that if you only use half the road then there is half the chance the palyer will get the trigger each time, thus prolonging the time to get through all the pager messages *and* increasing the randomness.

It is times like this you realise how much the GTA 1 mission coders cared.

## Try it Yourself

I know, this is perhaps the greatest moment of your life. You have realised that snazzy wireframe maps and lense flares are not the best thing about games, it is all about the dedication and inginuity of the coders! Well, perhaps not…

Here is the code you will need to get off the ground, you will have to customise the coordinates of objects and the line in the `.fxt` file that the `P_BRIEF` is set to display. I have set the timed delay to just one second.

Here is the code sample:

```ini
[1]
Pager Briefs, 22, yourmap.cmp, 0,
100 1 1 1 1 0

0 (0,0,0) DUMMY 0 0
1 1 (1,1,1) TRIGGER 10 255

100 (5,5,4) PARKED 04 768

{TRIGGER objects:}
1000 (126,16,4) TRIGGER 25000 10 {start commands from line 25000, radius of 10 blocks}
1001 (200,36,3) TRIGGER 2 25010 2
1002 (102,40,3) TRIGGER 4 25020 4
1003 (31,125,3) TRIGGER 25030 2
1004 (113,109,3) TRIGGER 25040 1
1005 (231,129,3) TRIGGER 25050 1
1006 (145,155,3) TRIGGER 25060 4
1010 (131,166,3) TRIGGER 25100 4
1020 (207,172,3) TRIGGER 25200 4
1021 (170,170,3) TRIGGER 25210 2
1022 (248,176,3) TRIGGER 25220 2
1023 (180,200,3) TRIGGER 25230 4

{Create the player:}
32100 1 (4,5,4) PLAYER 100 256 {owns car number 100, is facing East)

-1

0 DONOWT 0 0 0 0 0

10 STARTUP 1 0 0 0 0
12 MESSAGE_BRIEF 0 0 0 0 8001
98 DONOWT 0 -1 0 0 0

{Each of these code blocks is for one TRIGGER:}
25000 DISABLE 1000 0 0 0 0 {TRIGGER object number 1000}
25001 KEEP_THIS_PROC 0 0 0 0 0 {don't let anything stop me!}
25002 SURVIVE 0 0 0 250 0 {wait ~10 seconds}
25004 P_BRIEF 0 -1 0 0 2600 {display message number 2600 via the pager}
{Code block ends.}

25010 DISABLE 1001 0 0 0 0
25011 KEEP_THIS_PROC 0 0 0 0 0
25012 SURVIVE 0 0 0 25 0
25013 P_BRIEF 0 -1 0 0 2601

25020 DISABLE 1002 0 0 0 0
25021 KEEP_THIS_PROC 0 0 0 0 0
25022 SURVIVE 0 0 0 25 0
25023 P_BRIEF 0 -1 0 0 2602

25030 DISABLE 1003 0 0 0 0
25031 KEEP_THIS_PROC 0 0 0 0 0
25032 SURVIVE 0 0 0 25 0
25033 P_BRIEF 0 -1 0 0 2603

25040 DISABLE 1004 0 0 0 0
25041 KEEP_THIS_PROC 0 0 0 0 0
25042 SURVIVE 0 0 0 25 0
25043 P_BRIEF 0 -1 0 0 2604

25050 DISABLE 1005 0 0 0 0
25051 KEEP_THIS_PROC 0 0 0 0 0
25052 SURVIVE 0 0 0 25 0
25053 P_BRIEF 0 -1 0 0 2605

25060 DISABLE 1006 0 0 0 0
25061 KEEP_THIS_PROC 0 0 0 0 0
25062 SURVIVE 0 0 0 25 0
25063 P_BRIEF 0 -1 0 0 2606

25100 DISABLE 1010 0 0 0 0
25101 KEEP_THIS_PROC 0 0 0 0 0
25102 SURVIVE 0 0 0 25 0
25103 P_BRIEF 0 -1 0 0 2610

25200 DISABLE 1020 0 0 0 0
25201 KEEP_THIS_PROC 0 0 0 0 0
25202 SURVIVE 0 0 0 25 0
25203 P_BRIEF 0 -1 0 0 2620

25210 DISABLE 1021 0 0 0 0
25211 KEEP_THIS_PROC 0 0 0 0 0
25212 SURVIVE 0 0 0 25 0
25213 P_BRIEF 0 -1 0 0 2621

25220 DISABLE 1022 0 0 0 0
25221 KEEP_THIS_PROC 0 0 0 0 0
25222 SURVIVE 0 0 0 25 0
25223 P_BRIEF 0 -1 0 0 2622

25230 DISABLE 1023 0 0 0 0
25231 KEEP_THIS_PROC 0 0 0 0 0
25232 SURVIVE 0 0 0 25 0
25233 P_BRIEF 0 -1 0 0 2623

-1
```
