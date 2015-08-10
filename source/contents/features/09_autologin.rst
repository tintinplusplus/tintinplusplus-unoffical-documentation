Autologin
========

A typical feature of every config: game autologin. Usually it looks like this:

::

  #event {SESSION_CONNECTED} {
    #showme {CONNECTED: %0};
    #regex {%0} {^sessionname$} {CONNECT name password} {0};
  }
  
* ``sessionname`` is the name of your session you will use in the ``#session`` command
* ``CONNECT name password`` is the autologin command. Some MUDs understand only username and password, so you have to write: ``username\npassword`` instead.

This snippet activates on session connect. It checks the session name and sends the autologin command.
