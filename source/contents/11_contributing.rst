============
Contributing
============
I've invited all the TinTin++ user's on github that I know about to the organization, which should provide all of you (them) commit privileges. If I missed you, just sent a request to join the group `here <https://github.com/orgs/tintinplusplus/people>`_.


-------------
Documentation
-------------
The `TinTin Unofficial Documentation <http://http://tintin-unoffical-documentation.readthedocs.org/>`_ is built using sphinx, which uses python. To build a local version of the documentation that you can edit and publish, you'll need to install the necessary python dependencies first. There are lots of ways to do this, personally I like to use virtualenv and virtualenvwrapper to setup an isolated python environment::

    pip install virtualenv virtualenvwrapper

Then add the following to your shell initialization (.bashrc, .zshrc, etc) file::

    if [[ -f '/usr/local/bin/virtualenvwrapper.sh' ]]; then
        export WORKON_HOME=$HOME/.virtualenvs
        export PROJECT_HOME=$HOME/Documents/Projects
        source /usr/local/bin/virtualenvwrapper.sh
    fi

Then create the virtualenv and install the python requirements:

    mkvirtualenv mudfiles
    pip install -r requirements.txt

And finally build your local copy of the documentation (from docs/)::

    make clean && make html && chrome build/html/index.html

Once you've make your modifications, if you've got commit privileges, just push your updates.  Otherwise, submit a `pull request <https://help.github.com/articles/using-pull-requests/>`_.

---------
RTD Theme
---------
The docs use the `sphinx rtd theme <https://github.com/snide/sphinx_rtd_theme>`_. I've modified the sphinx config.py to use this theme when building locally as well. To build locally, you'll need to install the them via pip (this is done automatically when you install the requirements).

I'm not sure yet, but I may need to fork the theme to setup the additional features on the :doc:`10_development` page.

References:

- `Read The Docs Theme Documentation <http://read-the-docs.readthedocs.org/en/latest/index.html>`_
