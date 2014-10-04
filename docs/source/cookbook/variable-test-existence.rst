=======================================
Testing for the Existence of a Variable
=======================================
To test for the existence of a variable::

    #if {&MYVAR} { #showme $$MYVAR EXISTS         };
    #else        { #showme $$MYVAR DOES NOT EXIST };

