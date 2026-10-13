# Specifics

## Powerups

Welcome to the powerups tutorial. In this we shall be talking through different powerups, and creating a respawning powerup like the ones you get in the original GTA. Firstly I've setup a basic mission file;

```
10 (52,32,1) PLAYER 0 0
15 (52,32,1) TRIGGER 1000 0
-1

0 DONOWT 0 0 0 0 0
1000 STARTUP 15 20000 -1 0 0

-1
```

If you don't understand this code, then I suggest you take a look at the [7 Days tutorial](./7days-guide.md) that will take you through these basics. Line 1000 is run straight away, as there is a trigger on the square the player stands on that goes to line 1000. The `STARTUP` line will jump straight to line 20000, where we'll turn on all our `POWERUP`s.

There are loads of powerups, each with it's own numerical value. There's the four guns plus 6 powerups and the info sign. Here's a list of them all and their respective number;

- 1 - Pistol
- 2 - Machine Gun
- 3 - Rocket Launcher
- 4 - Flame Thrower
- 5 - Empty Box
- 6 - Speedup
- 7 - Empty Box
- 8 - Empty Box
- 9 - Police Bribe
- 10 - Armour
- 11 - Multiplier Up
- 12 - Get out of jail free card
- 13 - Extra Life
- 14 - Info sign (no crates)
- 15 - Extra Life
- 16 - Empty Box

All but the info sign are stored in crates. To create a powerup, you need to define the powerup using the `POWERUP` line in the declarations, then turn it 'on' using the `POWERUP_ON` line in the mission code. Here's an example;

```
2000 (50,32,1) POWERUP 1 49
```

The `1` represents the the powerup type - in the case, a pistol. The second variable can represent lots of things. It the powerup is a gun (powerups 1 to 4) then it represents either the ammo when you pick it up (maximum of 99) or, if greater than 99, a timer used for kill frenzies. Put it on 100 and the player will get infinite bullets. We won't go in to the timer thing yet - that's for another tutorial.

If the powerup is number `14`, an info sign, then the number represents which 'help' line in the FXT file it will display when they player gets it. So if you use;

```
2000 (50,21,1) POWERUP 14 5
```

Then is will display a help sign that, when you get it, will display the message `[help5]` that's in the FXT file.

If you use any other powerup, then the second parameter can be left as `0`.

Now what we have to do is turn 'on' the powerup. Without doing this, the powerup wouldn't 'exist' in the game. It's simply a case of using the `POWERUP_ON` line;

```
20000 POWERUP_ON 2000 0 0 0 0
29999 DONOWT 0 0 0 0 0
```

Line 20000 just turns on the powerup in line 2000, and line 29999 stop the flow of code. This leaves lots of space for more powerups in the future. Now we've done this, test out your mission. You'll have a working powerup!

What would be good now is to create a respawn powerup, like they've got in the real games. Note that when you respawn, there's usually a pistol waiting to be picked up. This is because the `POWERUP` is recreated after a short time limit, so that when you respawn in a hospital or police station then there's a gun waiting for you. We'll do the same with our powerup. Firstly, we need a trigger to go off when the player gets the powerup. Unfortunately there's not a specific trigger when the player collects a powerup - but what we can do is do a `TRIGGER` when the player is on the same square as the powerup. We've done this with;

```
2001 (50,32,1) TRIGGER 30000 0
```

This will run line 30000 once the player gets on co-ordinates `(50,32,1)`. Although this doesn't mean the player has actually taken the powerup (because they're on crates you could have walked over it and not punched the crate, thus not taking the powerup) it's a good start. We then do the code at line 30000. This is the code I've used;

```
30000 DISABLE 2001 0 0 0 0
30001 IS_POWERUP_DONE 2000 0 30005 -1 0
30002 SURVIVE 0 0 0 500 0
30003 GENERAL_ONSCREEN 2000 30003 0 0 0
30004 POWERUP_ON 2000 0 0 0 0
30005 ENABLE 2001 -1 0 0 0
```

The first thing you should do with all triggers is to disable them - otherwise they'll keep retriggering. Line 30000 simply disables the trigger in line 2001. We then check to see if the powerup in question is done. If it's not, then we want to quit this procedure, but if it is we want to do the respawn code. `IS_POWERUP_DONE` checks at that moment whether or not the powerup in line 2000 is 'done' - i.e. has it been taken. It if has, then it goes to success - the '0' means go on to the next line. If it's not done, then we go to line 30005, that re-enables the trigger and stops the procedure. The procedure will keep triggering until the powerup is done, and we get to line 30002.

We then want to wait for a while. We don't want the powerup to respawn straight away - that'd just look silly. `SURVIVE` halts the execute of code for a certain number of 'ticks' - in this case 500. There are exactly 25 'ticks' in a second, so 500 is 20 seconds. In the original game they seem to vary between 500 and 1000 of these ticks - 20 to 40 seconds. Depending on how hard to you want your map, chose a number that suits you.

Once the `SURVIVE` is up, I use a cunning little peice of code in line 30003. This isn't exactly compulsorary (they don't use it in the original, I made this up myself :P). Basically it sets up a loop that continues to loop around line 30003 until the player can't see the powerup. That way, it won't suddenly 'appear' on the game screen. `GENERAL_ONSCREEN` checks to see if the player and object 2000 - our powerup - are on the screen together. If they are, then it goes to success - in this case 30003. Therefore it'll keep running line 30003 until it goes to fail - until the powerup is offscreen. Once they're offscreen then it goes to line 30004, that simply turns back on the powerup. Line 30005 reenables the trigger so that it will do it all again when the player comes back to take the powerup.

There ya go - one working respawning powerup. But what's that? You want to make it more complicated for yourself? You want it so it will only respawn a set number of times, and then it wont respawn again? Well ok... if you insist....

For this we'll need a counter. For an introduction on counters, see Day 5 of the mission code tutorial. We'll setup a counter in the declarations with a starting value of `0`;

```
2002 (0,0,0) COUNTER 0 0
```

Now what we want to do is increment this counter every time it respawns. When the counter gets to 3, we want to stop enabling the trigger, so it can't respawn again. Take a look at the new code;

```
30000 DISABLE 2001 0 0 0 0
30001 IS_POWERUP_DONE 2000 0 30006 -1 0
30002 SURVIVE 0 0 0 500 0
30003 GENERAL_ONSCREEN 2000 30003 0 0 0
30004 POWERUP_ON 2000 0 0 0 0
30005 INCCOUNT 2002 -1 0 3 0
30006 ENABLE 2001 -1 0 0 0
```

The main change is inclusion of line 30005. This line does two things. It first increments the counter in line 2002. It then compares the counter with 3. If it equals 3, it goes to success - if it doesn't equal 3, it goes to fail. The -1 stops the code carry on - this means that the trigger won't re-enable. This occurs when the counter equals 3. Otherwise, it goes on to line 30006 to re-enable the counter.

Hey presto - one respawning powerup that has a set limit of respawns! Feel free to use this in your code, or play around with it to suit your needs. You'll need a powerup, a trigger, a counter, a powerup_on and new set of lines 30000-30006 for each powerup you want to respawn. 

## Kill Frenzies

This section will teach you all you need to know about putting kill frenzies into your game. There are 3 main types of kill frenzies, each progressively difficult and each with their own tutorial;

### Gun Kill Frenzies

Your vanilla kill frenzy, usually with a machine gun, rocket launcher or flame thrower. Get as many points within the set time limit.

In this tutorial we'll be creating a basic kill frenzy - a set time limit to get said number of points with unlimited ammo. To understand this code we will presume that you can do all that's mentioned in the 7 day mission coding tutorial and the powerups tutorial.

We're going to write machine gun kill frenzy, that gives you 80 seconds to do $1500 of damage. We start with the basics as usual;

```
10 (52,32,1) PLAYER 0 0
15 (52,32,1) TRIGGER 1000 0

2000 (50,32,1) POWERUP 2 2100

-1

0 DONOWT 0 0 0 0 0

1000 STARTUP 15 20000 -1 0 0

20000 POWERUP_ON 2000 0 0 0 0
29999 DONOWT 0 0 0 0 0

-1
```

Lines 10 and 15 are just setting up the player and the start trigger for the game. This launches the mission code at line 1000 - our `STARTUP` command (all this should now be familier). Line 2000 sets up the kill frenzy machine gun. The 2 represents the machine gun. The 2100 represents the 'survive ticks' that the gun lasts for. Also recall that for numbers less that 100 it represents the ammo in the gun. We want an 80 second timer, so we do 80x25 = 2000, then add 100 to it. Make sense?

Line 1000 jumps to 20000, where we turn on all our `POWERUPS`. Line 2000 simply turns on your powerup in question, and 29999 stops the flow of code.

Note that even at this stage when you get the machine gun it sets up the timer, gives you infinite ammo and removes the gun when the timers up. What we now need to do is start a thread when the player gets the powerup, using a `TRIGGER`;

```
2001 (50,32,1) TRIGGER 30000 0
```

This jumps to line 30000 when the player gets the co-ordinates 50,32,1 (remember, this is not specifically when the player gets the powerup). The thread at line 30000 needs to setup the killfrenzy, then launch 3 checks; check to see if the frenzy has been completed, check to see if the time is up, and check to see if they die or get arrested. Here's the first set of code;

```
30000 DISABLE 2001 0 0 0 0
30001 IS_POWERUP_DONE 2000 0 30009 -1 0
30002 MESSAGE_BRIEF 0 0 0 0 2503
30003 SURVIVE 0 0 0 5 0
30004 FRENZY_BRIEF 0 0 0 0 3514
30005 FRENZY_SET 30010 0 -1 -1 0
30006 KICKSTART 30010 0 -1 -1 -1
30007 KICKSTART 30020 0 -1 -1 -1
30008 KICKSTART 30030 -1 -1 -1 -1
30009 ENABLE 2001 -1 -1 0 0
```

As always, the first thing we do is disable the trigger. Then we check to see if the powerup is done - if it is, then carry on with the code - if it's not, re-enable the trigger and stop until the trigger is hit again. Line 30002 then displays the KILL FRENZY big message, along with the speech. A quick wait (line 30003) and we display the small message - get $1500 in 80 seconds. Note the use of `FRENZY_BRIEF` here instead of just `BRIEF` - the only difference is it displays the skull-and-crossbones image instead of a mobile phone.

The MTD defines `FRENZY_SET`; "Sets the score for this player in the correct place in line. This should be a `FRENZY_CHECK` or else strange things will happen….". Indeed. What it does is 'take a note' fo the current player score, so it knows when you've got a certain number of points, storing it in the FRENZY_CHECK line, hence the 30010 (We haven't created yet).

Once that's done we kickstart our 3 processes. Line 30008 stops the flow of code with the first -1. Our first check, in line 30010, checks for the player reaching the correct score;

```
30010 FRENZY_CHECK 1000 0 30010 0 50000
30011 DROP_WANTED_LEVEL 0 0 0 0 0
30012 MESSAGE_BRIEF 0 0 0 0 4000
30013 STOP_FRENZY 2000 0 0 0 0
30014 KILL_PROCESS 30007 0 0 0 0
30015 KILL_PROCESS 30008 0 0 0 0
30016 RESET_KF 0 -1 -1 0 0
```

`FRENZY_CHECK` is a check type command. It checks to see that at that given moment if the player has exceeded increased their start score by, in this case, 1000. If it has then it goes to success (in this case, the next line). If they are below, then it goes to fail. Our fail is line 30010, to create a loop around the `FRENZY_CHECK` line. If this is complete then it also adds the score - in this case I've added 50000.

(Note that you may choose not to add a score, but give another bonus, such as an extra life)

We then drop the wanted level (line 30011) and display the “FRENZY COMPLETE” message (line 30012). Line 30013, `STOP_FRENZY`, removes the gun and timer defined at line 2000, our powerup. We then kill the other two processes. `RESET_KF` just removes any kill frenzy processes still running.

The rest of the code should be quite self explanatory;

```
30020 SURVIVE 0 0 0 2000 0
30021 MESSAGE_BRIEF 0 0 0 0 2504
30022 STOP_FRENZY 2000 0 0 0 0
30023 KILL_PROCESS 30006 0 0 0 0
30024 KILL_PROCESS 30008 0 0 0 0
30025 RESET_KF 0 -1 -1 0 0

30030 DEAD_ARRESTED 10 0 30030 -1 0
30031 MESSAGE_BRIEF 0 0 0 0 2504
30032 STOP_FRENZY 2000 0 0 0 0
30033 KILL_PROCESS 30006 0 0 0 0
30034 KILL_PROCESS 30007 0 0 0 0
30035 RESET_KF 0 -1 -1 0 0
```

These are the other two processes that we're checking against. The first one waits for the time to be up, and the second is a loop checking to see if the player is dead or arrested. In both cases we display the “FRENZY FAILED” message (which is accompanied with that annoyingly churpy “better luck next time!” sfx), stop the frenzy process, kill the other two checks and do the reset command.

And there you have it - one fully working kill frenzy!

### Car Kill Frenzies

Most commonly with the tank. Get a set number of points within a certain time limit.

Creating a kill frenzy in a car is extremely similar to a powerup, so we'll be taking the code we made in the previous tutorial and modifying that. Hopefully through looking at the code you should understand the changes we've made. The first change is changing the powerup line to a `PARKED` line, and changing the `TRIGGER` to a `CARTRIGGER`;

```
2000 (50,33,1) PARKED 37 256
2001 (50,32,1) CARTRIGGER 30000 2000
```

`37` represents the tank. In the original I think they use 2 vehicles for the car kf's - tanks and monster bugs. Don't be afraid to play around and use different vehicles. The advantage with using a tank is when the kill frenzy is started, the tank has the grenade launcher attached - there's no other way in the code to get this working.

The first change to the mission code is the timer. Before, when making a gun kf, we didn't need to specify a timer, because guns have them built in. Because we're doing it inside a car, we have to start one ourselves;

```
30001 KF_BRIEF_TIMED 0 0 0 80 3524
```

`KF_BRIEF_TIMED` does a slightly different kind of timer to a normal `P_BRIEF_TIMED`. This timer displays a countdown in the top left hand side - not in a timer. We need to cancel this timer in all the checking threads too, using;

```
30014 KF_CANCEL_BRIEFING 30001 0 0 0 0
```

The only other main change we need to do for a car kill frenzy is checking the player gets out of the car. Initially you may think you also need a check to see if the car's exploded - but if it has, it means that either the player has died or is out of the car anyway. So for this check with use the `CHECK_CAR`, a check type command, that we'll loop around until the player gets out of the car;

```
30026 CHECK_CAR 2000 30026 0 -1 0
```

You'll also note that in the checking section I've been a little bit nifty and used 'common' code for more than one check. Every check that fails the mission - time running out, dead/arrested and getting out of the car - all need to cancel the FRENZY_CHECK thread, cancel the timer, display the "FRENZY FAILED!" and do a RESET. However, each check also needs to kill the other two checks. Hence the following code;

```
30020 SURVIVE 0 0 0 20 0
30021 KILL_PROCESS 30008 0 0 0 0
30022 KILL_PROCESS 30009 30030 0 0 0

30023 DEAD_ARRESTED 10 0 30023 -1 0
30024 KILL_PROCESS 30007 0 0 0 0
30025 KILL_PROCESS 30009 30030 0 0 0

30026 CHECK_CAR 2000 30026 0 -1 0
30027 KILL_PROCESS 30007 0 0 0 0
30028 KILL_PROCESS 30008 30030 0 0 0

30030 MESSAGE_BRIEF 0 0 0 0 2504
30031 KF_CANCEL_BRIEFING 30001 0 0 0 0
30032 KILL_PROCESS 30006 0 0 0 0
30033 RESET_KF 0 -1 -1 0 0
```

The three checks kill the other two processes, then all jump to line 30030 for the 'common' code. You can use this method a lot in missions to keep code neat and tidy. Also, it means that if there are other bits of code that need to be added to all of them, you just need to add in an extra line at 30030 - not in every check.

And there you have it, one car kill frenzy!

### Model Car Kill Frenzies

You get an infinite number of minute cars to get a certain ammount of points by blowing them up.

Model Cars are small remote controlled cars that blow up on demand. They appear in a few cases in the original game - I remember them being in a mission early on, and also in Vice City when they were tracking you down. There were also cases of Model Car kill frenzies.

The basic premise is simple - get a certain number of points within the set time by controlling these model cars and blowing up cars and people to reach the target. Also, coding a kill frenzy like this is actually quite simple. We'll be building off our Car kill frenzy, so make sure you've gone through that tutorial before you go on to this one.

In fact, the only different to this and a car kill frenzy are a few model car lines and one or two changes to the code. This first thing we need to add is a `DUMMY` line;

```
2003 1 (52,32,1) DUMMY 0 0
```

To explain why we need this, we first need to understand how the system does these model car setups. Basically, when it starts up, it transfer the car you are driving to a model car. In effect, you are 'inside' the model car. You continue to control this car until it's destroyed. Then it checks to see if you've got any more model cars - if it does it returns control to the next model car. If not, it returns control back to the original car. If also monitors for you pressing the 'enter/exit' key to get out - at this moment, it jumps back to the control of the original vehicle.

This DUMMY line holds the information of the Model Car you are driving. Just like `GET_CAR_INFO` and `GET_DRIVER_INFO`, the details of the model car is stored here and can be reference to by using this dummy. For the co-ordinates it's suggested you put them as the same as the vehicle. My research in to these co-ordinates show that only the Z value is important (put it at a different Z value and the model car will be created at a different level). The (X,Y) co-ordinates seam to have no effect - they are always created under our car.

There are two new commands we'll introduce here - `START_MODEL` and `RETURN_CONTROL`. The first we use is `START_MODEL`;

```
30003 START_MODEL 2003 0 0 -1 100
```

This is the most important line, and does all the setting up of the model. It puts you in control of a model car, and stores the information in our dummy (line 2003). 100 specifies the number of model cars to give. Because it's a kill frenzy, we want 'unlimited' cars, so we set it to something high. You'll notice when playing with them that it takes 3-4 seconds for the next model car to start, so with a limit of 100 cars within 80 seconds it's impossible to run out.

(Note: This line is actually more powerful that we've actually used. For missions with the model cars, you can specify which car they should be aiming to blow up. By using `DO_MODEL`, you can check to see if the car has been blown up. If it has, then the model car setup is stopped. See the MTD.doc for more details).

The other command we'll be using is RETURN_CONTROL. This returns the control back to the original car. We want to do this when both the kill frenzy is completed or failed. The 30003 points to the line where the START_MODEL resides;

```
30018 RETURN_CONTROL 30003 0 0 0 0
....
30034 RETURN_CONTROL 30003 0 0 0 0
```

I've made 2 other alterations to the code. The first is changing the `CHECK_CAR` to `IS_PED_IN_CAR`. Why? Remember, when you do the `START_MODEL` command, you are theoretically driving the model car. So if we do a `CHECK` on the original car we'll fail the mission when you start driving the first model car. We might be able to get away with doing a `CHECK_CAR` on the DUMMY line, but we might run in to problems (such as when control passes from one model car to the next). A better way is to use IS_PED_IN_CAR, which checks to see if the ped `<ped>` is inside any car. So we loop around this line until the player stops driving a car;

```
30026 IS_PED_IN_CAR 10 30026 0 -1 0
```

Finally, I've added the line;

```
30033 DEAD_ARRESTED 10 30033 0 -1 0
```

The best way to explain this is by taking it out and looking at the consquences. Remove this line, and then use the model car to blow up the original car. You'll get the frenzy failed message, but then before you respawn you can drive around the blown up car! To stop this, we add line 30033. This checks to see if you're dead/arrested before doing the `RETURN_CONTROL` line. This is looped around until you are not dead/arrested - i.e., waiting till you respawn. That way we don't return control too early.

## Insane Stunt Bonuses

### Introduction

This tutorial will show you how to make the INSANE STUNT BONUS features for you map. These are used in the originalgame extensivly in San Andreas, no-one seems to have found most of them unless they actually looked round the map and mission file, like we will now.

The way they are actually work is that there is a 'launch' ramp and a 'catch' ramp on the map. There is a launch TRIGGER and a catch TRIGGER object in the mission file too.

The catch trigger is turned off when the level starts but when you drive on the launch ramp the catch trigger is enabled when You drive on the launch ramp. The way it does this is that the launch trigger enables the catch trigger, then waits for a few seconds, then turns off the catch trigger.

You have to get from the launch ramp to catch ramp within this time. If you crash, or decide to chicken out of the jump, the catch trigger will be disabled and the launch trigger enabled ready for a seconds try.

If you reach the catch trigger before the launch trigger disables it again, you will get the INSANE STUNT BONUS and 50 grand. Radical!

Now you may have noticed that you might be able to to drive to the lanch trigger, drive round whatever the jump is over, trigger the catch trigger and so get the bonus with ever doing the jump. This is indeed possible, but the timers are set very close to the time it willt take for a sucessful jump, and the launch or catch trigger will be put at the end of a ramp that you would have to drive up. So it's pretty impossible to cheat these jumps in reality.

If you noticed this congrats, you have the makings of an excellent mission coder - maybe even a good as me one day! 8-D

### Where's the Stuff Responsible for This?

I will be using San Andreas as the example here as it has a few of these bonuses. Sure, you rember, there was that Penetrator (Porsche 911) parked up in north-east Richman that you cold jump over all that water... So open up the original mission.ini file, SanA.cmp in J25 1.6 and the original English.FXT file to view the text.

To locate these stunt bonuses, we'll first have to locate the “INSANE STUNT BONUS” text, so search the FXT file for `INSANE STUNT BONUS`

And it will bring up this line:

```
[2514]INSANE STUNT BONUS!
```

Now we search the mission code for to see when message 2514 is created. So search for:

```
BRIEF 0 0 0 0 2514
```

And it finds this chunk:

```
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

Now we see the `DISABLE` is pointed to 10000 which must be a trigger, so we go up to the objects area of that chapter and find the object chunk around 10000:

```
10000 1 (42,195,4) TRIGGER 29000 1
10010 1 (42,200,4) TRIGGER 29010 1
```

Marvelous! A takeoff and landing trigger.

### ut What is it Doing?

Now to work through this code. The `DISABLE 10000` part means that it was `TRIGGER 10000` that started this procedure. We'll bear that in mind.

Now a quick run through it. After `TRIGGER`s 10000 and 10010 are DISABLEd it then does this odd-looking command:

```
29020 GOTO 10010 0 29041 100 0
```

Now what this actually does is a little odd to explain. Pronounce the command "Got To" not "Goto" because the command is testing to see if you "get to" trigger 10010 before 100 frames have been drawn - GTA draws 25 frames per second, thus it gives you 4 seconds to leap from the launch ramp to the catch ramp, which wouldn't be long enough to drive over the jump trigger, drive round and then drive over the landing trigger before the timer ran out. Times like this you realise how much the GTA1 mission coders cared about thier work and how good their dedication made it.

If you don't get to the catch trigger before 100 frames have passed then it goes to line 29041:

```
29041 ENABLE 10000 0 0 0 0
```

Now this may seem a bit weird, but what it's doing is to stop waiting for the player to reach the catch trigger and enables the launch trigger ready for you to try again. You probably noticed when playing that if you didn't make it then tyou could try it again... once you'd driven back there from the hospital!

But what if you do make it? Well if you go over the catch trigger before 4 seconds after the launch trigger then the command processor falls through to the next command in sheer delight:

```
29030 IS_GOAL_DEAD 85 29041 0 0 0
```

Now this checks to see if the PLAYER (object 85) is dead. If he is, it goes to 29041 which makes it all ready to start again. You may have noticed that when you've over-jumped it, like blown straight over the catch ramp and crashed into an ornamental pond (or similar) then you can still try it again. And if you do make it the command processor staggers triumphanty into the "success" bunch of code:

```
29035 SURVIVE 0 0 0 40 0
29038 MESSAGE_BRIEF 0 0 0 0 2514
29039 SURVIVE 0 -1 0 0 50000
```

The `SURVIVE` line makes it wait for 40 frames, then it displays our good old friend message 2514:

```
[2514]INSANE STUNT BONUS!
```

Then the final `SURVIVE` line adds 50000 points to your total (assuming x1 bonus multiplier) and kills the routine with that sharp pointy -1 parameter.

Hopefully you now have some understanding of how these things work!

## Pager Messages

### What is This?

This tutorial will show you how to make the random pager messages we all love from the original GTA1 maps. The best way to start is to explain quite how they work.

There are a network of `TRIGGER` objects around the city with a range of 4 blocks. When the player runs over these, it starts a routine which waits for a couple of minutes, then makes the pager message. The reason it waits so long is so that you never really know quite where the pager message got triggered from, thus making them seem random. It's a neat feature.

### Where's the Stuff Responsible for This?

I will be using San Andreas as the example here as it's pager messages are in capitals so are easy to find. (Plus they're bloody hilarious!) So open up the original mission.ini file, SanA.cmp in J25 1.6 and the original English.FXT file to view the text.

To locate the text we will be working with, search the FXT file for:

```
[2610]Man gives birth to dog. Shocked wife has kittens. Film at eleven
```

LOL, it's a great one I know. Now to find the `TRIGGER` that will set it off. We have to reverse-engineer this from the message back trhough the code to the object, it's a very common technique for finding out how the mission file ticks.

If you search the mission file for:

```
P_BRIEF 0 -1 0 0 2610
```

That will find the line that makes the pager show that message. Easy. Now if you look around that lne, you'll see the procedure:

```
28300 DISABLE 9700 0 0 0 0
28301 KEEP_THIS_PROC 0 0 0 0 0
28310 SURVIVE 0 0 0 2500 0
28320 P_BRIEF 0 -1 0 0 2610
```

### But What is it Doing?

Now to work through this code. The `DISABLE 9700` part means that it was `TRIGGER 9700` that started this procedure. We'll bear that in mind.

The `KEEP_THIS_PROC` is used so that the pager routine won't get killed. It would get killed if you complete a mission in this time, as at the end of the mission it does a RESET which stops all routines apart from the the reset is in.

The `SURVIVE 0 0 0 2500 0` makes the command processor wait there for 2500 frames. now there are 25 GTA frames per second, and 2500 divided by 25 is 100. Therefore the timer actually waits 100 seconds. Wow, that's like looking into the brain of the original mission writers, scary huh?

The `-1` in the `P_BRIEF` statement is put there to stop the command processor from falling out of the bottom of the routine. Not something you want to happen!

Now that we understand the code we can go have a look at where this object is. To search for this trigger sucessfully, we can take 2 approaches. We can search the whole docment for it's object number and hope that we can fgure out which obejct 9700 it is, or we can search for this:

```
TRIGGER 28300 4
```

This is likely to work better, because we only want trigger objects, we only want a trigger that points to line 38300, and also we know (because I told you) that the radious has to be 4. So serch for that and it'll jump straight to the right line:

```
9700 (77,10,1) TRIGGER 28300 4
```

We see that it's coords are `(77,10,1)` and if you go check this on the `SanA.cmp` map you'll see this trigger is located in Richman and covers and area which only includes half the width of the east-west road. You'll find this happen a lot, because the programmers figured that if you only use half the road then there's half the chance the palyer will get the trigger each time, thus prolonging the time to get through all the pager messages AND increasing the randomness.

It's times like this you realise how much the GTA1 mission coders cared.

### How do I Try This Myself?

I know, this is perhaps the greatestthe moment of your life. You've realised that snazzy wireframe maps and lense flares aren't the best thing about games, it's the dedication and inginuity of the coders.

Here's the code you'll need to get off the ground, you'll have to custimise the co-ords of objects and the line in the FXT file that the `P_BRIEF` is set to display (or make a new line and use that nummr in the `P_BRIEF` command - whoa, you're a full-on modder now!) and tinker with the time it waits for...

Well here ya go, you crazy diamond:

```
[1]
Pager Briefs, 22, yourmap.cmp, 0,
100 1 1 1 1 0

0 (0,0,0) DUMMY 0 0
1 1 (1,1,1) TRIGGER 10 255

100 (5,5,4) PARKED 04 768

1000 (126,16,4) TRIGGER 25000 10
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

32100 1 (4,5,4) PLAYER 100 256

-1

0 DONOWT 0 0 0 0 0

10 STARTUP 1 0 0 0 0
12 MESSAGE_BRIEF 0 0 0 0 8001
98 DONOWT 0 -1 0 0 0


25000 DISABLE 1000 0 0 0 0
25001 KEEP_THIS_PROC 0 0 0 0 0
25002 SURVIVE 0 0 0 250 0
25004 P_BRIEF 0 -1 0 0 2600

25010 DISABLE 1001 0 0 0 0
25011 KEEP_THIS_PROC 0 0 0 0 0
25012 SURVIVE 0 0 0 250 0
25013 P_BRIEF 0 -1 0 0 2601

25020 DISABLE 1002 0 0 0 0
25021 KEEP_THIS_PROC 0 0 0 0 0
25022 SURVIVE 0 0 0 250 0
25023 P_BRIEF 0 -1 0 0 2602

25030 DISABLE 1003 0 0 0 0
25031 KEEP_THIS_PROC 0 0 0 0 0
25032 SURVIVE 0 0 0 250 0
25033 P_BRIEF 0 -1 0 0 2603

25040 DISABLE 1004 0 0 0 0
25041 KEEP_THIS_PROC 0 0 0 0 0
25042 SURVIVE 0 0 0 250 0
25043 P_BRIEF 0 -1 0 0 2604

25050 DISABLE 1005 0 0 0 0
25051 KEEP_THIS_PROC 0 0 0 0 0
25052 SURVIVE 0 0 0 250 0
25053 P_BRIEF 0 -1 0 0 2605

25060 DISABLE 1006 0 0 0 0
25061 KEEP_THIS_PROC 0 0 0 0 0
25062 SURVIVE 0 0 0 250 0
25063 P_BRIEF 0 -1 0 0 2606

25100 DISABLE 1010 0 0 0 0
25101 KEEP_THIS_PROC 0 0 0 0 0
25102 SURVIVE 0 0 0 250 0
25103 P_BRIEF 0 -1 0 0 2610

25200 DISABLE 1020 0 0 0 0
25201 KEEP_THIS_PROC 0 0 0 0 0
25202 SURVIVE 0 0 0 250 0
25203 P_BRIEF 0 -1 0 0 2620

25210 DISABLE 1021 0 0 0 0
25211 KEEP_THIS_PROC 0 0 0 0 0
25212 SURVIVE 0 0 0 250 0
25213 P_BRIEF 0 -1 0 0 2621

25220 DISABLE 1022 0 0 0 0
25221 KEEP_THIS_PROC 0 0 0 0 0
25222 SURVIVE 0 0 0 250 0
25223 P_BRIEF 0 -1 0 0 2622

25230 DISABLE 1023 0 0 0 0
25231 KEEP_THIS_PROC 0 0 0 0 0
25232 SURVIVE 0 0 0 250 0
25233 P_BRIEF 0 -1 0 0 2623

-1
```

## SENDTO Command

Yeah that's right folks, `SENDTO` is a perfectly solid command... bear in mind it uses the same AI as the Fire Trucks and Ambulances and you realise that it's the best vehicle AI the mission file has to offer.

The key to `SENDTO` is setting the map up. To make the vehicle drive from one place to another you will need to make sure that the road is continuous, if you have missed any vehicle directions it won't work.

You must make sure you're slopes are defined correctly too. Also something to note is at junctions it can sometimes be prudent to remove traffic that may create a crash with your bus. If you have the bus coming out of a 2-laner at full pelt across a 6-lane freeway there's a high chance it will get clouted by cross traffic.

Therefore leave the vehicle direction for the bus route across, but remove the vehicle arrows for 5-8 cubes to each side. Of course, this means you won't be able to use this stretch as a `SENDTO` route now - you either chance a crash or limit the routes I'm afraid.

It's a good idea to start the vehicle in an area defined as “PLAYER ONLY” but with traffic directions, this will prevent your car being spawned on top of a citizen vehicle and also prevent it crashing into something when the first `SENDTO` fires it off - it does tend to lurch off with the first `SENDTO` command. Another feature is that to set the performance of your car in `SENDTO` mode, use `GTACars` in the 'specs' tab.

The max speed (multiply by 3 for mph top speed) acceleration and braking all effect the AI. As does the tail slide in the steering tab.

The car will accelerate at the SAME rate to it's top speed; if this is high it's a good idea to set the acceleration low.

Once the map is set up ready, now you have to put in you waypoints/checkpoints or whatever you want to call them.

Using DUM_MISSION_TRIGs with a radius of 1 are your bet bets for this as they define the vehicle you are using directly.

It's also worth noting here that the vehicle has to be defined BEFORE the waypoints - that's just the way GTA1 associates objects.

The trick to you waypoints is that the Z-coord has to be 1 above the logical level.

So if your car is on level 4 and you want it to drive along a flat road at that level to a waypoint, that waypoint has to be defined at level 3.

Now to get the vehicle started there's one of two methods:

- Make the `PLAYER` get in and make the car drive like it's being chauffer-driven.
- Use `DUMMYON` to create a driver. I prefer not to use `DUMMY_DRIVE_ON`.

Remember to `REMAP` the player at the end of the route if you are chauffer-driving it!

I'll publish the code I use here:

```
[1]
Guided Tour, 22, Pacific.cmp, 0,
100 1 1 1 1 0

0 1 (1,1,1) TRIGGER 30 255 'level start trigger
1 1 (9024,2880,192) PARKED_PIXELS 66 520 'PLAYERs car

100 1 (139,47,3) PARKED 9 512 'Tour Bus
110 1 (132,43,2) DUM_MISSION_TRIG 60 1 100 'First waypoint
118 1 (117,26,3) DUM_MISSION_TRIG 100 1 100 'second waypoint
122 1 (117,26,3) DUM_MISSION_TRIG 100 1 100 'final waypoint

32100 1 (146,46,2) PLAYER 1 768

-1

0 DONOWT 0 -1 0 0 0

30 STARTUP 0 28000 -1 0 0 '28000 DISABLEs all the triggers
44 ARROWCAR 100 0 0 0 0 'car 100 is used for this
50 STEAL 100 0 0 0 0
52 SPEECH_BRIEF 0 0 0 0 1002 'once stolen inform player
54 ENABLE 110 0 0 0 0
56 DUMMYON 100 0 0 0 0 'DUMMYON prevents PLAYER interfering
58 SENDTO 100 -1 0 110 0 'Begin route

60 DISABLE 110 0 0 0 0 'disable this waypoint
64 ENABLE 120 0 0 0 0 'enable next waypoint
68 SENDTO 100 -1 0 120 0 'proceed to next

70 DISABLE 120 0 0 0 0 'disable this waypoint
72 ENABLE 122 0 0 0 0 'enable next waypoint
74 SENDTO 100 -1 0 122 0 'proceed to next

120 DISABLE 122 0 0 0 0 'disable this waypoint
122 MESSAGE_BRIEF 0 0 0 0 2500 'MISSION COMPLETE
124 SPEECH_BRIEF 0 0 0 0 1009 'Explain mission over
128 REMAP_PED 32100 0 0 0 0 'Change PLAYERs clothes back to normal 398
DONOWT 0 -1 0 0 0 'Finish procedure
```

That's what's being used in Pacific City and it's working every time now. I haven't expanded this code yet but these are some things you may like to use as belt and braces: `CARSTUCK_TRIG` - so that if the vehicle crashes and gets wedged against a fence or such you allow the player to get out, or the AI driver gets out and fights on foot. `DESTROY` - if the vehicle drives off a cliff and lands in the sea, the hospital respawn engine doesn't intervene as you aren't the `PLAYER`, you're the `DUMMYON`.

Therefore detect the vehicle being destroyed, remap the player and do `PED_OUT_OF_CAR` or something to kill the ped manually. Or `EXPLODE` the car/`PLAYER`... depends what you want I guess.