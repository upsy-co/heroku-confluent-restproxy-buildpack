Forked from https://github.com/heroku/heroku-confluent-buildpack

# Heroku Confluent Buildpack

Installs the [Confluent](https://confluent.io) distribution on
Heroku. While you won't be able to run ZooKeeper or Kafka on it, you
could in theory run Kafka Rest on it, which is the primary purpose of
this buildpack.

The original buildpack was modified to support only the REST proxy.

## Setup

```bash
$ heroku create -b https://github.com/upsy-co/heroku-confluent-restproxy-buildpack.git
```

Now that you have an app, you can download and "install" a version
by setting up `CONFLUENT_VERSION=1.0.1`, and deploying an app that
makes use of confluent.

```bash
$ heroku config:set CONFLUENT_VERSION=1.0.1
Setting config vars and restarting vast-woodland-9430... done, v3
CONFLUENT_VERSION: 1.0.1
```

A `bin/run-confluent-restproxy` script will be created which will perform the following
steps:

* Will generate a `confluent.properties` file from the `CONFLUENT_PROPERTIES` environment variable, whose value must be JSON.

* It will setup a trap to run `bin/kafka-rest-stop` on
  SIGINT, or SIGTERM.

* It will then substitute `%PORT%` for the value of the environment
  variable `$PORT` (this enables static configuration).

* It will then launch `bin/kafka-rest-start confluent.properties`

This provides ultimate flexibility in generating your properties file
from the heroku config.

## Your app's Procfile

```bash
$ cat Procfile
web: bin/run-confluent-restproxy
```
