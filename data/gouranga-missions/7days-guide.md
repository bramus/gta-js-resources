# 7 Day Guide for MISSION.ini

## Intro

Before we begin, here's a rundown of what we'll be doing:

- Day 1 - We'll be kicking off straight away into code - the whole basics of the code will be layed down.
- Day 2 - On the second day of the tutorial, my true love gave to me.. our first check! We'll be checking against blowing up the car on the second day.
- Day 3 - An important (and tricky) day - checking for them getting out of the car.
- Day 4 - We want to make this trickier - if they die or get arrested, we want them to fail the mission. Day 4 shows you how.
- Day 5 - Still too easy? Then lets have a time limit! Don't get to the garage door in time and it's BBQ time.
- Day 6 - We get in to advanced stuff here, using GET_DRIVER_INFO to get a shooting ped
- Day 7 - Extra titbits. We'll be introducing loads of kewl l33t extras to finish off the mission.

## Basics

This section includes a few useful bullet points to keep in mind when writing your own missions;

- Using the original mission code

  The best way to learn mission code is through the original. If there's something you see in the original that you don't know how to do, then have a look for it in the mission code. The best way to do this is to remember a briefing message around the code you want to look at. Then open up the FXT file, find the relevant peice of code, then do a search for that number.

- Set the player line to a memerable one

  It's a good idea to set the PLAYER line to having a memerable line number. You will have to refer to it quite a lot during the mission code (such as setting a peds enemy), so with a memerable line number you won't have to keep checking back. Something like 666 you won't forget.

- Working out survive lengths

  You'll be using `SURVIVE`'s loads in your missions. Remember, there are 25 survive 'ticks' in a second - so ten seconds would be a SURVIVE of 250. Also remember that for kill frenzy times on a POWERUP, you need to add 100 to this number.

- Creating kick ass insane stunt bonuses

  Everyone loves insane stunt bonuses, so it's worth the effort to create a few. The trick behind doing them is the downwards slope 'feature' in the GTA mission engine - if a vehicle falls and lands on a slope (in either direction) then the damage is set to 50. This way, you can create a huge jump and get the player to land safely by setting up a small slope as a landing pad!

- Set out your mission code correctly

  It's important to number your code in the best way. Renumbering your code is a horrible thing to do - take my word for it. Leave gaps of 5 or 10 inbetween each line. Set each mission to a group of 1000 lines, both in the mission declarations and the code (so mission 5 will have lines 5000-5999 in the declarations and mission code, for example). Set a section for your kill frenzies, insane stunt bonuses and respawning powerups (such as 20000-25000, 25000-30000 and 30000 onwards).

- Make a powerups and triggers procedure

  You need to turn on all your powerups and disable most of your triggers right at the start of the mission. The best way to do this is launch a `KICKSTART` straight after the `STARTUP` line, say to line 35000. Here you can list all your `DISABLE`'s and `POWERUP_ON`'s. set 35999 to a do `NONOWT` to stop the code.

- Ensure all your files are easily excessible

  When writing missions you need a lot open - the map file, the `MTD.doc`, the `mission.ini`, the `english.fxt` etc... it's a good idea to setup a quick way to launch them all. I setup a program group in the start menu with links to the game and app the files that I use. You may also find it useful to use a mod-launcher to make it easy to test different mods.

- Keep your mission positions close

  When writing a mission, it's a good idea to keep all the points close together. For example, if the player has to go to a phone, pick up a car and take it to a garage, put all the points in the same road or area. That way you can quickly test each section. Once you're sure everything works perfectly, space everything out and test the difficultly of the level.

## Day 1

The mission we'll be coding is in San Andreas textures. The mission invloves 3 parts;

- Answering a phone
- Grabbing a car
- And driving it to the garage

We'll be laying the basic grounds for the whole mission in our first day, so we best get a move on. There are a couple of concepts that need to be introduced before we hit the code. The code is split in to 2 sections - objects and declarations. Objects define the peds, cars and triggers that are used in the main code. When a trigger is activated - the two most common being `TRIGGER` and `CARTRIGGER` (see ya `MTD.doc` for all of 'em) - the code then jumps to the line specified in the trigger. It then runs through each line of code performing the specific action.

After each line of code, it will jump to the next line depending to weather it's a success or failure. For example, have a look at the line `DEAD_ARRESTED`. And example of this line would be;

```
1000 DEAD_ARRESTED 500 1500 3000 0 0
```

This line will check if the ped defined at line 500 is dead or arrested at that given point. If it is, it's goes to success (line 1500). It's it's not, it goes to failure (line 3000). Also, a 0 represents the next line of code, and a -1 represents stopping the flow of code. Thus;

```
1000 DEAD_ARRESTED 500 0 -1 0 0
1010 ARROW 1000 0 0 0 0
```

With this line, if the ped is dead or arrested, it goes on to the next line (line 1010), but if it isn't, the the code stops running this thread.

It is important to remember that the engine can _run many threads at one time_ - a good example of this is when you're doing a kill frenzy at the same time as the mission. It can be checking to see if you've done the mission objective (killing a Ped, for eg), whilst also checking to see if you've completed the kill frenzy. Checks are used in every mission - checking to see if you're dead, if the mission car is destroyed, if the times run out etc.

There are two main ways of starting more than one thread. The easiest way is to use triggers - in the kill frenzy idea (say a tank KF), the game will be running the mission thread, but when you get in the tank a `CARTRIGGER` will start the kill frenzy thread too. Both will run simultaniously and constantly until one is halted.

There will be times when you will want to stop the flow of code until something is completed. There are two ways of doing this - using a Wait or a check. An example of a wait would be `DESTROY`. This waits for you to destroy a particular vehicle before moving the code on. Other useful examples include `STEAL`, `WAIT_FOR_PED` and `PARK` (more on all of them later in the tutorial). A check basically involves looping round the same line of code until a criteria is met. For example;

```
1000 DEAD_ARRESTED 500 0 1000 0 0
```

This command will check to see if ped defined at line 500 is dead or arrested. If he is, it'll go to the next line of code. If it's not, it'll go to line 1000 - that is, the current line. So the system will continue to check line 1000 until ped 500 is dead or arrested.

Anyway, enough banter, and on with the code. We'll start with the object declarations. In this mission, we'll need a telephone, a door and a car. We'll also need a `PLAYER` line - to define where the player starts, and a `TRIGGER`, to start off the mission. I've also included a second door and telephone (for completions sake) and a `DUMMY` (used for parking the car - more on that in a minute). The code I've used looks like so;

```
10 (63,150,4) PLAYER 0 0
15 (63,150,4) TRIGGER 1000 0
20 (130,145,4) TELEPHONE 0 256
30 (131,145,4) TELEPHONE 0 256
40 (59,151,3) DOOR 1 4 0
45 (59,152,3) DOOR 1 4 0
50 (94,107,4) FUTURECAR 4 0
60 (59,151,4) DUMMY 0 0
```

The line numbers are deliberately spaced out a lot - this is so that if you need to add more things in the future you've got space to do so. First up is the player line - I've stuck him on the pavement. The trigger is on the same square as the player - that means it will trigger right at the start of the game to start the mission. Next is the telephone declarations - both pointing downwards (angle of 256). The doors have been lifted from the original code. `FUTURECAR` defines a car that will be added to the map at a later date - we don't want the player jumping in the car before they've answered the telephone call.

The first line of code you should always have in the commands is `DONOWT` on line 0. Don't ask why, it's just the convention used. Basically, without it the code will run straight into mission code.

```
0 DONOWT 0 0 0 0 0
```

Now, the trigger in line 15 will run the code starting at line 1000 - and so our next line should be 1000. There's a special command for the first line of mission code - `STARTUP`. The only effect I can see it actually having is the "GRAND THEFT AUTO" cheering at the start of the game, but hey we'll stick it in.

```
1000 STARTUP 15 0 -1 0 0
```

As stated in the MTD, the 15 is the line of the trigger in the declarations. We want it to run the next line of code - and so we've put a 0 in the success line.

The first part of the mission is going to answer the phone. We want to display the first message, point the arrow to the phone, and then keep checking until they answer it.

```
1010 SURVIVE 0 0 0 30 0
1020 BRIEF 0 0 0 0 1001
1030 SURVIVE 0 0 0 20 0
1040 ARROW 30 0 0 0 0
1050 ANSWER 30 0 1050 30 2000
```

The `SURVIVE` line is a wait-type command - it waits for the alotted time to be up, in this case '30' (these are measured in 'game ticks'. 25 of these 'ticks' makes up 1 second. So for 60 seconds, you'd want a `SURVIVE` for 1500 etc). We'll be using this a lot to make the game less jumpy - we don't want the message to be display straight away when the game starts. A little pause makes it more natural. Line 1020 displays our first brief. As the MTD states, the last property of `BRIEF` defines which message to be displayed in the FXT file. Download my FXT file to see which lines we'll be using. 1001 just introduces the mission and sends the person to the phone for more information. Another little pause (line 1030) and then we use the `ARROW` command to point to the telephone. the 30 in line 1040 defines what to point to - in this case it's the telephone defined in line 30.

Once the arrow is pointing to the phone, we want to start the phone a-ringing and wait for them to answer it. This is all done in the command `ANSWER`, used in line 1050. Answer is defined as `<phone> <success> <failure> <rings> <score>`. What we want to do is keep the phone ringing constantly until they answer it. So we've got a loop situation - we want it to keep looping at line 1050 until it's answered. Firstly, the telephone is at line 30, so `<phone>` = 30. Next is the success line - if they answer it then it goes on to the next command. If they fail to answer it within the time, then we repeat the command - hence the 1050. I've set it to 30 rings (it doesn't really matter in this case - if they fail then it does it again anyway). `<score>` defines how many points they get if they succeed - I gave 'em £2000.

Next up, we want them to get the car;

```
1070 ARROW_OFF 0 0 0 0 0
1080 SURVIVE 0 0 0 5 0
1090 BRIEF 0 0 0 0 1002
1100 SURVIVE 0 0 0 20 0
1110 PARKED_ON 50 0 0 0 0
1120 ARROWCAR 50 0 0 0 0
1130 STEAL 50 0 0 0 0
```

First up, we turn off the arrow (line 1070). Otherwise the arrow keeps pointing at the phone until the next `ARROW` command. We give them the next brief (line 1090), and do a couple more `SURVIVE`'s. Before we point to the car for them to get, we must 'turn on' the car (line 1110). Currently the car doesn't exist on the map. `FUTURECAR`'s must be turned on. You can use either `CAR_ON` (to create the car with a driver) or `PARKED_ON` (without a driver). The 50 points to the line of the `FUTURECAR` to create. We now want to point to the car - note this time we use `ARROWCAR` instead of `ARROW` (line 1120). `ARROWCAR` has the added benefit of following round the car if it's on the move. Even though the car we're pointing to is stationary, it's worth doing `ARROWCAR` anyway (just incase you nudge it before getting in).

Once the car's on and the arrow's pointing at it, we want the player to get in the car. This time, we use a wait command - `STEAL`. This halts the execution of the code until the player gets in the car (in this case, the car in line 50). There are other ways of doing this (such as doing a loop with `CHECK_CAR`) but the `STEAL` command has the added bonus of doing a 'cheering' sound when the player gets in. The execution of code continues once the player gets into the car - and then it's on to the next line.

```
1140 ARROW_OFF 0 0 0 0 0
1150 SURVIVE 0 0 0 5 0
1160 BRIEF 0 0 0 0 1003
1170 DOOR_ON 40 0 0 0 0
1180 SURVIVE 0 0 0 20 0
1190 ARROW 60 0 0 0 0
1200 PARK 40 0 60 3 20000
```

The final part of the mission involves getting the car to the garage. Just like before, we turn off the arrow (line 1140) and give the player a briefing (line 1160). Before we send the player off to the door to drive inside, we must turn the door 'on'. This basically means that it'll open up when the player gets close to it. Once that's done, we give a quick wait before pointing to the door (line 1190 - the 60 pointing to the `DUMMY` 'inside' the door).

Once again, we want to hault the execution of code until the player drives in to the garage. It's another wait command - this time `PARK`. This is badly explained in the `MTD.doc` - but this command does actually wait for you to park in it before continuing. 40 is the line of the door to drive into. The 60 is the space 'inside' the door - that's our `DUMMY` in line 60. The 3 is the face you want the guy to walk out of once the car is parked - for us it's east, which corresponds to 3 (as a general guide it's usually the opposite of the face of the door). Once that's done, the mission is completed, so we give them 20,000 points for their trouble. All that's left is to give the tie up the end of the mission;

```
1210 ARROW_OFF 0 0 0 0 0
1230 DROP_WANTED_LEVEL 0 0 0 0 0
1235 MISSION_END 0 0 0 0 0
1240 SURVIVE 0 0 0 20 0
1250 MESSAGE_BRIEF 0 0 0 0 2500
1260 SURVIVE 0 0 0 20 0
1270 BRIEF 0 0 0 0 1004
1280 RESET 0 -1 -1 -1 0
```

The first thing to remember to do is turn off the arrow - otherwise it'll be pointing at something completely irrelevant until you do the next mission. `DROP_WANTED_LEVEL` basically calls off the cops from you - this is generally used in GTA when you complete a mission (line 1230). `MISSION_END`, on the face of it, doesn't really do much - as the `MTD.doc` states it "tells the game that this mission has been successfully completed". I've never actaully found a use for it, but hey, who's to argue with DMA :) Line 1250 displays the "MISSION COMPLETE!" sign - the `MESSAGE_BRIEF` line displays the big yellow writing in the middle of the screen. It's also accompanied by a cheer. A final message is in order, congratulating the player (line 1270). Although not compulsorary, I use the `RESET` command at the end of each mission. It basically frees up all the mission cars and peds, and stops and processes that's running in the mission. Note that the first and second "-1" are used to stop the execution of this thread.

And there you have it - one GTA mission! Try it out, run it, play around with it. Try changing the location of the telephone, player and car, or the type of car you get into. You'll immediately notice some problems with the mission though - if you blow up the mission car, for example, the mission won't fail. Thankfully, this is but a 1st in a 7-part mission extravanganza - the next day will be concentrating on this very problem. But for now, take a deep breath, give yourself a pat on the back, and get a good nights sleep - it's been a good days work :D

## Day 2

Now that we've got the basic mission structure set up, we can start adding in the checks. Probably the most important is of blowing up the car. Currently, if you blow up the car then the arrow continues to point at it, and the mission continues. What we need is a constant check to see if the car is dead - when it is, we need to stop the mission.

This is done by using `KICKSTART`'s. A kickstart will run two checks in parallel - check to see if you've got in the car (or, if you're in the car, driven to the garage) and another check to see if the car's destroyed. If the mission is completed successfully, then we need to stop the process of checking for the car - and similarly, if the car is destroyed we want to stop checking to see if they've gone to the garage.

The first change to the mission structure is the addition of these two lines;

```
1125 KICKSTART 1400 0 -1 -1 -1
1129 KICKSTART 1130 -1 -1 -1 -1
```

What this does it start two processes. The first process is for checking if the mission car has been destroyed (we haven't coded this bit yet). This check will start on line 1400 (hence the 1400 in line 1125). What this `KICKSTART` does is start a new thread of code, whilst continuing with the current one. We then want to `KICKSTART` the rest of the code (line 1129), and then hault the process. This is done with another `KICKSTART` line, pointing to line 1130. The first -1 represents where to go when the `KICKSTART` succeeds (remember -1 means stop the thread).

Why do we need the `KICKSTART` in line 1129, I hear you ask? It seams perfectly logical to carry on the code normally. But remember, if you destroy the car, we want to stop this thread. We can't stop the thread unless there's a `KICKSTART`. Make sense? If not, don't worry, it will when we get on to actually killing the process.

The next line I've added is at 1205:

```
1205 KILL_PROCESS 1125 0 0 0 0
```

Once the park routine has been completed (remember on line 1200 it will wait for the person to park before continuing), we want to stop the check on destroying the car. Otherwise, when the car park's it is actually 'destroyed' (it's ceases to exist when you drive it to a garage) and thus they'd fail the mission when they complete it! Line 1205 stops the process that was kickstarted in line 1400. Basically, wherever the thread is running from line 1400, it will stop in it's tracks. Useful huh?

Now, all that's left is the check that the car's destroyed. Here's how I have done it;

```
1400 DESTROY 50 0 0 0 0
1410 KILL_PROCESS 1129 0 0 0 0
1420 ARROW_OFF 0 0 0 0 0
1430 SURVIVE 0 0 0 20 0
1440 MESSAGE_BRIEF 0 0 0 0 2501
1450 SURVIVE 0 0 0 20 0
1460 BRIEF 0 0 0 0 1007
1470 RESET 0 -1 -1 -1 0
```

Line 1400 is the most important. This is a wait type jobbie - it'll wait for the car defined in line 40 (our `FUTURECAR`) to be destroyed by any means possible. This includes blowing up, getting shot by a police car or another ped, or being 'turned off' by the mission code. If the car's destroyed then we want to stop the other thread - otherwise even after the mission failed it's possible to complete the mission (perhaps not in this case, because the car's been exploded, but it's still good advice to remove all processes if not being used). Therefore we stop the process that was kickstarted in line 1129.

From then on it's basic code - turning off the arrow, displaying the "MISSION FAILED!" message (remember, `MESSAGE_BRIEF` does the big yellow writing - in the case of 2501 it's accompanied by a sound effect), a failed msesage, then a `RESET`, stopping the thread.

Very important stuff covered in todays lesson - the basic premise of controlling multiple threads. The mission is not fully functional, but could pass off as a real mission. However, there's still loads to do yet to get it up to pro level. On day three we'll be doing a tricky section - checking for the player getting out of the car - then instructing them to get their ass back in again. I'll see ya then.

## Day 3

At the end of today's tutorial, the mission will be in fully working order. There's one more check that's required; checking for the player getting out of the car.

This one's a little bit different from the other kind of checks. In this one - when the player gets out of the car - we don't want to fail the mission. We want to pause the mission thread, and wait for the player to get back in before continuing.

In this mission we're going to have the car check starting at line 1500. So we want to start a new process at line 1500 when the player gets in the car. The first line that we need to add is therefore a `KICKSTART` straight after the player `STEAL`s the car;

```
1135 KICKSTART 1500 0 -1 -1 -1
```

This will start a new thread of code at 1500. We also need to stop this thread once the missions complete. Otherwise, once the car is parked (and subsquently disapeared) we'd get the 'get back in the car' message. Therefore we kill this process at the same time we kill the `DESTROY` check;

```
1208 KILL_PROCESS 1135 0 0 0 0
```

However, we don't have to kill the process in the thread of 1400 (the one that fails the mission if the mission car is destroyed). "How so?" I hear you cry. Well I'll tell you. The `KILL_PROCESS` line doesn't just kill the `KICKSTART` it's directed at - it also kills all subsequent `KICKSTART`s in the same block of code. Therefore, line 1410 not only kills the mission process, but it also kills the car check process that's started in line 1135. Follow? Take a look at the completed code and it should make more sense.

All that's left is to do the car checking code. What we need is a block of code that runs when the player gets out of the car, tells the player to get back in the car, waits for him to get back in, then start the check all over again. Here's how I've done it;

```
1500 CHECK_CAR 50 1500 0 -1 0
1510 ARROW_OFF 0 0 0 0 0
1520 SURVIVE 0 0 0 10 0
1530 BRIEF 0 0 0 0 1005
1540 SURVIVE 0 0 0 10 0
1550 ARROWCAR 50 0 0 0 0
1560 STEAL 50 0 0 0 0
1570 ARROW_OFF 0 0 0 0 0
1580 SURVIVE 0 0 0 10 0
1590 ARROW 60 1500 0 0 0
```

`CHECK_CAR`, as the name suggests, is a check type of command. It checks at that given moment if the player is in the car defined in line 50 (our mission car). If he is, it goes to `<succ>`, if not it goes to `<fail>`. So we want to keep looping around this line until it `<fails>` - until the player gets out of the car. Therefore the `<succ>` is line 1500, and `<fail>` is 0. The -1 represents not checking for a specific car remap (see the MTD.doc for full details of this command).

Once the player gets out of the car, we turn off the arrow (line 1510) and tell him to get back in (line 1530). We then point to the car (line 1550) and wait for him to steal it. Instead of the `STEAL` command I could have used the `CHECK_CAR` command, but `STEAL` gives the nice 'chearing' sound. It's then a matter of pointing back to the garage (line 1590). Note that the `<successs>` in line 1590 jumps the thread back to line 1500 - therefore looping back to check again. This way, the player can get in and out of the car infinite times and the system will keep checking for it.

It's worth noting here that in this example we've been quite lucky, in the fact that we don't have to 'pause' the mission thread whilst the player is getting out of the car. Note that between lines 1510 and 1550 the player is out of the car, but we do nothing to stop the original thread of checking how the mission is going. This is because the check we're doing in the original mission thread is `PARK` - and that check can't be completed unless in the mission car. Therefore if you're out of the car you can't complete the mission. However in many mission you have to temporarily turn off the mission thread - such as disabling a trigger - to make sure the player can't complete the mission if out of the car.

Now that the car check is done, we've got a fully working mission. Not bad for 70 lines of code eh? However, this tutorial plans to give you a full startup on mission programming, and were quite a way off that mark yet. Therefore we're going to jam-back this small mission with loads of extras and checks. Probably the most used check is checking the player is `DEAD` or `ARRESTED`, so this will be the next port of call. See ya on day 4!

## Day 4

Today we'll be covering a Dead/arrested check - that is, the player fails the mission if they die or get arrested. It's not a compulsorary check by any means, but a good one to have if you think your mission's too easy.

We're gonna start our check on line 1600, so that's where we need to start a new thread. It will kickstart after you answer the phone - but it can be done before you answer the phone, or once you get in the car. Here's the line I've used;

```
1122 KICKSTART 1600 0 -1 -1 -1
```

As always, this process needs to be cancelled. First up, we need one when we complete the mission (otherwise you could complete it, and then die, both completing and failing the mission). A simple `KILL_PROCESS` needed here;

```
1207 KILL_PROCESS 1122 0 0 0 0
```

We also need one if the car explodes. Unlike the car-checking process, we need to specically stop this process here because it's comes before getting in the car, not after.

```
1415 KILL_PROCESS 1122 0 0 0 0
```

Once the thread is taken care of, it's on to the code itself. We need to check if he's dead or arrested, then stop usual processes and display the mission failed message. Here's the code I used;

```
1600 DEAD_ARRESTED 10 0 1600 0 0
1610 KILL_PROCESS 1125 0 0 0 0
1620 KILL_PROCESS 1129 0 0 0 0
1630 ARROW_OFF 0 0 0 0 0
1640 DEAD_ARRESTED 10 1640 0 0 0
1650 MESSAGE_BRIEF 0 0 0 0 2501
1660 SURVIVE 0 0 0 20 0
1670 BRIEF 0 0 0 0 1006
1680 RESET 0 -1 -1 -1 0
```

The `DEAD_ARRESTED` line is a another check - it checks to see if, at that given moment, the player is either dead or arrrested. It goes to `<success>` if the player is, or `<fail>` if not. The check reports `<success>` the moment the player dies or is arrested, and keeps reporting a success until the player resporns at the hospital/police station. Furthermore, it will report if the player is on fire. Using this as a loop, I've kept repeating on line 1600 until the condition is true. Once it's reported true we kill two processes (the mission thread (line 1620) and the destroy thread (line 1610)), to make sure you can't fail the mission a second time or complete it once you die or get arrested.

(note: It is possible to split up this process in to two different threads - being dead or being arrested. This can be done with the two seperate checks `IS_GOAL_DEAD` and `IS_PED_ARRESTED`. That way you can just check for one of these cases, or have two different threads having different failure messages).

Line 1630 does a basic turning-off of the arrow. Line 1640, however, needs a quick explanation. First I'll explain what it does, and then how it does it. What line 1640 does is wait for the player to resporn before displaying the "MISSION FAILED!" message. That way it looks much more slick - otherwise it'll do the mission failed bit the minute you get caught on fire, which looks a bad pants. It does it by setting up another `DEAD_ARRESTED` loop - this time looping whilst the player is dead/arrested. When the player has "stopped being" dead or arrested (i.e. they resporn) then it does the mission failed message.

The rest of the code should be pretty common to you by now - display the mission failed message, short pause, display a message then do the usual reset command.

This is a very useful check, and is used in just about all of the missions in the original. It's up to you whether or not you want the player to fail the mission if they die or get arrested. Even if you choose not to, it's quite slick to display a message like "quit mucking around and get back on the job". If you wanna do this, just delete lines `1610` - `1630` and `1650`, and change `1680` to `DONOWT 0 0 0 0 0`.

There's one more line that I've put in to the code;

```
1137 INC_HEADS 4 0 -1 -1 0
```

This line simply increases the number of coppers on you by 4 - to a maximum of 4. So whatever wanted level you are on, once you get to line `1137` (which happens when you get in the car), you'll have 4 cops on you. Now there's an `ARRESTED` check, it makes the little that little bit harder :)

You'll notice that, now with 3 checks and the basic mission structure, it starts to get very complicated what processes should be killed and when. My only advice to someone doing a mission like this is just think through it logically - if the player has blown up the car - has it started the dead/arrested check? If not, don't kill it. Has the timer started? It it has, kill this process. If the programs quits out without an error message or crashes, you'll know it's something to do with your `KICKSTARTs` - such as killing a process that hasn't begun.

In the next lesson we'll be tackling the last of our checks - another common one to many of GTA's missions - the timer check. See ya there!

## Day 5

Right, for this lesson we'll setting up a timer. The timer will start when you get in the car, and we'll give the player 40 seconds to get to the garage. If they do it in time, the timer is turned off - but if they fail to get their in the allotted time the car will blow up and the mission will fail.

Firstly, we want to setup the timer, and `KICKSTART` a new thread. This new thread will wait (using the `SURVIVE` command) for 40 seconds, then will blow up the car and fail the mission. This check will start on line 1700, so these are the 2 lines we need;

```
1132 P_BRIEF_TIMED 0 0 0 40 1500
1134 KICKSTART 1700 0 -1 -1 -1
```

Line 1132 sets up the timer, using `P_BRIEF_TIMED`. Line 40 is the number to count down from (40 seconds) and 2502 is the message write in the pager - in this case "time left ...". As the MTD suggests don't use a complicated message here, just a short message works best. Remember this line does not do any time checking in itself - it just sets up a pager message lasting 40 seconds. To perform the time check we start up the new thread in line 1700 - hence the `KICKSTART` to 1700 (line 1134).

For the time-checking thread, we need to wait for the time to elapse, and then do the usual mission-failing commands:

```
1700 SURVIVE 0 0 0 1000 0
1710 ARROW_OFF 0 0 0 0 0
1720 KILL_PROCESS 1122 0 0 0 0
1730 KILL_PROCESS 1125 0 0 0 0
1740 KILL_PROCESS 1135 0 0 0 0
1755 EXPLODE_CAR 50 0 0 0 0
1760 MESSAGE_BRIEF 0 0 0 0 2501
1770 SURVIVE 0 0 0 20 0
1780 BRIEF 0 0 0 0 1008
1790 RESET 0 -1 -1 -1 0
```

So for the first line, we do a `SURVIVE`. Remember there are 25 'game ticks' for every second - so for 40 seconds we want 40x25 ticks = 1000. This line waits for the time to expire before going on to the next line. When the time has expired, we kill the arrow (line 1710), and then proceeed to kill off all the processes that are running. These are the `DEAD_ARRESTED` check, the `DESTROY` on the car check, and the getting-out-of-car check (lines 1720, 1730 and 1740 respectively).

`EXPLODE_CAR` is a nice and simple command (line 1755), simply exploding the car declared in line 50 (our mission car). We then do the usual mission failure gumf.

Now what we need to do is kill this process and stop the timer if the mission is completed, if you're dead/arrested, or if you destroy the car. This is easy for the mission complete thread, but hard for the other two - I'll explain why in a minute. Firstly, when the mission is complete. We need to stop the thread started in line 1134, and then stop the pager-timer started in line 1132. Hence we use these lines;

```
1209 KILL_PROCESS 1134 0 0 0 0
...
1220 CANCEL_BRIEFING 1132 0 0 0 0
```

Line 1220 is a new command - but quite self-explanatory. It simply stops the timer defined in line 1132 (our pager timer).

Now for stopping the timer if you're dead/arrested - in the thread from line 1600. However, it's not as simple as just killing the process and starting the timer. Why? Because we don't know if the timer has started yet. If you recall, we started the dead/arrested check once they answered the phone. The timer thread doesn't start till you get in the car. If you die or get arrested before reaching the mission car, then there's is no timer to stop, and no thread to kill. This will result in an error message, which is no good.

There are various ways of getting round such a problem - and probably the slickest of them all, using a scheme I devised myself (*swags tie*), is using counters. A counter is just what it sounds - a definition that can hold an integer number. It can be incremented (increased by 1) and decremented (decreased by one), and can be COMPAREd with other numbers. It was designed for missions which involve, say, taking 3 taxi's to the docks. However, it can also be used in cases like this. I'll explain as we go - but first to define the counter. This is done in the definitions section of the code;

```
70 (0,0,0) COUNTER 0 0
```

The first number sets the starting value for the counter - in this case we just want it on 0. This counter will act as a boolean value - taking one of two values. These values will be 0 and 1. 0 will represent before taking the car, and 1 will represent after taking the car. Remember, once you get in the car the timer thread starts - so that way we can kill the thread and timer. If it hasn't, then we can skip killing them.

Firstly, we want to increment the counter once you get in the car. This is done with the `INCCOUNT`;

```
1131 INCCOUNT 70 0 0 -1 0
```

The 70 represents the counter definition. So before the code gets to line 1131, counter 70 has a value 0. Then once we get to line 1131, counter 70 has a value of 0 plus 1, which is obviously 1. Now it's down to the bits where we need to kill the timer - the `DESTROY` thread and the `DEAD_ARRESTED` thread. What we want to do for each is check the counter, and remove the timer and the timer thread if counter 70 equals 1. If it equals 0, we don't. Here's the code I've added for the `DESTROY` thread;

```
1405 COMPARE 70 0 1410 1 0
1406 KILL_PROCESS 1134 0 0 0 0
1408 CANCEL_BRIEFING 1132 0 0 0 0
```

Using the `COMPARE` line, we check the value of counter 70. If it equals 1 (hence the person has got in the car, and the timer has started) then it goes on to the next line (hence the '0' in the second parameter). If it doesn't equal one (i.e. it equals 0) then we want to skip over the next two lines of code, going to line 1410. Lines 1406 and 1408 are just like what we did for the mission complete thread - kill the timer process (line 1606) and cancel the timer briefing (line 1608).

We do exactly the same for the other check - the dead/arrested thread;

```
1605 COMPARE 70 0 1610 1 0
1606 KILL_PROCESS 1134 0 0 0 0
1608 CANCEL_BRIEFING 1132 0 0 0 0
```

Decide whether to kill the timer or not, then kill the timer process and briefing if required.

Now the timer's setup, you may notice it's a bit too easy to complete the mission. Lets make it a bit more tricky, by moving the car further away and changing the car from a police car. I've changed it to;

```
50 (183,135,4) FUTURECAR 63 768
```

But play around and put it wherever you like.

As you can see, running multiple threads can get very complicated - and this is only for a one-job mission. We have made this hard for outselves - starting loads of checks all at different times - and you can see it's easy to run in to difficulties. However, it's very useful to know how to get around these problems, and I think using counters is a good way. Take a step back and look at the completed code, and make sure you understand how it all works.

You'll be glad to know that that's all were doing on multiple threads - we've covered just about all you'll need to know about starting and killing as many checks as you can think up. Ones that we haven't covered including killing other peds and letting a ped or car get out of your site. Over the next couple of days I'll be introducing a few neat tricks - although not required with this mission, but will give you a bit more knowledge to make your missions more versatile. Day 6 will cover the very useful `GET_DRIVER_INFO` command, one of the trickier ones to use in the MTD. We'll also be playing around with peds. Byeee!

## Day 6

Righto. On to day 6 and we're nearing the end of our tutorial. Today we'll be tackling peds. What we're gonna do is wait for the player to get near the car, then get a ped inside the car to get out and start shooting at him. As an added bonus, if you kill the guy you get a 'gratious violence bonus'.

First up, we need to put a guy inside the car. This is the easy bit - instead of using `PARKED_ON` (which turns on a `FUTURECAR`) we use `CAR_ON`, which turns on a futurecar with a ped inside. So I've replaced 1110 with;

```
1110 CAR_ON 50 0 0 0 0
```

Nice and easy. Now, we need a trigger, so that when the player gets in distance with the car the ped gets out. This is done using a trigger. The trigger line I've used is;

```
90 (183,135,4) TRIGGER 1800 4
```

Basically, when the player gets within a 4-square range of the set co-ordinates (the same as the car) then it will run the code in line 1800. At line 1800 we'll do the code to get the ped out of the car.

However first of all we need to disable this trigger. It's always important to disable your triggers before they're used - otherwise they could be doing another mission and it will trigger off. To do this, it's best to start a new thread right at the start of the game that is used to turn off all your triggers. To do this, we'll start a `KICKSTART` to line 30000;

```
1005 KICKSTART 30000 0 -1 -1 -1
```

Then, at 30000 we'll disable our trigger;

```
30000 DISABLE 90 0 0 0 0
31999 DONOWT 0 0 0 0 0
```

Line 31999 just stops the thread of code. This isn't really neccessary - we could just use -1's in the `DISABLE` to stop the flow of code - but it's useful in these situations because you'll be adding loads of `DISABLE`'s later in your mission writing, and you don't want to have to keep changing over the `-1`'s.

(There are a couple of exceptions to this rule. You don't have to disable any triggers related to objects that don't exist at the start - e.g. a `CARTRIGGER` linked to a `FUTURECAR`.)

Once that's done we now have to enable the trigger. This needs to be done once you get the telephone call. To enable a trigger, it's simply using the `ENABLE` command;

```
1115 ENABLE 90 0 0 0 0
```

Now that's done, we just need to do the code at line 1800 onwards. This is where our first problem arrises - how do we control the ped that's in the car? You'll notice that we haven't actually declared the ped - it was created when we did the `PARKED_ON` command. To get around this problem, we use the `GET_DRIVER_INFO` line. In the MTD it makes this line much more complicated than it needs to be - which is a shame because it's an extremely useful line. What it does is get the 'information' of the driver of a car, and store it in a `DUMMY`. We can then use the reference of the `DUMMY` to control the ped. To use this line, we first need a dummy to store it in, which is defined in the definitions;

```
80 (0,0,0) DUMMY 0 0
```

Once we've got the ped info, we can get him to get out of the car and start shooting at our ped. I'll show you the code I've done from line 1800 onwards, then explain it all;

```
1800 DISABLE 90 0 0 0 0
1803 GET_DRIVER_INFO 50 0 0 80 0
1805 REMAP_PED 80 0 0 186 0
1810 SURVIVE 0 0 0 10 0
1820 PED_OUT_OF_CAR 50 0 0 0 0
1830 CHANGE_PED_TYPE 80 0 -1 23 10
1840 PED_WEAPON 80 0 0 2 0
1850 IS_GOAL_DEAD 80 0 1850 10 5000
1860 MESSAGE_BRIEF 0 -1 -1 0 2516
```

The first thing you must do when using a trigger is to disable it (line 1800). Otherwise it will keep retriggering (the one exception to this rule is the trigger you use in conjunction with the `STARTUP` command, as this disables it automatically). When then use the `GET_DRIVER_INFO` line. `50` refers to the car we've created, and `80` refers to the `DUMMY`. It takes the 'information' of the driver of car `80`, and 'stores' it in dummy `80`. From now on, when we need to refer to the ped, we can use `80`.

Line 1805 remaps the ped - the default colour of a ped is a rather obvious yellow, so let's remap him to a different colour. Remap `186` makes him a nice blueish colour (there are various lists around the internet that list all the specific ped remaps if you want to get creative). Note how we've used `80` to refer to the ped.

`PED_OUT_OF_CAR` simply gets the driver of car `50` to get out of the car (not 'throw' out of the car, as the MTD states). We then want the ped to start shooting at the player, and so we use `CHANGE_PED_TYPE`. Peds can take loads of different types - shooting at peds, following peds, running away etc - and these are all nicely documented in the MTD. `80` refers to the ped that we want to change, and `23` is the type (in this case, a standing shooting ped with top accuracy). The `10` refers to the 'data' required for the specific ped type - in this case, the ped to shoot at. So we use `10`, to shoot at the player. Other types of data include the ped to follow or protect (again, the MTD covers all this nicely).

The default weapon for a shooting ped is a pistol, so lets spice things up a bit and give him a machine gun. You can give a ped any of the four weapons in the game; pistol, machine gun, grenade launcher and flame thrower, with numbers 1-4 resepectively (although I've found flame throwers don't work very well with peds). Using the line `PED_WEAPON`, we give the ped `80` gun `2` - a machine gun (line 1840).

As a little extra, I've set up a check to see if the guy gets killed, and if he does, we'll award them with a little bonus. To do this, I started up a loop using `IS_GOAL_DEAD`, that constantly loops around 1850 until the ped dies. As an added bonus with this code, you can set up a 'killer' - so if the player doesn't kill him (e.g. he gets run over) then they don't get the bonus. When the line goes to success (i.e. the player has killed the ped) then it goes to line 1860, to display the message. We use line 1850 to add the points (`5000`, in this case). Again, we use the -1's to stop the flow of the thread.

Now we've added the ped you'll note it's quite a bit harder to play it. To make it a bit easier you could give the ped only a pistol, or make him a worse shot (by choosing a different ped type). In our final tutorial we'll be a doing a couple more extras, like powerups and car remaps. C ya!

## Day 7

On to the last day, and there's just a couple of things we want tidy up before we finish. First up, now the missions finished, we can place all the mission points (the starting place, the phone, the car and the garage) to exactly where we want. I've moved the ped over to the left, and moved the car to a better place too. I've also chosen a slightly slower car to make it even harder;

```
10 (6,202,4) PLAYER 0 768
15 (6,202,4) TRIGGER 500 0
...
50 (198,114,4) FUTURECAR 31 256
```

Remember, whenever you move the player line, you also have to move the trigger line too, otherwise none of the code will start.

I've also tidied up the start code a bit too. It's usually a good idea to split each mission up every 1000 lines - i.e. mission 1 lines 1000-1999, mission 2 lines 2000-2999 etc. Also, to prepare for more missions in the future, I've kickstarted to the first mission. Here's the new start code;

```
500 STARTUP 15 0 -1 0 0
520 KICKSTART 30000 0 -1 -1 -1
530 KICKSTART 1000 -1 -1 -1 -1
......
1000 SURVIVE 0 0 0 30 0
```

How about giving the player a different colour? Yellow does get rather boring. To change his clothes, simply use the `REMAP_PED` line. In this case I've chosen a fetching blue colour;

```
510 REMAP_PED 10 0 0 164 0
```

Whilst we're on remaps, we can also remap the mission car. The car we've chosen doesn't remap with huge differences, but it does change in tint and it'll do for the example. Simply use;

```
1112 REMAP_CAR 50 0 0 1 0
```

The 1 represents the remap colour - play around with other values with different cars to get the one you want. Note we've done this after the `CAR_ON` line, otherwise we'll get an error message. Next up, lets give the player a powerup at the start. To define it you simply use;

```
110 (6,203,4) POWERUP 2 49
```

The 2 represents the gun type (remember 1 is pistol, 4 is flame thrower). 49 represents the ammo, with a maximum of 99 (after 99, it goes into a timer used for kill frenzies). POWERUP's are by default 'off', and so need to be turned on at the start of the game. We can do this in our handy routine in line 30,000;

```
30001 POWERUP_ON 110 0 0 0 0
```

This simply makes the powerup in line 110 be 'on' - that is, exist in the game world. Note this doesn't to the auto-resporn powerups - once you've taken it it won't come back. Resporning powerups can be the subject of another tutorial :) There's one last thing that I've changed before it's all completed: you may have noticed from testing through that if you get pulled out of the car by cops when you're arrested, you get the "get back in the car" message before the "dead/arrested" message. Not exactly a bug, but not very slick. To get around this problem, we just have to kill the thread of checking if you're in the car. Just like the timer check, we only want to kill it if we've go in to the car, so we put it within then COMPARE bit;

```
1607 KILL_PROCESS 1135 0 0 0 0
```

Right, we're done! Congratulations on making you're first, fully working, featured packed, mission. From here on you should be able to do any kind of mission you like - we've touched on just about everything you'll need to get going. Although naturally we haven't been able to go over every command you'll need, you should now have the experience to use them confindently. Just about all missions follow this basic formula - one mission thread, then side processes checking to see if you've done something wrong.

There are a couple of areas that we deliberately haven't gone deep into. We haven't used many `TRIGGER`s - nothing but the basic `CARTRIGGER` and `TRIGGER`. However they all work in a similar way, and you should have no problems using them. We also haven't used more advanced process control - that is, `NEXT_KICK` and `KILL_SPEC_PROC`. The main reason for this is that they aren't really required - they do make your mission easier to code once you understand them, and once you've got the basics under the belt then they're worth using, but if you don't then your missions won't suffer.

That's all for this tutorial. Good luck with your mission writing in the future!