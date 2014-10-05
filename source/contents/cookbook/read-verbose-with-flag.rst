======================
Read Verbose with Flag
======================
Even with #CONFIG VERBOSE ON and #DEBUG ALL enabled, this output is still suppressed if #read is called from within an alias.
We can get around this by prefixing the #READ command with #LINE VERBOSE. Unfortunately, if you have lots of files being read, 
then each time you want to enable this globally, you have to modify it throughout your entire configuration.

To make this a little more user-friendly, we can wrap the #read command in an alias that issues the command based on a global flag::

    #var READ_VERBOSE 1;
    #alias read {
        #if { "$READ_VERBOSE" == "0" } {#read %1};
        #else {#line verbose #read %1};
    }
