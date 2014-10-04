====================
Passing by Reference
====================
Even though we don't have address or dereferencing operators in tintin, we can still emulate some very basic referential operations::

    #var myvar     {mydata};
    #var mypointer {myvar};

    #showme $mypointer:${$mypointer};
    > myvar:mydata

    #alias myalias {
        #showme value of "dereferenced" alias argument: ${%1};
    }
    myalias myvar;
    > value of "dereferenced" alias argument: mydata

    #function myfunction {
        #return $%1
    };

    #showme valued of "dereferenced" function argument: @myfunction{myvar};
    > value of "dereferenced" function argument: mydata
