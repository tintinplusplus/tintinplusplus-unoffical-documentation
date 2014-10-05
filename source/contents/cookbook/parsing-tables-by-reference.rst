===========================
Parsing Tables by Reference
===========================
Through "passing by reference" it's possible to parse a table without knowing it's structure beforehand. First, we'll create our example data structure::

    #var V1.T1 {{T1K1}{T1V1}{T1K2}{T1V2}};
    #var V1.T2 {{T2K1}{T2V1}{T2K2}{T2V2}};

Next, we create a variable that contains the name of the variable that contains our tables::
    #var table_index V1;

Now we get a list of those tables. Note that we are using the "pass by reference" technique to operate on a dynamic variable name::

    #var tables ${$table_index}[];

Now, we need to iterate over the tables. We've successfully abstracted ourselves away from operating on a specific table, so this will parse any set of tables stored in a nested variable::

    #foreach {$tables} {table} {
        #var table {${$table_index}[$table]};
        #foreach {$table[]} {index} {
            #showme $index:$table[$index];
        }
    }
