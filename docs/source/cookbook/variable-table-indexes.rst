========================================
Retrieving Table Indexes from a Variable
========================================
Retrieve a list of indexes from any reference point within a nested variable you must use the table index operator "[]"::

    #var MODULES {
        {module}  { {loaded} {1} {path} {modules/module/__init__.tin}  }
        {profile} { {loaded} {1} {path} {modules/profile/__init__.tin} }
        {session} { {loaded} {1} {path} {modules/session/__init__.tin} }
    };

    #showme $MODULES[];
    {module}{profile}{session}

Note: You cannot retrieve this list of indexes and store them in a variable, they will be parsed as a table rather than a brace delimited list. The only place this works is within the context of a #foreach loop input.
