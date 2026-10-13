# Insane Stunts in GTA 1 Coding

Source: https://projectcerbera.com/gta/1/tutorials/insane-stunts

This tutorial will show you how to make the `INSANE STUNT BONUS` features for you map. These are used in the original game extensively in San Andreas. No-one seems to have found most of them unless they actually looked round the map and mission file, like we will now.

The way they actually work is that there is a launch ramp and a catch ramp on the map. There is a launch `TRIGGER` and a catch `TRIGGER` object in the mission file too. The catch trigger is turned off when the level starts but when you drive on the launch ramp the catch trigger is enabled when You drive on the launch ramp. The way it does this is that the launch trigger enables the catch trigger, then waits for a few seconds, then turns off the catch trigger.

You have to get from the launch ramp to catch ramp within this time. If you crash, or decide to chicken out of the jump, the catch trigger will be disabled and the launch trigger enabled ready for a seconds try. If you reach the catch trigger before the launch trigger disables it again, you will get the `INSANE STUNT BONUS` and 50,000 points. Radical!

Now you may have noticed that you might be able to to drive to the lanch trigger, drive round whatever the jump is over, trigger the catch trigger and so get the bonus with ever doing the jump. This is indeed possible, but the timers are set very close to the time it willt take for a sucessful jump, and the launch or catch trigger will be put at the end of a ramp that you would have to drive up. So it is pretty impossible to cheat these jumps in reality.

If you noticed this congrats, you have the makings of an excellent mission coder - maybe even as good as me one day! c{8¬B

## Finding the Code

I will be using San Andreas as the example here as it has a few of these bonuses. Sure, you rember, there was that Penetrator (Porsche 911) parked up in north-east Richman that you cold jump over all that water. Open up the original `mission.ini` file using Notepad or a mission editor, `SanA.cmp` in Junction 25 1.6 and the original `English.fxt` file to view the text.

To locate these stunt bonuses, we will first have to locate the `INSANE STUNT BONUS` text, so search the `.fxt` file for:

```text
INSANE STUNT BONUS
```

And it will bring up this line:

```text
[2514]INSANE STUNT BONUS!
```

Now we search the mission code for to see when message `2514` is created. So search for:

```ini
BRIEF 0 0 0 0 2514
```

And it finds this chunk:

```ini
29000 DISABLE 10000 0 0 0 0
29010 DISABLE 10010 0 0 0 0
29020 GOTO 10010 0 29041 100 0
29030 IS_GOAL_DEAD 85 29041 0 0 0
29035 SURVIVE 0 0 0 40 0
29038 MESSAGE_BRIEF 0 0 0 0 2514
29039 SURVIVE 0 -1 0 0 50000

29041 ENABLE 10000 0 0 0 0
29042 ENABLE 10010 -1 0 0 0
```

Now we see the `DISABLE` is pointed to `10000` which must be a trigger, so we go up to the objects area of that chapter and find the object chunk around `10000`:

```ini
10000 1 (42,195,4) TRIGGER 29000 1
10010 1 (42,200,4) TRIGGER 29010 1
```

Marvelous! A takeoff `TRIGGER` and landing `TRIGGER`.

## Understanding the Code

Now to work through this code. The `DISABLE 10000` part means that it was `TRIGGER 10000` that started this procedure. We will bear that in mind.

Now a quick run through it. After `TRIGGER` numbers `10000` and `10010` receive the `DISABLE` command, it then does this odd-looking command:

```ini
29020 GOTO 10010 0 29041 100 0
```

Now what this actually does is a little odd to explain. Pronounce the command “Got To” not “Goto” because the command is testing to see if you “get to” `TRIGGER` number `10010` before `100` frames have been drawn. GTA draws 25 frames per second, thus it gives you four seconds to leap from the launch ramp to the catch ramp. This would not be long enough to drive over the jump trigger, drive round and then drive over the landing trigger before the timer ran out. Times like this you realise how much the GTA 1 mission coders cared about their work and how good their dedication made it.

If you do not get to the catch trigger before `100` frames have passed then it goes to command `29041`:

```ini
29041 ENABLE 10000 0 0 0 0
```

Now this may seem a bit weird, but what it is doing is to stop waiting for the player to reach the catch trigger and enables the launch trigger ready for you to try again. You probably noticed when playing that if you did not make it then you could try it again…once you had driven back there from the hospital!

But what if you do make it? Well if you go over the catch trigger within the four seconds, the command processor falls through to the next command in sheer delight:

```ini
29030 IS_GOAL_DEAD 85 29041 0 0 0
```

Now this checks to see if the `PLAYER` (object `85`) is dead. If he is, it goes to command `29041` which makes it all ready to start again. You may have noticed that when you have over-jumped it, like blown straight over the catch ramp and crashed into an ornamental pond (or similar) then you can still try it again. And if you do make it the command processor staggers triumphantly into the ‘success’ code:

```ini
29035 SURVIVE 0 0 0 40 0
29038 MESSAGE_BRIEF 0 0 0 0 2514
29039 SURVIVE 0 -1 0 0 50000
```

The `SURVIVE` line makes it wait for `40` frames, then it displays our good old friend message `2514`:

```text
[2514]INSANE STUNT BONUS!
```

Then the final `SURVIVE` line adds 50,000 points to your total (assuming ×1 bonus multiplier) and kills the routine with that sharp and pointy `-1` parameter.

Hopefully you now have some understanding of how these things work!

## Trying it Yourself

With this tutorial I am supplying a map, mission code AND the in-game text document for it. If you open the `mission.ini` file you may notice I do my code slightly differently by using a `DUMMY` rather than a trigger, this a marginally better way of doing it than they did.

I have made the first launch/catch ramp combination, and a slightly lower second launch ramp. What you have to do is make the catch ramp for the second one and place the `DUMMY` accordingly. Bear in mind that the end of the launch and catch ramps have to be flat!

And bear in mind that the more subtle your ramps are the more enjoyable they will be to find and use. Using stairwells up to a line of high buildings with their rooves at the same heights, especially with them all sloped, will create an imaginative jump which is a joy to come off. Also, using the superbike for a jump means that you can use the steepest slopes.
