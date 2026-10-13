# Comments in GTA 1 Coding

Source: https://projectcerbera.com/gta/1/tutorials/comments

A comment is any string of letters and numbers which the processor should ignore. Adding comments to your code is very useful for many reasons which is why nearly all programming languages allow comments to be added.

In GTA 1, the comments in the `mission.ini` file were removed before it was sold. This is because the file would have been too big if the comments were left in, so they removed the comments to reduce the filesize. This allowed the minimum system requirements for GTA 1 to be lowered.

## Adding Your Own Comments

There is only one way to add comments in the `mission.ini` file but it is very powerful. Here is an example of what comments can be used:

```ini
Vice is Painless, 22, miami.cmp, 0,
100 1 1 1 1 0


{Vice Beach mission phones:}
0 1 (243,61,4) TELEPHONE 0 0
1 1 (243,62,4) TELEPHONE 0 0
2 1 (243,64,4) TELEPHONE 0 0
3 1 (243,65,4) TELEPHONE 0 0

{Vice Shores mission phones:}
4 1 (144,116,4) TELEPHONE 0 256
5 1 (145,116,4) TELEPHONE 0 256
6 1 (144,118,4) TELEPHONE 0 768
7 1 (145,118,4) TELEPHONE 0 768

{-- Player starts at this position. These three TRIGGER objects; each begin a thread of commands starting at the line declared:}
171 1 (9,19,4) TRIGGER 32000 0 {begin commands from line 32000, which is the "Target Score" checking process}
172 1 (9,19,4) TRIGGER 10 0 {begin commands from line 10 which includes the opening messages and the ringing telephone selection animations}
173 1 (9,19,4) TRIGGER 30000 0 {begin commands from line 30000, used for creating all the POWERUP objects and disabling TRIGGERs which are not needed at the start}
```

In that section of code, everything which is between a `{` and a `}` will be ignored by GTA 1, even the hyphens (`-`) and colons (`:`). This has allowed me to add a description above groups of related objects, such as the sets of four `TELEPHONE` objects, as well as descriptions beside objects, such as the `TRIGGER` objects. By adding such a detailed comment beside things like `TRIGGER` objects you will not have to remember what code is being run at the line they are referring to.

## Ignoring Code

As well as using comments to describe the objects and commands you have written, you can use comments to make GTA 1 ignore things whilst you are testing.

For example, you might write some commands like this:

```ini
-1

{Commands Begin}
10 STARTUP 1 0 -1 0 0 {Begins the command processor}
12 REMAP_PED 33000 0 0 133 0 {Casual clothes for player}
14 KICKSTART 20000 16 -1 -1 -1 {Branch to commands from 20000}
16 ARROW 10 -1 -1 0 0 {Point arrow at object 10, then stop these commands}
```

If we decide that we want to disable the `REMAP_PED` command (line #12) whilst we test something, we could put a `{` in front and a `}` afterwards, like this:

```ini
-1

{Commands Begin}
10 STARTUP 1 0 -1 0 0 {Begins the command processor}
{12 REMAP_PED 33000 0 0 133 0} {Casual clothes for player}
14 KICKSTART 20000 16 -1 -1 -1 {Branch to commands from 20000}
16 ARROW 10 -1 -1 0 0 {Point arrow at object 10, then stop these commands}
```

GTA1 would now ignore the `REMAP_PED` command because it has been turned into a comment.

## Multiple Lines

The last thing you might find useful is that comments can contain new lines and blank lines, like this:

```ini
-1

{Commands Begin}
10 STARTUP 1 0 -1 0 0 {Begins the command processor}
{12 REMAP_PED 33000 0 0 133 0} {Casual clothes for player}
14 KICKSTART 20000 16 -1 -1 -1 {Branch to commands from 20000}
16 ARROW 10 -1 -1 0 0 {Point arrow at object 10, then stop these commands}
{This is the start of a comment which will include new line.
Here is a new line, part of the same comment.


This is still the same comment. It only ends when the wiggle bracket is closed.}
```
