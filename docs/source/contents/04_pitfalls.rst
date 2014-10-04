========
Pitfalls
========
Issues that commonly trip up users.


----------
Data Types
----------
The data type documentation is significantly lacking. The table data type is completely undocumented. I highly recommend making sure you understand them thoroughly first, as the parser is extremely particular about how data is stored and referenced - and will fail silently if anything is out of place.


-------------------------
If Statement Terminations
-------------------------
When a statement is on a line all by itself, you need to (in most cases) terminate it with a semicolon (;). This is sometimes not required for block statements, such as #IF statements::

    #if { condition } {
        commands
    }

The previous code works fine - most of the time. However, if this code is in an alias and that alias is called from within an #if block somewhere else, the code will break ... for that occurrence only. The practical solution is to terminate all these code blocks explicitly::

    #if { condition } {
        commands
    };


---------------
Silent Failures
---------------
When issuing #read commands from an alias, all debugging output is silenced, regardless of::

    #CONIG {VERBOSE} ON;
    #DEBUG {ALL} ON;

You will not see error messages from these files. This present a significant problem for me, as I do all my loading through my module manager.
You can force verbose output by prefixing the #read commands with #line verbose.

TODO: Test how #line log works with #line verbose.


-------------------
Variable Collisions
-------------------
There.is.no.scope. Variables using the same name in separate command files will overwrite each other. Beware.


---------------
Quoting Strings
---------------
When working with strings, quotes matter::

    #showme "Test";         # yields "Test"
    #showme Test;           # yields Test

These values are not equivalent. Typically, strings remain unquoted.
Note: This has tripped me up several times when passing data between the shell and tintin using #script.
Note: In some contexts, for example `switch statements <http://tintin.sourceforge.net/board/viewtopic.php?t=2214>`_, strings MUST be quoted.


-----------------
Switch Statements
-----------------
Arguments in switch statements must be quoted, otherwise you'll get insane alias argument stack frame behavior. See "Quoted Strings" above.


--------
Comments
--------
If you don't terminate a #nop statement with a semicolon it will "absorb" the subsequent line::

    #nop I'm just a comment
    #script {
        ....
    }

In this example, the script tag will be commented out and silently fail to execute.


-------------
Command Files
-------------
Command files must start with a #tintin command. If they don't, they'll fail to load. If you're loading the file through an alias it will silently fail to load.
To make sure this never happens, I start every file with:

    #nop VSOF;

And regardless of how I modify my code, that line is never changed.
Note: The error message displayed when this happens is "Invalid start of file." Hence VSOF (Valid Start of FIile).
