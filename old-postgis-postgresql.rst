There is a bug in versions of Django > 1.5 which causes incompatibilities using PostGIS 2.x so you will need to install compatible versions of PostgreSQL and PostGIS.
These instructions were taken from https://gist.github.com/fcurella/3188632::

    brew install postgresql9

Output::

    ==> Caveats
    If builds of PostgreSQL 9 are failing and you have version 8.x installed,
    you may need to remove the previous version first. See:
      https://github.com/mxcl/homebrew/issues/issue/2510

    To build plpython against a specific Python, set PYTHON prior to brewing:
      PYTHON=/usr/local/bin/python  brew install postgresql
    See:
      http://www.postgresql.org/docs/9.0/static/install-procedure.html


    If this is your first install, create a database with:
      initdb /usr/local/var/postgres9

    If this is your first install, automatically load on login with:
      mkdir -p ~/Library/LaunchAgents
      cp /usr/local/Cellar/postgresql9/9.0.13/org.postgresql.postgres.plist ~/Library/LaunchAgents/
      launchctl load -w ~/Library/LaunchAgents/org.postgresql.postgres.plist

    If this is an upgrade and you already have the org.postgresql.postgres.plist loaded:
      launchctl unload -w ~/Library/LaunchAgents/org.postgresql.postgres.plist
      cp /usr/local/Cellar/postgresql9/9.0.13/org.postgresql.postgres.plist ~/Library/LaunchAgents/
      launchctl load -w ~/Library/LaunchAgents/org.postgresql.postgres.plist

    Or start manually with:
      pg_ctl -D /usr/local/var/postgres9 -l /usr/local/var/postgres9/server.log start

    And stop with:
      pg_ctl -D /usr/local/var/postgres9 stop -s -m fast


    Some machines may require provisioning of shared memory:
      http://www.postgresql.org/docs/current/static/kernel-resources.html#SYSVIPC

    If you want to install the postgres gem, including ARCHFLAGS is recommended:
        env ARCHFLAGS="-arch x86_64" gem install pg

    To install gems without sudo, see the Homebrew wiki.

    To have launchd start postgresql9 at login:
        ln -sfv /usr/local/opt/postgresql9/*.plist ~/Library/LaunchAgents
    Then to load postgresql9 now:
        launchctl load ~/Library/LaunchAgents/homebrew.mxcl.postgresql9.plist

PostGIS::

    brew install postgis15

Output::

    ==> Caveats
    To create a spatially-enabled database, see the documentation:
      http://postgis.refractions.net/documentation/manual-1.5/ch02.html#id2630392
    and to upgrade your existing spatial databases, see here:
      http://postgis.refractions.net/documentation/manual-1.5/ch02.html#upgrading

    PostGIS SQL scripts installed to:
      /usr/local/share/postgis
    PostGIS plugin libraries installed to:
      /usr/local/Cellar/postgresql9/9.0.13/lib

Untap the keg::

    brew untap homebrew/versions

To create a database instance::

    initdb /usr/local/var/postgres9

You can now start the database server using::

    pg_ctl -D /usr/local/var/postgres9 -l /usr/local/var/postgres9/server.log start

Or to set it to start automatically, see the output above after installing postgresql.
