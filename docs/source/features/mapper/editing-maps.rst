===============
Editing the Map
===============

---------------
Dynamic Mapping
---------------
As covered in fundamentals, we need to disable the static flag to allow rooms to be created as navigate around the mud::

    #map flag static off

Once this is disabled, the mapper will automatically create rooms in the map as it interprets your movement.

NOTE: Dynamic mapping requires the #pathdir settings to be configured. The default are good when starting, but more can be done with these.

--------------
Manual Mapping
--------------
While the majority of mapping is typically done via dynamic mapping, there will be situations where you need to manually create or fix parts of the map.

To create an adjacent room, use the command::

    #map dig <direction>

This will insert a room in the specified direction, but not move you to it.

To insert a room between you and an adjacent room, use the command::

    #map insert <direction>

This will create a room between you and the room in the specified direction. This is used extensively for creating additional spaces to avoid overlap, and hidding entire map branches, covered in the following sections.

If you need to remove a room between your current room and the room on the other side of an adjacent room, you can use the command::

    #uninsert <direction>

While the seems trivial, it's very useful because it automatically updates room vnums for both rooms and reconnects them for you automatically. Doing this by hand is very tedious.

There are also times when you need to move your position around the map, but not on the mud. This is done with the command::

    #map move <direction>

This command is primarily used when the mapper gets out of sync with your actual location on the mud, especially while manually editing the map.


---------------------
Undoing Room Creation
---------------------
The mapper (out of the box) cannot tell when movement was prevented. It interprets your input and updates the map accordingly. One of the first things that happens in this situation, is that you'll attempt to move through a closed door or encounter some kind of situation that prevents the movement from occurring, but the mapper will create the room anyway.

There are several ways to recover from this situation, the first is with::

    #map undo

This will unwind the erroneous room creation and move you to your previous location in the map.
The second is to not unwind the room creation, but to move yourself back to your previous location in the map. This can be done with either of the commands::

    #map goto <vnum>
    #map move <direction>

If desired, you then delete the room that was created with::

    #map delete <direction>

-----------------
Overlapping Rooms
-----------------
Most muds aren't laid out in a perfect grid - there's lots of overlapping. How you design your map is a personal preference, but one of the basic tools is to hide branches from the map.

Void rooms help to solve spacing issues by creating an additional "space" between two adjacent rooms.

The most basic way of using this, is with #map insert, and specifying the roomflag void::

    #map insert <direction> void

This will move the room to <direction> over an existing space, creating the space needed to avoid overlapping rooms.


-------------------
Hiding Map Branches
-------------------
Sometimes, entire areas should be hidden from a view until they are entered. To do this, we use hidden void rooms. As previously covered, we can insert a void room with the command::

    #map insert <direction> void

Note the vnum of the room that was created, because we will need to set the hide flag on it as well::


To do this, you create a "void" room (this is basically a place marker in the map) and set it's hidden flag to 1. When you navigate to this room you'll pass over it in whatever direction you were already going.

There are several ways to create the rooms, the most straight forward is:

    #map insert <direction> void
    #map goto <vnum>
    #map roomflag hide
    #map move <previous direction>

You should now see the entire branch hidden from your current position.

NOTE: It does not appear possible to set both the hide and void roomflags at the same time when using the #insert command.
NOTE: THe #map return command does not appear to work to return from the void room to the previous location.

There are probably more efficient ways of doing this, but I don't know them.

