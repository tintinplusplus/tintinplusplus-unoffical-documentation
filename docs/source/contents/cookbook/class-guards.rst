============
Class Guards
============
When storing configuration and settings in command files, you can wrap them in "class guards" to ensure that the old code is destroyed each time the is reloaded::

    #class {MYCLASS} {kill};
    #class {MYCLASS} {open};
    ...
    #class {MYCLASS} {close};
