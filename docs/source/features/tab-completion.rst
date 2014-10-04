==============
Tab Completion
==============
Tab completion does not work for commands with whitespace, the following script demonstrates this behavior::

    #nop Our "nested" tab completions.;
    #tab module list;
    #tab module load;
    #tab module kill;
    #tab module reload;

    #nop Our primary API alias. Calls the correct aliases for us based on the ;
    #nop initial arguments;
    #alias module {

        #showme ALIAS: module;

        #if {"%1" == "list"} {moduleList};
        #elseif {"%1" == "load"} {moduleLoad %2};
        #elseif {"%1" == "kill"} {moduleKill %2};
        #elseif {"%1" == "reload"} {mdouleReload %2};
        #else {
            #showme ERROR: Usage: module list | load <module> | kill <module> | reload <module>;
        };
    }

    #nop Some stub aliases.;
    #alias moduleList   {#showme moduleList};
    #alias moduleLoad   {#showme moduleLoad %1};
    #alias moduleKill   {#showme moduleKill %1};
    #alias moduleReload {#showme moduleReload %1};

This results in the following tab completions::

    mod<tab>
    module kill<tab>
    module mod<tab>
    module module kill<tab>
    module module mod
    ... etc
