# `SENDTO` in GTA 1 Coding

Source: https://projectcerbera.com/gta/1/tutorials/sendto

Yeah thats right folks, `SENDTO` is a perfectly solid command! Bear in mind it uses the same AI as the Fire Trucks and Ambulances and you realise that it is the best vehicle AI the mission file has to offer.

The key to `SENDTO` is setting the map setup. To make the vehicle drive from one place to another you will need to make sure that the road is continuous - if you have missed any vehicle directions it will not work. You must make sure your slopes are defined correctly, too.

Also something to note is at junctions it can sometimes be prudent to remove traffic that may create a crash with your bus. If you have the bus coming out of a 2-laner at full pelt across a 6-laner freeway there is a high chance it will get clouted by cross traffic. Therefore leave the vehicle direction for the bus route across, but remove the vehicle arows for 5-8 cubes to each side. Of course, this means you will not be able to use this stretch as a `SENDTO` route now - you either chance a carash or limit the routes I am afraid.

It is a good idea to start the vehicle in a map area defined as ‘Player Only’ but with traffic directions to prevent your car being spawned on top of a citizen vehicle. This precaution will also prevent it crashing into something when the first `SENDTO` command starts it moving.

## Enhancing Performance

To set the performance of your car when in `SENDTO` mode, use the GTACars **Specs** tab. The acceleration, braking and maximum speed (multiply by three to get it in mph) all effect the AI. The **tail slide** and some other parameters effect it too, I believe. The car will accelerate at the same rate until it reaches top speed. It is a good idea to set the acceleration low for high top speeds.

## Creating the Route

Once the map is set up ready, now you have to put in you navpoints, waypoints, checkpoints or whatever you want to call them. Using `DUM_MISSION_TRIG` objectss with a radius of `1` are your best bet for this as they define the vehicle you are using directly. It is also worth noting here that the vehicle has to be defined BEFORE the waypoints due to the way GTA 1 associates objects.

The trick to you waypoints is that the Z-axis coordinate has to be one above the logical level. So, if your car is on level four and you want it to drive along a flat road at that level to a waypoint, that waypoint has to be defined at level three.

## Engaging the AI

Now to get the vehicle started there are one of two methods:

1. Make the `PLAYER` get in and make the car drive itself, like being driven by a chauffer.
2. Use `DUMMYON` to create a driver. I prefer not to use `DUMMY_DRIVE_ON`.

Remember to `REMAP` the player at the end of the route if you are chauffer-driving it!

## Code Sample

I will publish the code I use here:

```ini
[1]
Guided Tour, 22, Pacific.cmp, 0,
100 1 1 1 1 0

0 1 (1,1,1) TRIGGER 30 255 {level start trigger}
1 1 (9024,2880,192) PARKED_PIXELS 66 520 {PLAYERs car}

100 1 (139,47,3) PARKED 9 512 {Tour Bus}
110 1 (132,43,2) DUM_MISSION_TRIG 60 1 100 {First waypoint}
118 1 (117,26,3) DUM_MISSION_TRIG 100 1 100 {second waypoint}
{Add more waypoint objects here.}

32100 1 (146,46,2) PLAYER 1 768 {the player}

-1

0 DONOWT 0 -1 0 0 0

30 STARTUP 0 28000 -1 0 0 {28000 DISABLEs all the triggers}
44 ARROWCAR 100 0 0 0 0 {car 100 is used for this}
50 STEAL 100 0 0 0 0 {wait until player enters it}
52 SPEECH_BRIEF 0 0 0 0 1002 {once stolen inform player}
54 ENABLE 110 0 0 0 0
56 DUMMYON 100 0 0 0 0 {DUMMYON prevents PLAYER interfering}
58 SENDTO 100 -1 0 110 0 {Begin route}

60 DISABLE 110 0 0 0 0 {disable this waypoint}
64 ENABLE 122 0 0 0 0 {enable next waypoint}
68 SENDTO 100 -1 0 122 0 {proceed to next}

{Add more waypoint commands here.}

120 DISABLE 122 0 0 0 0 {isable this waypoint}
362 MESSAGE_BRIEF 0 0 0 0 2500 {MISSION COMPLETE}
364 SPEECH_BRIEF 0 0 0 0 1009 {Explain mission over}
368 REMAP_PED 32100 0 0 0 0 {Change PLAYERs clothes back to normal}
398 DONOWT 0 -1 0 0 0 {Finish procedure}
```

That is being used in Pacific City and it is working every time now.

## Going Further

These are some things you may like to add as belt and braces:

- `CARSTUCK_TRIG`: If the vehicle crashes and gets wedged against a fence or suchlike you allow the player to get out, or the AI driver gets out and fights on foot.
- `DESTROY`: If the vehicle drives off a cliff and lands in the sea the hospital respawn engine does not interveen as you are the `DUMMYON`. Therefore detect the vehicle being destroyed, remap the player and use `PED_OUT_OF_CAR` or something to kill the ped manually. Or just use `EXPLODE` on the car or `PLAYER`.
