# Mission Cleanup for GTA 1 Coding

Source: https://projectcerbera.com/gta/1/tutorials/mission-cleanup

Once a mission is over you should always clear up the mission environment. If you do not do this, mission objects will keep accumulating and can prevent the game from creating normal cars and pedestrians properly. The ending of every mission should have a `RESET` command which automatically erases every mission object from the game environment unless it has `1` placed before the coordinates, after the line number.

You should also use `MISSION_END` and use the proper `MESSAGE_BRIEF` command to indicate a failed or completed mission or kill frenzies. The cool sounds and announcements are hardcoded into the message numbers.

## Code Sample

It is best to finish off a completed mission using a set of commands like this:

```ini
{Player has done what the mission required:}
990 RESET 0 0 -1 -1 0 {clear mission environment}
992 MISSION_END 0 0 0 0 0 {indicate to the game engine that this mission is over}
994 MESSAGE_BRIEF 0 0 0 0 2500 {MISSION COMPLETE!}
996 DECCOUNT 9000 0 0 0 0 {decrement the MISSION_COUNTER object}
998 DECCOUNT 23600 0 0 0 0 {zero the COUNTER object used to indicate if player is on a mission}
```

This code could be placed after something like a `PARK` command so that when the player delivered the vehicle to that garage, the mission would be completed. If the player fails the mission you should use the same code but with `2501` as the `MESSAGE_BRIEF` entry to use.
