# Smallest `mission.ini` for GTA 1 Coding

Source: https://projectcerbera.com/gta/1/tutorials/smallest

If you have made a new city you will probably want to remove the original missions because they will not match your new landscape. If you just remove everything from `mission.ini` - or even delete the file - the game will not load. I have written the minimum code required to make a level load with the player, a few cars and the opening "Grrrrand Theft Auto!" message.

You can download the code as a file inside a winRAR archive (`smallest.ini`), or you can copy the code from here:

```ini
{Demonstration of smallest mission code.}

[1]
Easy Liberty City NEW, 22, nyc.cmp, 0,
100 1 1 1 1 0

{Objects Section}
0 1 (0,0,0) DUMMY 0 0 {safety object in case of bad parameter}
1 1 (127,127,0) TRIGGER 100 128 {startup TRIGGER covers entire map}
2 1 (0,0,0) PARKED 04 000 {player start vehicle}

{Level Setup:}
32000 1 (0,0,0) MISSION_COUNTER 4 0 {dunno}
32002 1 (0,0,0) SECRET_MISSION_COUNTER 0 0 {number of secrets in city}
32004 1 (0,0,0) MISSION_TOTAL 12 0 {total number of missions}
32006 1 (0,0,0) TARGET_SCORE 100000 0 {number of points to complete level}

32100 1 (0,0,0) PLAYER 2 0 {player owns object #2, the start vehicle}

-1

{Commands Section:}
0 DONOWT 0 -1 0 0 0

{Startup Commands:}
STARTUP 1 0 -1 0 0 {activates the mission environment}
DONOWT 0 -1 0 0 0 {stops these commands}

-1

[2]
Easy Liberty City NEW, 22, nyc.cmp, 0,
100 1 1 1 1 0

{Objects Section}
0 1 (0,0,0) DUMMY 0 0 {safety object in case of bad parameter}
1 1 (127,127,0) TRIGGER 100 128 {startup TRIGGER covers entire map}
2 1 (0,0,0) PARKED 04 000 {player start vehicle}

{Level Setup:}
32000 1 (0,0,0) MISSION_COUNTER 4 0 {dunno}
32002 1 (0,0,0) SECRET_MISSION_COUNTER 0 0 {number of secrets in city}
32004 1 (0,0,0) MISSION_TOTAL 12 0 {total number of missions}
32006 1 (0,0,0) TARGET_SCORE 100000 0 {number of points to complete level}

32100 1 (0,0,0) PLAYER 2 0 {player owns object #2, the start vehicle}

-1

{Commands Section:}
0 DONOWT 0 -1 0 0 0

{Startup Commands:}
STARTUP 1 0 -1 0 0 {activates the mission environment}
DONOWT 0 -1 0 0 0 {stops these commands}

-1

[1001]
DEATHMATCH1, 22, nyc.cmp, 0,
100 1 1 1 1 0

0 1 (136,77,4) PARKED 0 0
1 1 (157,84,4) PARKED 21 256
2 1 (166,60,4) PARKED 28 0
3 1 (185,70,4) PARKED 22 512

4 1 (135,77,4) PLAYER 0 256
5 1 (157,85,4) PLAYER 1 512
6 1 (165,60,4) PLAYER 2 256
7 1 (186,70,4) PLAYER 3 768

8 1 (135,77,4) TRIGGER 1 3

-1

0 DONOWT 0 0 0 0 0
1 DISABLE 8 0 0 0 0

-1

[1002]
Canonball Run1, 22, nyc.cmp, 0,
100 1 1 1 1 0

0 1 (136,77,4) PARKED 0 0
1 1 (157,84,4) PARKED 21 256
2 1 (166,60,4) PARKED 28 0
3 1 (185,70,4) PARKED 22 512

4 1 (135,77,4) PLAYER 0 256
5 1 (157,85,4) PLAYER 1 512
6 1 (165,60,4) PLAYER 2 256
7 1 (186,70,4) PLAYER 3 768

8 1 (135,77,4) TRIGGER 1 3

-1

0 DONOWT 0 0 0 0 0
1 DISABLE 8 0 0 0 0

-1

[1003]
Canonball Run2, 22, nyc.cmp, 0,
100 1 1 1 1 0

0 1 (136,77,4) PARKED 0 0
1 1 (157,84,4) PARKED 21 256
2 1 (166,60,4) PARKED 28 0
3 1 (185,70,4) PARKED 22 512

4 1 (135,77,4) PLAYER 0 256
5 1 (157,85,4) PLAYER 1 512
6 1 (165,60,4) PLAYER 2 256
7 1 (186,70,4) PLAYER 3 768

8 1 (135,77,4) TRIGGER 1 3

-1

0 DONOWT 0 0 0 0 0
1 DISABLE 8 0 0 0 0

-1

[1004]
Canonball Run3, 22, nyc.cmp, 0,
100 1 1 1 1 0

0 1 (136,77,4) PARKED 0 0
1 1 (157,84,4) PARKED 21 256
2 1 (166,60,4) PARKED 28 0
3 1 (185,70,4) PARKED 22 512

4 1 (135,77,4) PLAYER 0 256
5 1 (157,85,4) PLAYER 1 512
6 1 (165,60,4) PLAYER 2 256
7 1 (186,70,4) PLAYER 3 768

8 1 (135,77,4) TRIGGER 1 3

-1

0 DONOWT 0 0 0 0 0
1 DISABLE 8 0 0 0 0

-1

[102]
San Andreas medium, 22, sanb.cmp, 0,
100 1 1 1 1 0

{Objects Section}
0 1 (0,0,0) DUMMY 0 0 {safety object in case of bad parameter}
1 1 (127,127,0) TRIGGER 100 128 {startup TRIGGER covers entire map}
2 1 (0,0,0) PARKED 04 000 {player start vehicle}

{Level Setup:}
32000 1 (0,0,0) MISSION_COUNTER 4 0 {dunno}
32002 1 (0,0,0) SECRET_MISSION_COUNTER 0 0 {number of secrets in city}
32004 1 (0,0,0) MISSION_TOTAL 12 0 {total number of missions}
32006 1 (0,0,0) TARGET_SCORE 100000 0 {number of points to complete level}

32100 1 (0,0,0) PLAYER 2 0 {player owns object #2, the start vehicle}

-1

{Commands Section:}
0 DONOWT 0 -1 0 0 0

{Startup Commands:}
STARTUP 1 0 -1 0 0 {activates the mission environment}
DONOWT 0 -1 0 0 0 {stops these commands}

-1

[103]
San Andreas hard, 22, sanb.cmp, 0,
100 1 1 1 1 0

{Objects Section}
0 1 (0,0,0) DUMMY 0 0 {safety object in case of bad parameter}
1 1 (127,127,0) TRIGGER 100 128 {startup TRIGGER covers entire map}
2 1 (0,0,0) PARKED 04 000 {player start vehicle}

{Level Setup:}
32000 1 (0,0,0) MISSION_COUNTER 4 0 {dunno}
32002 1 (0,0,0) SECRET_MISSION_COUNTER 0 0 {number of secrets in city}
32004 1 (0,0,0) MISSION_TOTAL 12 0 {total number of missions}
32006 1 (0,0,0) TARGET_SCORE 100000 0 {number of points to complete level}

32100 1 (0,0,0) PLAYER 2 0 {player owns object #2, the start vehicle}

-1

{Commands Section:}
0 DONOWT 0 -1 0 0 0

{Startup Commands:}
STARTUP 1 0 -1 0 0 {activates the mission environment}
DONOWT 0 -1 0 0 0 {stops these commands}

-1

[1101]
DEATHMATCH1, 22, sanb.cmp, 0,
100 1 1 1 1 0

0 1 (136,77,4) PARKED 0 0
1 1 (157,84,4) PARKED 21 256
2 1 (166,60,4) PARKED 28 0
3 1 (185,70,4) PARKED 22 512

4 1 (135,77,4) PLAYER 0 256
5 1 (157,85,4) PLAYER 1 512
6 1 (165,60,4) PLAYER 2 256
7 1 (186,70,4) PLAYER 3 768

8 1 (135,77,4) TRIGGER 1 3

-1

0 DONOWT 0 0 0 0 0
1 DISABLE 8 0 0 0 0

-1

[1102]
Canonball Run1, 22, sanb.cmp, 0,
100 1 1 1 1 0

0 1 (136,77,4) PARKED 0 0
1 1 (157,84,4) PARKED 21 256
2 1 (166,60,4) PARKED 28 0
3 1 (185,70,4) PARKED 22 512

4 1 (135,77,4) PLAYER 0 256
5 1 (157,85,4) PLAYER 1 512
6 1 (165,60,4) PLAYER 2 256
7 1 (186,70,4) PLAYER 3 768

8 1 (135,77,4) TRIGGER 1 3

-1

0 DONOWT 0 0 0 0 0
1 DISABLE 8 0 0 0 0

-1

[1103]
Canonball Run2, 22, SANB.cmp, 0,
100 1 1 1 1 0

0 1 (136,77,4) PARKED 0 0
1 1 (157,84,4) PARKED 21 256
2 1 (166,60,4) PARKED 28 0
3 1 (185,70,4) PARKED 22 512

4 1 (135,77,4) PLAYER 0 256
5 1 (157,85,4) PLAYER 1 512
6 1 (165,60,4) PLAYER 2 256
7 1 (186,70,4) PLAYER 3 768

8 1 (135,77,4) TRIGGER 1 3

-1

0 DONOWT 0 0 0 0 0
1 DISABLE 8 0 0 0 0

-1

[1104]
Canonball Run3, 22, sanb.cmp, 0,
100 1 1 1 1 0

0 1 (136,77,4) PARKED 0 0
1 1 (157,84,4) PARKED 21 256
2 1 (166,60,4) PARKED 28 0
3 1 (185,70,4) PARKED 22 512

4 1 (135,77,4) PLAYER 0 256
5 1 (157,85,4) PLAYER 1 512
6 1 (165,60,4) PLAYER 2 256
7 1 (186,70,4) PLAYER 3 768

8 1 (135,77,4) TRIGGER 1 3

-1

0 DONOWT 0 0 0 0 0
1 DISABLE 8 0 0 0 0

-1

[202]
Vice is Painless, 22, miami.cmp, 0,
100 1 1 1 1 0

{Objects Section}
0 1 (0,0,0) DUMMY 0 0 {safety object in case of bad parameter}
1 1 (127,127,0) TRIGGER 100 128 {startup TRIGGER covers entire map}
2 1 (0,0,0) PARKED 04 000 {player start vehicle}

{Level Setup:}
32000 1 (0,0,0) MISSION_COUNTER 4 0 {dunno}
32002 1 (0,0,0) SECRET_MISSION_COUNTER 0 0 {number of secrets in city}
32004 1 (0,0,0) MISSION_TOTAL 12 0 {total number of missions}
32006 1 (0,0,0) TARGET_SCORE 100000 0 {number of points to complete level}

32100 1 (0,0,0) PLAYER 2 0 {player owns object #2, the start vehicle}

-1

{Commands Section:}
0 DONOWT 0 -1 0 0 0

{Startup Commands:}
STARTUP 1 0 -1 0 0 {activates the mission environment}
DONOWT 0 -1 0 0 0 {stops these commands}

-1

[203]
Vice is Painless, 22, miami.cmp, 0,
100 1 1 1 1 0

{Objects Section}
0 1 (0,0,0) DUMMY 0 0 {safety object in case of bad parameter}
1 1 (127,127,0) TRIGGER 100 128 {startup TRIGGER covers entire map}
2 1 (0,0,0) PARKED 04 000 {player start vehicle}

{Level Setup:}
32000 1 (0,0,0) MISSION_COUNTER 4 0 {dunno}
32002 1 (0,0,0) SECRET_MISSION_COUNTER 0 0 {number of secrets in city}
32004 1 (0,0,0) MISSION_TOTAL 12 0 {total number of missions}
32006 1 (0,0,0) TARGET_SCORE 100000 0 {number of points to complete level}

32100 1 (0,0,0) PLAYER 2 0 {player owns object #2, the start vehicle}

-1

{Commands Section:}
0 DONOWT 0 -1 0 0 0

{Startup Commands:}
STARTUP 1 0 -1 0 0 {activates the mission environment}
DONOWT 0 -1 0 0 0 {stops these commands}

-1 

[1201]

DEATHMATCH1, 22, miami.cmp, 0,
100 1 1 1 1 0 

0 1 (136,77,4) PARKED 0 0
1 1 (157,84,4) PARKED 21 256
2 1 (166,60,4) PARKED 28 0
3 1 (185,70,4) PARKED 22 512

4 1 (135,77,4) PLAYER 0 256
5 1 (157,85,4) PLAYER 1 512
6 1 (165,60,4) PLAYER 2 256
7 1 (186,70,4) PLAYER 3 768

8 1 (135,77,4) TRIGGER 1 3

-1

0 DONOWT 0 0 0 0 0
1 DISABLE 8 0 0 0 0

-1

[1202]
100 1 1 1 1 0

0 1 (136,77,4) PARKED 0 0
1 1 (157,84,4) PARKED 21 256
2 1 (166,60,4) PARKED 28 0
3 1 (185,70,4) PARKED 22 512

4 1 (135,77,4) PLAYER 0 256
5 1 (157,85,4) PLAYER 1 512
6 1 (165,60,4) PLAYER 2 256
7 1 (186,70,4) PLAYER 3 768

8 1 (135,77,4) TRIGGER 1 3

-1

0 DONOWT 0 0 0 0 0
1 DISABLE 8 0 0 0 0

-1

[1203]
Canonball Run2, 22, miami.cmp, 0,
100 1 1 1 1 0

0 1 (136,77,4) PARKED 0 0
1 1 (157,84,4) PARKED 21 256
2 1 (166,60,4) PARKED 28 0
3 1 (185,70,4) PARKED 22 512

4 1 (135,77,4) PLAYER 0 256
5 1 (157,85,4) PLAYER 1 512
6 1 (165,60,4) PLAYER 2 256
7 1 (186,70,4) PLAYER 3 768

8 1 (135,77,4) TRIGGER 1 3

-1

0 DONOWT 0 0 0 0 0
1 DISABLE 8 0 0 0 0

-1

[1204]
Canonball Run3, 22, miami.cmp, 0,
100 1 1 1 1 0

0 1 (136,77,4) PARKED 0 0
1 1 (157,84,4) PARKED 21 256
2 1 (166,60,4) PARKED 28 0
3 1 (185,70,4) PARKED 22 512

4 1 (135,77,4) PLAYER 0 256
5 1 (157,85,4) PLAYER 1 512
6 1 (165,60,4) PLAYER 2 256
7 1 (186,70,4) PLAYER 3 768

8 1 (135,77,4) TRIGGER 1 3

-1

0 DONOWT 0 0 0 0 0
1 DISABLE 8 0 0 0 0

-1
```

You can now test out custom levels easily by replacing the map filename parameter.
