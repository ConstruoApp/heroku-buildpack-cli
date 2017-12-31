Heroku buildpack: Heroku CLI
=========================

Fork Info
---------

This is a modified buildpack that aside of installing heroku CLI, it installs heroku fork plugin and copies `HEROKU_CLI_EMAIL` and `HEROKU_CLI_TOKEN` env vars to `$HOME/.netrc` file.


This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) that
allows one to run the Heroku CLI in a dyno alongside application code.

This is not a replacement for the [Heroku API](https://devcenter.heroku.com/articles/platform-api-reference#overview) or various clients like [v3 Ruby](https://github.com/heroku/platform-api), [node](https://www.npmjs.org/package/heroku-client) or [python](https://github.com/heroku/heroku.py). Some private APIs like `pgbackups` do require the buildpack, so this exists.

Usage
-----

Example usage:

    $ heroku config:set HEROKU_API_KEY=`heroku auth:token`

    $ heroku buildpacks:add https://github.com/heroku/heroku-buildpack-cli

    $ git push heroku master
    ...

    remote: -----> heroku-cli app detected
    remote: === Fetching and vendoring Heroku CLI into slug
    remote: === Installing profile.d script
    remote: === Heroku CLI installation done
    ...

    $ heroku run 'heroku auth:token'
    Running `heroku auth:token` attached to terminal... up, run.3706
    abcdef0123456789abcdef0123456789abcdef01

    $ heroku run 'heroku pgbackups:capture SILVER -a myapp'
    Running `heroku pgbackups:capture SILVER -a myapp` attached to terminal... up, run.9532

    HEROKU_POSTGRESQL_SILVER_URL  ----backup--->  b003

    Capturing... done
    Storing... done

Notes
-----

Instead of setting HEROKU_API_KEY directly on the app as shown above, a short lived token may be passed in at run time:

```
heroku run "HEROKU_API_KEY=`heroku authorizations:create --expires-in 600 --short` heroku pgbackups:capture SILVER -a myapp" -a myapp
```
