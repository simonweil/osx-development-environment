===============================
Configuring OSX for Development
===============================

This doc assumes you are doing a clean install of `Homebrew <http://mxcl.github.io/homebrew/>`_ on a clean install of OSX 10.9.x (Mavericks) with Xcode 5.x. If you've got MacPorts installed or have otherwise already buggered up your system, you'll either need to live with your decision or do a clean install and start from scratch.

This guide is geared primarily towards Django and Rails development. If you use another web framework, you'll either need to live with your decision or start from scratch with Django or Rails ;)

Without further ado...

Install Xcode
-------------

You can install Xcode directly from the App Store but this is a large download. If you need the full Xcode anyway, go for it. Once this is complete, open Xcode, agree to the TOS and then select ``Xcode -> Preferences -> Downloads -> Components`` and make sure the Command Line Tools package is installed.

If you don't need the full Xcode, you can simply type ``git`` at the command line and it will pop up a download dialog to fetch the developer tools.

According to the internets, you can also manually download just the Command Line Tools. You can download these tools `here <https://developer.apple.com/downloads/index.action>`_. Agree to the TOS by typing "sudo xcodebuild -license".

Homebrew
--------

Install::


    ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go/install)"
    brew doctor

Bash
----

Install::

    brew install bash

Update the default shell::

    sudo vi /etc/shells

Add the path to the shell you want to use if not already present, then set it::

    chsh -s /usr/local/bin/bash

Bash completion
---------------

Install::

    brew install bash-completion

Output::

    ==> Caveats
    Add the following lines to your ~/.bash_profile:
      if [ -f $(brew --prefix)/etc/bash_completion ]; then
        . $(brew --prefix)/etc/bash_completion
      fi

    Homebrew's own bash completion script has been installed to
      /usr/local/etc/bash_completion.d

    Bash completion has been installed to:
      /usr/local/etc/bash_completion.d

Add to your ``~/.bash_profile``::

    if [ -f $(brew --prefix)/etc/bash_completion ]; then
        . $(brew --prefix)/etc/bash_completion
    fi

Now activate it::

    source ~/.bash_profile

You should also add (to enable colors in the shell)::

    export CLICOLOR=1

wget
----

Handy to have in general (especially if you're copy/paste-ing someone else's commands...like below in this very document)::

    brew install wget

rsync
-----

OSX's default ``rsync`` is old and dumb. Replace it::

    brew tap homebrew/dupes
    brew install rsync
    brew untap homebrew/dupes

Programming Languages & Web Frameworks
======================================

Python
------

Homebrew installs pip and distribute by default when installing Python::

    brew install python --framework

The ``--framework`` option allegedly installs Python in a way so that software depending on the OS X Python will still install correctly. I don't know, I read it on a blog somewhere.

virtualenv::

    pip install virtualenv

iPython::

    pip install ipython

iPython notebook (install zeromq first, see directions below)::

    pip install pyzmq tornado Jinja2

You'll also need numpy to build gdal::

    pip install numpy

Python 3::

    brew install python3

Output::

    ==> Caveats
    Homebrew's Python3 framework
      /usr/local/Cellar/python3/3.3.1/Frameworks/Python.framework

    Distribute and Pip have been installed. To update them
      pip3 install --upgrade distribute
      pip3 install --upgrade pip

    To symlink "Idle 3" and the "Python Launcher 3" to ~/Applications
      `brew linkapps`

    You can install Python packages with
      `pip3 install <your_favorite_package>`

    They will install into the site-package directory
      /usr/local/lib/python3.3/site-packages
    Executable python scripts will be put in:
      /usr/local/share/python3
    so you may want to put "/usr/local/share/python3" in your PATH, too.

    See: https://github.com/mxcl/homebrew/wiki/Homebrew-and-Python

Django bash completion::

    mkdir ~/.django

    wget --no-check-certificate https://raw.github.com/django/django/c09f6ff0a58d016eeb7536f1df1fa956f94f671c/extras/django_bash_completion -O ~/.django/django_bash_completion

Add to ``~/.bash_profile``::

    if [ -f ~/.django/django_bash_completion ]; then
        . ~/.django/django_bash_completion
    fi

Ruby & Rails
------------

This installs both Ruby and Rails in one go::

    \curl -L https://get.rvm.io | bash -s stable --rails --autolibs=enabled

Sass::

    gem install sass

Less::

    brew install node
    npm install -g less

Version Control
===============

Mercurial::

    brew install mercurial

Git::

    brew install git

Output::

    ==> Caveats
    The OS X keychain credential helper has been installed to:
      /usr/local/bin/git-credential-osxkeychain

    The 'contrib' directory has been installed to:
      /usr/local/share/git-core/contrib

    Bash completion has been installed to:
      /usr/local/etc/bash_completion.d

    zsh completion has been installed to:
      /usr/local/share/zsh/site-functions

SVN::

    brew install subversion

Data Stores
===========

PostgreSQL
----------

If you need to develop with Django < 1.5, see separate guide old-postgis-postgresql.rst.
Otherwise::

    brew install postgres

Output::

    ==> Caveats
    initdb /usr/local/var/postgres -E utf8    # create a database
    postgres -D /usr/local/var/postgres       # serve that database
    PGDATA=/usr/local/var/postgres postgres   # …alternatively

    If builds of PostgreSQL 9 are failing and you have version 8.x installed,
    you may need to remove the previous version first. See:
      https://github.com/mxcl/homebrew/issues/issue/2510

    To migrate existing data from a previous major version (pre-9.3) of PostgreSQL, see:
      http://www.postgresql.org/docs/9.3/static/upgrading.html

    When installing the postgres gem, including ARCHFLAGS is recommended:
      ARCHFLAGS="-arch x86_64" gem install pg

    To install gems without sudo, see the Homebrew wiki.

    To have launchd start postgresql at login:
        ln -sfv /usr/local/opt/postgresql/*.plist ~/Library/LaunchAgents
    Then to load postgresql now:
        launchctl load ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist
    Or, if you don't want/need launchctl, you can just run:
        postgres -D /usr/local/var/postgres

PostGIS::

    brew install postgis

Output::

    ==> Caveats
    To create a spatially-enabled database, see the documentation:
      http://postgis.net/docs/manual-2.1/postgis_installation.html#create_new_db_extensions
    If you are currently using PostGIS 2.0+, you can go the soft upgrade path:
      ALTER EXTENSION postgis UPDATE TO "2.1.0";
    Users of 1.5 and below will need to go the hard-upgrade path, see here:
      http://postgis.net/docs/manual-2.1/postgis_installation.html#upgrading

    PostGIS SQL scripts installed to:
      /usr/local/share/postgis
    PostGIS plugin libraries installed to:
      /usr/local/opt/postgresql/lib
    PostGIS extension modules installed to:
      /usr/local/opt/postgresql/share/postgresql/extension

To create a database instance::

    initdb /usr/local/var/postgres -E utf8

You can now start the database server using::

    pg_ctl -D /usr/local/var/postgres -l /usr/local/var/postgres/server.log start

Or to set it to start automatically, see the output above after installing postgresql.

Create the spatially enabled template::

    createdb template_postgis
    psql -f /usr/local/share/postgis/postgis.sql template_postgis
    psql -f /usr/local/share/postgis/spatial_ref_sys.sql template_postgis

Create users::

    createuser -s web

To create a spatially enabled database::

    createdb -T template_postgis mydbname

MySQL
-----

PostgreSQL is always preferred but sometimes you don't have a choice::

    brew install mysql

Output::

    ==> Caveats
    A "/etc/my.cnf" from another install may interfere with a Homebrew-built
    server starting up correctly.

    To connect:
      mysql -uroot

    To have launchd start mysql at login:
      ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents
    Then to load mysql now:
      launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
    Or, if you don't want/need launchctl, you can just run:
      mysql.server start

Create a database and set permissions for development::

    mysql -uroot

    CREATE DATABASE project CHARACTER SET UTF8;
    GRANT ALL PRIVILEGES ON project.* TO 'web'@'localhost' WITH GRANT OPTION;

MariaDB
-------
This is a drop-in replacement for MySQL by the original authors (forked after Oracle bought Sun)::

    brew install mariadb

Output::

    ==> Caveats
    Set up databases with:
        unset TMPDIR
        mysql_install_db --user=`whoami` --basedir="$(brew --prefix mariadb)" --datadir=/usr/local/var/mysql --tmpdir=/tmp

    To have launchd start mariadb at login:
        ln -sfv /usr/local/opt/mariadb/*.plist ~/Library/LaunchAgents
    Then to load mariadb now:
        launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mariadb.plist
    Or, if you don't want/need launchctl, you can just run:
        mysql.server start

MongoDB
-------

Install::

    brew install mongodb

Output::

    ==> Caveats
    To have launchd start mongodb at login:
        ln -sfv /usr/local/opt/mongodb/*.plist ~/Library/LaunchAgents
    Then to load mongodb now:
        launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mongodb.plist
    Or, if you don't want/need launchctl, you can just run:
        mongod


You have to create a data directory. By default it expects the data to be stored in ``/data/db``
Otherwise, create a directory and pass the path when running the server::

    mongod --dbpath=/Users/sallysue/Projects/data/mongodb

Redis
-----

Install::

    brew install redis

Output::

    ==> Caveats
    To have launchd start redis at login:
        ln -sfv /usr/local/opt/redis/*.plist ~/Library/LaunchAgents
    Then to load redis now:
        launchctl load ~/Library/LaunchAgents/homebrew.mxcl.redis.plist
    Or, if you don't want/need launchctl, you can just run:
        redis-server /usr/local/etc/redis.conf

memcached
---------

Install::

    brew install memcached

Output::

    To have launchd start memcached at login:
        ln -sfv /usr/local/opt/memcached/*.plist ~/Library/LaunchAgents
    Then to load memcached now:
        launchctl load ~/Library/LaunchAgents/homebrew.mxcl.memcached.plist
    Or, if you don't want/need launchctl, you can just run:
        /usr/local/opt/memcached/bin/memcached


Task Queues
===========

Rabbit MQ
---------

Install::

    brew install rabbitmq

Output::

    ==> Caveats
    Management Plugin enabled by default at http://localhost:15672

    Bash completion has been installed to:
      /usr/local/etc/bash_completion.d

    To have launchd start rabbitmq at login:
        ln -sfv /usr/local/opt/rabbitmq/*.plist ~/Library/LaunchAgents
    Then to load rabbitmq now:
        launchctl load ~/Library/LaunchAgents/homebrew.mxcl.rabbitmq.plist
    Or, if you don't want/need launchctl, you can just run:
        rabbitmq-server

ZeroMQ
------

Install::

    brew install zeromq

Output::

    ==> Caveats
    To install the zmq gem on 10.6 with the system Ruby on a 64-bit machine,
    you may need to do:

    ARCHFLAGS="-arch x86_64" gem install zmq -- --with-zmq-dir=/usr/local/opt/zeromq

Celery
------

Homepage => https://github.com/celery/django-celery/

Install::

    pip install -U Celery

To run::

    ./manage.py celeryd

To configure your Django project to work with Celery/RabbitMQ, see http://docs.celeryproject.org/en/latest/getting-started/brokers/rabbitmq.html

Web Servers
===========

nginx
-----

Install::

    gem install passenger
    brew install nginx --with-passenger --with-debug --with-spdy --with-gunzip

Output::

    ==> Caveats
    Docroot is: /usr/local/var/www

    The default port has been set to 8080 so that nginx can run without sudo.

    If you want to host pages on your local machine to the wider network you
    can change the port to 80 in: /usr/local/etc/nginx/nginx.conf

    You will then need to run nginx as root: `sudo nginx`.

    To have launchd start nginx at login:
        ln -sfv /usr/local/opt/nginx/*.plist ~/Library/LaunchAgents
    Then to load nginx now:
        launchctl load ~/Library/LaunchAgents/homebrew.mxcl.nginx.plist

Apache
------

Homebrew relies on the supplied OSX version of Apache, it just adds modules to it from a tap.
See https://github.com/Homebrew/homebrew-apache for more information.


Miscellaneous tools
===================

https://github.com/coolwanglu/pdf2htmlEX
``brew install pdf2htmlex``

Image processing utils
----------------------

``brew install optipng jpegoptim pngcrush ImageMagick``

Homebrew maintenance
--------------------

To update your installed brews::

    brew update
    brew outdated
    brew upgrade

Get a checkup from the doctor and follow the doctor's instructions::

    brew doctor

