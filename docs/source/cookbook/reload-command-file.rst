===================
Reload Command File
===================
When testing code, it's desirable to be able to run the same code over and over after making small changes. I add the following alias to my "test" command files when to make this very fast::

    #alias reload {
        #kill {all};
        #showme <faa>Reloading ...;
        #read thisfile.tin;
    }

NOTE: The reloading is taking place inside of an alias, so verbose output will be disabled.
