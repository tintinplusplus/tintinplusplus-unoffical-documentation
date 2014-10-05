==========
Data Types
==========
There are several different data types in tintin, several of which are undocumented. They must be used in specific ways that vary by context, so it can get tricky:

- Simple variables: The basic building block for all data types.
- String lists: A simple variable, used in the context of a foreach loop that contains semicolon delimited fields.
- Brace lists: A brace delimited type that is only valid in the context of a foreach loop input.
- Nested variables: A variable containing another variable. Try not to think of this as arrays, this will lead you to dark places.
- Lists: A complex data type that uses integers for it's indexes. An API is provided for working with this data type.
- Tables: A complex data type consisting of name/value pairs.


----------------
Simple Variables
----------------
A simple variable::

    #var SIMPLE_VARIABLE value;

These are self explanatory, enough said.


------------------
Removing Variables
------------------
To remove a variable, use #unvariable (or #unvar).
When planning out your data storage, keep in mind that you can deeply nest variables & tables in the same "root" variable, then remove the entire thing at once, using #unvar nested_data;

You cannot remove multiple variables at once, i.e::

    #unvar var1 var2 var3;

You must remove them individually::

    #unvar var1;
    #unvar var2;
    #unvar var3;

This is worth considering when you design your data structures.


------------
String Lists
------------
A string list is a simple variable that contains a string with semicolons that can be interpretted in the context of a foreach loop. From #help foreach::

    #foreach {bob;tim;kim} {name} {tell $name Hello}


-----------
Brace Lists
-----------
Brace lists confused me pretty bad. They are only valid in the context of a forloop and cannot be stored in a variable. Again, from #help foreach::

    #foreach {{bob}{tim}{kim}} {name} {tell $name Hello}

The format for these lists is extremely similar to that of a "table" (more on tables later).

If you attempt to use the brace list syntax outside of the context of a forloop input, you'll get confusing behavior::

    #var list {{bob}{tim}{kim}};
    #OK. VARIABLE {list} HAS BEEN SET TO {{bob}{tim}{kim}{}}.

The parser created the "list" but added an extra {} at the end. This is because the parser is interpreting the input as a table, and tables store key/value pairs. So to recap - you cannot create "brace lists" outside of the scope of a #foreach input. 

----------------
Nested Variables
----------------
Variables can be nested using either "array" or "dot" syntax. Both of the following forms are syntactically correct::

    #var {V1.N1.N2}   {VALUE};
    > #VARIABLE {V1}={{N1}{{N2}{VALUE}}}

    #var {V2[N1][N2]} {VALUE};
    > #VARIABLE {V2}={{N1}{{N2}{VALUE}}}

Note the syntax, it's important. The exploded representation::

    #VAR V1
    {
        {N1}
        {
            {N2}{VALUE}
        }
    }

Each "nested" variable is encapsulated with an extra set of braces.
Note: I've run into some situations where whitespace in nested variables and tables matter, so don't define them in your code using the exploded form above (no extra whitespace!).

You can retrieve the values from nested lists using either syntax::

    #showme $V1.N1.N2;
    > VALUE

    #showme $V1[N1][N2];
    > VALUE

And in some cases (though I don't know why you'd want to) you can mix the "array" and "dot" syntax::

    #nop This works.;
    #showme $V1.N1[N2];
    > VALUE

    #nop This does not work.;
    #showme $V1[N1].N2};
    > {N2}{VALUE}.N2};

-----
Lists
-----
Lists are essentially integer indexed arrays::

    #list list add item1;
    #list list add item2;
    #list list add item3;
    #list list add item4;
    > #VARIABLE {list}={{1}{item1}{2}{item2}{3}{item3}{4}{item4}}

Note that the order is preserved in a list::

    #list list add item0;
    > #VARIABLE {list}={{1}{item1}{2}{item2}{3}{item3}{4}{item4}{5}{item0}}

You can add multiple values to a list at once, note that they are whitespace delimited::

    #list list add item1 item2 item3 item4;
    > #VARIABLE {list}={{1}{item1}{2}{item2}{3}{item3}{4}{item4}}

Like (just about) everything in tintin, you can disable whitespace delimiting by using explicit braces::

    #list list add {this is a sentance}
    > #VARIABLE {list}={{1}{this is a sentance}}

NOTE: I haven't used these types of lists as much as nested variables and tables, so I'm unsure if forcing whitespace into items will break the API.
For more information about lists, see #help list.

Tables
======
Tables are key/value pairs, stored in variables. They are undocumented, but very powerful::

    #var {T1} {{K1}{V1}{K2}{V2}};
    > #VARIABLE {T1}={{K1}{V1}{K2}{V2}}

Notice that the parser writes these differently than it writes nested variables. They are different, and handled differently. They are however stored exactly the same as lists, but with string-based indexes, rather than integers.

Tables are automagically sorted. If you need to preserve the order, you'll have to use another data type. AFAIK, there's no way around this::

    #var {T1} {{K2}{V2}{K1}{V1}};
    > #VARIABLE {T1}={{K1}{V1}{K2}{V2}}

Values can be inserted into tables using either "array" or "dot" notation::

    #var T1[K1] V1;
    > #VARIABLE {T1}={{K1}{V1}}

    #var T1.K1 V1;
    > #VARIABLE {T1}={{K1}{V1}}

This is exactly the same syntax as the "nested" variables referenced above. The only difference is that we can shove multiple key/value pairs into a single variable::

    #var T1.K1 V1;
    #var T1.K2 V2;
    > #VARIABLE {T1}={{K1}{V1}{K2}{V2}}

Note: While the data in the variable looks like a "brace delimited list", it is not. Don't get confused on this, it caused me great trouble. This form is ONLY used for storing key value pairs.


When referencing a table stored in a variable, we can pull out a list of the indexes using a special "[]" operator that's only valid for this purpose in this context::

    #showme $T1[];
    > {K1}{K2}

We can store tables in any value inside a nested variable::

    #var V1.N1.T1 {{K1}{V1}{K2}{V2}};
    > #VARIABLE {V1}={{N1}{{T1}{{K1}{V1}{K2}{V2}}}}

And retrieve it::

    #showme $V1.N1.T1;
    > {K1}{V1}{K2}{V2}

To iterate over the values, we have to be careful::

    #foreach {$V1.N1.T1} {table} {
        #showme $table;
    }
    > K1
    > V1
    > K2
    > V2

Referencing it this way in the #foreach loop input causes the parser to interpret the table as a brace list (remember above?). Not the behavior we want.

To iterate over the table correctly, we need to use the special "[]" operator mentioned above::

    #foreach {$V1.N1.T1[]} {table} {
        #showme $table;
    }
    > K1
    > K2

We can store multiple tables in a nested variable::

    #var V1.N1.T1 {{T1K1}{T1V1}{T1K2}{T1V2}};
    #var V1.N1.T2 {{T2K1}{T2V1}{T2K2}{T2V2}};
    > #VARIABLE {V1}={{N1}{{T1}{{T1K1}{T1V1}{T1K2}{T1V2}}{T2}{{T2K1}{T2V1}{T2K2}{T2V2}}}}


To get a list of our tables stored in N1, we can use::
    #showme $V1.N1[];
    > {T1}{T2}

However, we have to be careful how we attempt to iterate over them. The following gives us the name of each table, a newline, and then the table content::

    #foreach {$V1.N1} {table} {
        #showme $table;
    }
    > T1
    > {T1K1}{T1V1}{T1K2}{T1V2}
    > T2
    > {T2K1}{T2V1}{T2K2}{T2V2}

To just get the name, we have to use our "[]" operator::

    #foreach {$V1.N1[]} {table} {
        #showme $table;
    }
    > T1
    > T2

As far as I know, there is no way to pull the data for each table directly out. This requires a little more effort, though we can use the following syntax to accomplish the task::

    #foreach {$V1.N1[]} {table} {
        #foreach {$V1.N1[$table][]} {key} {
            #showme $table:$key:$V1.N1[$table][$key]
        }
    }
    > T1:T1K1:T1V1
    > T1:T1K2:T1V2
    > T2:T2K1:T2V1
    > T2:T2K2:T2V2

The syntax for the foreach input fields above is extremely picky. I played around with several variations using dots and braces, and this was the one I found that works.


----------
References
----------
- `The Variable Command                         <http://tintin.sourceforge.net/manual/variable.php>`_
- `The List Command                             <http://tintin.sourceforge.net/manual/list.php>`_
- `Retrieving Keys from an associative lists    <http://tintin.sourceforge.net/board/viewtopic.php?t=1578>`_
- `Working with associative lists               <http://tintin.sourceforge.net/board/viewtopic.php?t=1598>`_
- `Stumped Nested List                          <http://tintin.sourceforge.net/board/viewtopic.php?t=1930>`_
- `Forcing Variable Substitution                <http://tintin.sourceforge.net/board/viewtopic.php?t=2170>`_
- `Help with List Issues                        <http://tintin.sourceforge.net/board/viewtopic.php?t=2218>`_

-----
Notes
-----
TODO: Add information about checking for variable and index existence (&<variable name>).
TODO: Add notes on escaping periods in variable names (#var LOGFILE data/log/$ISODATE.log;).
