=========================================
Displaying the Map in a Separate Terminal
=========================================
Rather than displaying in a split, we can get a much more enjoyable experience by displaying the map in a separate window.

The easiest way to do this is to have the MAP ENTER ROOM event write the map to a separate file, like this::

    #event {MAP ENTER ROOM} {
        #map map 16x16 map.txt {a};
    };

Note: The {a} flag appends to the file, rather than overwriting it, so that we can use tail of view it whenever it changes. This can generate very large files.

And then we can view the file in a separate window with tail -F ::

    tail -n 16 -f map.txt

I've had mixed results with this approach - depending on the system and implimentation of tail, this may or may not work.

Rather than use this approach, we can get much better behavior by using the following bash script::

    #!/usr/bin/env bash

    MAP_FILE='map.txt'
    MAP_SIZE='map_size.tin'
    REFRESH_RATE=.25

    while [ "true" ]; do
        echo \#var MAP_ROWS $(tput cols)\; > $MAP_SIZE
        echo \#var MAP_LINES $(tput lines)\; >> $MAP_SIZE
        clear
        cat $MAP_FILE
        sleep $REFRESH_RATE
    done

This redisplays the file every .25 seconds, which on my system seems to be the fastest refresh rate that there is nearly no noticeable lag between my movement in TinTin and the map being updated.  Additionally, it writes a small tintin file, which contains two variable definitions - the number of columns and number lines in the window displaying the map.

In our map event hook, we can then have it write the perfectly sized map, by simply reading this tintin file and using those values to update the size of the map that is displayed::

    #event {MAP ENTER ROOM} {
        #read map_size.tin;
        #map map ${MAP_ROWS}x${MAP_LINES} map.txt;
    };

