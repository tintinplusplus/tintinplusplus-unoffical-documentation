========
Sessions
========


-------------------
The Startup Session
-------------------
The startup session is created at default. It's named gts (per #help session) and can be referenced with #gts, just as you would reference a character's session. Any code from command files loaded on startup is executed in this session.

There are probably other interesting things about #gts, but I don't know them.


-------------------
Session Inheritance
-------------------
When creating a new session - the entire config from the startup session is copied into the new session. This is important if you play multiple sessions at the same time - you don't want to copy character A's data into a session for character B.

It is also important to note that the copy of the data does not reference the original session in any way - it's a brand new copy. Once the new session has been established, changing the data in either session will not affect the copy of the data in the other.

If you use a "session API" (ttlib includes one), then this can be confusing at first. When I load the program, all my session management code is copied into the GTS session. When I create a new session, that code gets copied into the characters session. I can then use the session API from the characters session, however it will contain different variables than those in the #GTS session.


----------
References
----------

- `The All Command      <http://tintin.sourceforge.net/manual/all.php>`_
- `The Session Command  <http://tintin.sourceforge.net/manual/session.php>`_
