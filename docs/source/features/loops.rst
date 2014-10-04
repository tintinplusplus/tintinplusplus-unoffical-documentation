=====
Loops
=====
There are six commands in tintin for implementing different sorts of "loop" behavior:

1. Foreach: For each item in the specified list, retrieve the value and execute some command(s).
2. Forall: For all items in the specified list, execute some command. This command appears to be identical (except for the "goblin" syntax) to the #foreach command.
3. Loop: Loop from and to specified indexes (a traditional for loop).
4. While: Execute a set of command(s) until a specified condition is met.
5. Parse: Iterate over a string and for each character, execute some command.
6. Repeat: Execute some command a specified amount of times.

There are also two flow-control commands that can be used to alter the flow of the loops (though they appear to have identical behavior):

1. Break: Stop executing the current loop and resume execution at the end of the loop's control structure.
2. Continue: Stop executing the current loop and resume execution at the end of the loop's control structure.

-------
Goblins
-------
I honestly can't think of a better name "looping through lists" in TinTin. The syntax is completely nonstandard and doesn't fit into the same context as anything else in TinTin++.
I can never remember the syntax and have to look it up each time I write one.

From the foreach page of the official manual:
> To loop through all items in a list (or a nested variable) using the foreach command use $<list>[%*].

The forall loop uses a special "goblin" syntax for referencing the value of the current item: "&0".

Examples
========
Example syntax::

    #foreach {$mylist[%*]} {value} {
        #showme {$value};
    };

    #forall {$mylist[%*]} {
        #showme &0;
    };

    #loop {1} {5} {i} {
        #showme {$mylist[$i]};
    };

    #var {i} {5};
    #while {$i > 0} {
        #showme {$mylist[$i]};
        #math {cnt} {$cnt - 1};
    };

    #parse {a string of characters} {c} {#showme $c};

     #10 #showme {repeat};

References
==========

- `The Break Command                                <http://tintin.sourceforge.net/manual/break.php>`_
- `The Continue Command                             <http://tintin.sourceforge.net/manual/continue.php>`_
- `The Forall Command                               <http://tintin.sourceforge.net/manual/forall.php>`_
- `The Foreach Command                              <http://tintin.sourceforge.net/manual/foreach.php>`_
- `The Loop Command                                 <http://tintin.sourceforge.net/manual/loop.php>`_
- `The Parse Command                                <http://tintin.sourceforge.net/manual/parse.php>`_
- `The Repeat Command                               <http://tintin.sourceforge.net/manual/repeat.php>`_
- `The While Command                                <http://tintin.sourceforge.net/manual/while.php>`_
- `Stumped on Nested List                           <http://tintin.sourceforge.net/board/viewtopic.php?t=1930>`_
- `Retrieving keys from associative array variables <http://tintin.sourceforge.net/board/viewtopic.php?t=1578>`_
- `Iterating Lists with foreach                     <http://tintin.sourceforge.net/board/viewtopic.php?t=1303>`_
- `how do same thing like ismember                  <http://tintin.sourceforge.net/board/viewtopic.php?t=1720>`_
