# `PARK` in GTA 1 Coding

Source: https://projectcerbera.com/gta/1/tutorials/park

One of the most common ways that missions end in GTA 1 is that you deliver the vehicle you are using to a garage. When you get near to the roller door outside the garage, it opens up allowing you to drive in. Once the vehicle is inside the garage, the player respawns outside the garage door facing away from it. The car which was parked is removed from the game environment and the roller door closes, preventing the player from entering again.

The `PARK` command is what detects when the car is inside the door and reacts by automatically respawning the player outside, removing the car and closing the door.

## Building the Garage

The longest vehicles in GTA 1 are 128 pixels long, which is the length of cubes in the map editor or about nine metres in real units. Therefore it is best to make your garages two or three cubes deep but they should only be one cube wide. If you make them two cubes wide you would need to use two `PARK` commands running in seperate processes which kill each other when the vehicle is parked…very complicated.

You do not actually have to make a garage object like this because the `DOOR` object automatically creates an empty cube. However, actually making the garage in the map editor allows you to create exactly the size, shape and style of garage that you want.

## Declaring the Objects

Now that you have your garage made you need to add the door. There are two types of door which you can use in each of the three GTA 1 levels.

### Grey Roller Door

```text
<line num> <reset> (x,y,z) DOOR <face> <type> <interior>
```

In detail, the parameters do this:

| Parameter | Range | Description |
| --- | --- | --- |
| `line num` | 0 to 32767 | Unique identifier for this object. |
| `reset` | 0 | Do not keep this object active after `RESET` commands. |
| `reset` | 1 | Keep this object active after `RESET` commands. |
| `x` | 0 to 255 | Position for this object from left edge of landscape. |
| `y` | 0 to 255 | Position for this object from top edge of landscape. |
| `z` | 0 to 5 | Position for this object from high edge of landscape. |
| `face` | 0 | Door will appear on West face of the cube. |
| `face` | 1 | Door will appear on East face of the cube. |
| `face` | 2 | Door will appear on North face of the cube. |
| `face` | 3 | Door will appear on South face of the cube. |
| `type` | 0 to ? | Sets the exterior style of the door. Check the original code to get the right style. |
| `interior` | 0 to 255 | Can be used to force an interior style. |

### Secondary Door

In some levels it is a yellow and black gate. In others it is a grey sliding door, a bit like a blast door:

```text
<line num> <reset> (x,y,z) BARRIER <face> <num frames> <type>
```

In detail, the parameters do this:

| Parameter | Range | Description |
| --- | --- | --- |
| `line num` | 0 to 32767 | Unique identifier for this object. |
| `reset` | 0 | Do not keep this object active after `RESET` commands. |
| `reset` | 1 | Keep this object active after `RESET` commands. |
| `x` | 0 to 255 | Position for this object from left edge of landscape. |
| `y` | 0 to 255 | Position for this object from top edge of landscape. |
| `z` | 0 to 5 | Position for this object from high edge of landscape. |
| `face` | 0 | Door will appear on West face of the cube. |
| `face` | 1 | Door will appear on East face of the cube. |
| `face` | 2 | Door will appear on North face of the cube. |
| `face` | 3 | Door will appear on South face of the cube. |
| `num frames` | 0 to 16 | The vehicle model or type number which is allowed access. |
| `type` | 0 to 255 | Sets the interior to this texture ID. Check the original code to get the right style. |

### Associated Objects

You must place a `DUMMY` object in the same location as the `DOOR` or `BARRIER` which you want to use `PARK` with. There are some other door objects which only open under specific conditions but I will not cover those here. Objects like `DOOR` are usually created with `<reset>` set to `1` so that the object does get removed after other missions.

## Using the Commands

To get the automatic door closing, player respawning and car removal to work you must use the `PARK` command very carefully. Here is the syntax:

```text
<line num> PARK <object> <succeed> <dummy> <face> <score>
```

Each parameter is defined here:

| Parameter | Range | Description |
| --- | --- | --- |
| `line num` | 0 to 32767 | Unique identifier for this command. |
| `object` | 0 to 32767 | Unique identifier of the `DOOR` or `BARRIER` object used by the garage. |
| `succeed` | 0 to 32767 | Unique identifier of the command to go to if this command has worked. |
| `dummy` | 0 to 32767 | Unique identifier of the `DUMMY` object in the garage. |
| `face` | 0 | Player respawns to the South of the garage. |
| `face` | 1 | Player respawns to the West of the garage. |
| `face` | 2 | Player respawns to the North of the garage. |
| `face` | 3 | Player respawns to the East of the garage. |
| `score` | 0 to 9999999 | Number of points to add onto the player’s score. |

## Code Sample

I have put together a small code sample to replace Liberty City Chapter One to demonstrate how the `PARK` command is used. You can copy it from here:

```ini
{Ben “Cerbera” Millard
http://www.projectcerbera.com
26th November 2004}

[1]
PARK Command Demonstration, 22, nyc.cmp, 0,
100 1 1 1 1 0

0 (0,0,0) DUMMY 0 0 {failsafe object}
2 1 (107,119,4) PARKED 22 512 {player’s start vehicle, a Beast GTS facing East}
4 1 (105,119,4) PLAYER 2 256 {creates the player character facing east, with ownership of vehicle 1}
6 1 (105,119,4) TRIGGER 10 255 {begins commands from command 10 once the level has loaded}

{Objects used in the PARK mission:}
10 (161,74,4) PARKED 02 128 {the Countash facing South-East which the player must steal}
12 1 (114,104,3) DOOR 1 11 7 {door to the garage the player must use}
14 (114,104,3) DUMMY 0 0 {used with the PARK command}

-1

0 DONOWT 0 -1 0 0 0 {failsafe command}

{Level start commands:}
10 STARTUP 6 0 -1 0 0 {disable object 6, which is the level start TRIGGER}
12 MOBILE_BRIEF 0 0 0 0 1002 {Crazy Jimmy’s dumped a car in North East Park. Find it before the cops do - or die.}
14 ARROWCAR 10 0 0 0 0 {point mission arrow at the Countash}
16 STEAL 10 0 0 0 1000 {wait until player gets into the Countash, then give player $1,000}
20 ARROW_OFF 0 0 0 0 0 {turn off mission arrow}
22 MOBILE_BRIEF 0 0 0 0 1003 {Bring the car to Dulli’s in West Park. Move it!}
24 DOOR_ON 12 0 0 0 0 {active the garage door ready to receive the Countash}
26 ARROW 12 0 0 0 0 {point arrow to the garage door}
28 PARK 12 0 14 3 0 {wait until the Countash is in the garage, then respawn the player outside and close the garage door}
30 ARROW_OFF 0 0 0 0 0 {turn off the mission arrow}
32 MESSAGE_BRIEF 0 0 0 0 2500 {MISSION COMPLETE!}
34 MISSION_END 0 0 0 0 10000 {tell the game engine this mission is over, then give the player $1,000}
36 DROP_WANTED_LEVEL 0 0 0 0 0 {stop police from chasing the player}
38 RESET 0 -1 0 0 0 {clear mission environment, then stop these commands}

-1
```

As this is just a demonstration of the `PARK` command I have not used a `FUTURECAR` object and have not used `KICKSTART` to run a `DESTROY` check for the car and a `DEAD_ARRESTED` check for the player. If you are writing proper missions for public release, you should do all that sort of stuff.
