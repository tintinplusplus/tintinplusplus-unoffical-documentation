===============
Getting Started
===============


--------------
Creating a Map
--------------
To create a map, use::

    #map create <size>

The size argument specifies the maximum number of rooms the map can hold (this can be increased later, if necessary). I do not believe the maximum number of rooms affects the map performance (though the actual number of rooms absolutely does), so I typically create my maps with a very large number of maximum rooms (1,000,000). If no size is specified, the default value of 50,000 is used.


--------------------------------
Modifying the Maximum Room Count
--------------------------------
To modify the maximum number of rooms, use::

    #map resize <number>

I haven't ever had to do this, because I always create my maps with a starting size of 1,000,000 rooms (and I haven't gone over this number). As mentioned above, the maximum number of rooms does not affect map performance, so there are no drawbacks to doing this.


----------------
Destroying a Map
----------------
A map that is loaded can be destroyed with the command::

    #map destroy

This will remove it from memory and make us leave the map, but it will not affect a map file from which the map was loaded.

This is useful if you've read a map file into memory, made some mistakes, and want to start fresh.


---------------------------
Temporarily Leaving the Map
---------------------------
We can exit the map without destroying it (leaving it loaded in memory) with the command::

    #map leave

This is useful when entering a maze or another room where the mapper will not work correctly.


------------------------
Saving the Map to a File
------------------------
Once a map has been created in memory it can be saved for later use with::

    #map write <file>

Be very careful when saving your map files, this command will overwrite any file without warning. I have done this in the past (after a long session and very tired). I accidentally overwrote my command file rather than updating the map file.

To ensure this doesn't happen, I recommend creating an alias that always saves the file to the same place::

    #alias mapsave {
        #showme "Map saved.";
        #map write mymap.map;
    }

Two other tips when saving map files:

- You can make your map save alias automatically backup the previous map first using the #script command.
- You can setup automatic map saving using the #ticker command.


---------------------------
Loading the Map from a File
---------------------------
Maps that have been saved to a file can be loaded with the command::

    #map read <file>

Just like the #map create command, this will not automatically insert you into the map.


-------------------------------
Setting Our Position in the Map
-------------------------------
To set our position in the map, use the command::

    #map goto <vnum>

This will tell the mapper that we are at position <vnum> in the map. The goto command does not send any commands to the mud and therefore does not actually move us around. This is strictly used for telling the mapper where our position is.

If our map file is empty, the goto command will create the first room in the map. Once our map is populated, we can set our position to any room in the map using this command. Again, this doesn't actually move our character on the mud - it tells the mapper where our position in the map is.

This is mostly used when our actual position gets out of sync from where the mapper believes we are.

A quick note about vnums:
Vnum's are unique integer's that function as an index for each room in our map file. When starting with an empty map (right after #map create, or if using #map read on an empty file), we have no rooms, so this will create the first.

------------------
Displaying the Map
------------------
If you've gone through this far, you're probably wondering where the map is. There are an infinite amount of ways to display the map, but we're only going to cover the most basic methods here.

The most basic way to display the map is through the command::

    #map map

This command is used in other ways as well, but for our purposes right now, this will display the map.
Of course, we don't want to have to type a command each time we want to display the map, we'd rather have always be displayed and updated.

Again, there are lots of ways to do this, and we're just going to cover the most basic here.
TinTin provides a #split command, that allows us to split the screen. This is used for two primary purposes:

1. Separating the input field from the buffer
2. A very simple map display.

We're going to use it for both::

    #split 16 1

You should now have a separate input field and 16 dashed lines at the top of the terminal window. In order to display the maps in the split, we need to use the command::

    #map flag vtmap on

The #map flag vtmap on command takes advantage of this split to draw the map, only if the split is created and the flag is enabled. The map it draws is 16 rows high, and I don't believe there is a way to change this. Setting a larger height for the top split results in additional empty rows being displayed, but not a larger map display area.

After creating the splits, the buffer window is drawn differently than before. If you attempt to scroll up, you're going to see the buffer content you're looking for. Depending on your terminal you may need to use a different combination of keys to scroll correctly. On my OS X system in iTerm2, this is the function and up/down arrows.

----------------
Updating the Map
----------------
If you move around, you'll notice that the map is not updated - this is because the #map flag static flag is set and no other rooms exists (assuming you've got a brand new map with only 1 room).

We can disable this flag with the command::

    #map flag static off

Now if you move around, you'll see that new rooms are created.
