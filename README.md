Heroku buildpack: C
===================

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpack) for C apps.
It uses [Make](http://www.gnu.org/software/make/).

Usage
-----

Example usage:

    $ ls
    configure  Makefile  myapp.c

    $ heroku create --stack cedar --buildpack http://github.com/heroku/heroku-buildpack-c.git

    $ git push heroku master
    ...
    -----> Heroku receiving push
    -----> Fetching custom buildpack
    -----> C app detected
    -----> Configuring
           Looking for somelibrary… ok
    -----> Compiling with Make
           gcc -o myapp myapp.c

The buildpack will detect your app as C if it has the file `Makefile` in the root.  It will run a `configure` script if it exists in the root of the repository. It will then run `make` to compile the app.

Hacking
-------

To use this buildpack, fork it on Github.  Push up changes to your fork, then create a test app with `--buildpack <your-github-url>` and push to it.

For example, you can run `autogen.sh` if it exists.

Open `bin/compile` in your editor, and add the following lines above the configure step:

    if [ -f autogen.sh ]; then
      echo "-----> Running autogen.sh"
      ./autogen.sh 2>&1 | indent
    fi

Commit and push the changes to your buildpack to your Github fork, then push your sample app to Heroku to test.  You should see:

    -----> Running autogen.sh
