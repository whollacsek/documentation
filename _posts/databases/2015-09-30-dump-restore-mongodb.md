---
title: How to dump and restore my MongoDB database on Scalingo
modified_at: 2016-01-08 14:22:00
category: databases
tags: databases mongodb tunnel
index: 3
permalink: /databases/mongodb/dump-restore/
---

{% include info_command_line_tool.md %}

There's two ways to dump a distant database and restore the data in your Scalingo database. The first one involves dumping the data on your local workstation and the second one involves doing the same operations from within a Scalingo one-off container (see [application tasks]({% post_url app/2014-10-02-tasks %})).

## Dump and Restore from your local workstation

You can dump and restore your database from your local workstation using [Scalingo CLI]({% post_url cli/2015-09-18-command-line-tool %}) to [create a tunnel]({% post_url /databases/2014-11-24-tunnel %}) to your database:

A MongoDB URL is usually formatted like: <br>
`mongodb://<username>:<password>@<host>:<port>/<db>`

To get the URL of your database, go to the 'Environment' part of your dashboard or
run the following command:

{% highlight bash %}
$ scalingo -a myapp env | grep MONGO
{% endhighlight %}

If your remote database URL is:

{% highlight bash %}
mongodb://user:pass@my-db.mongo.dbs.com:30000/my-db
{% endhighlight %}

### Setup the tunnel

{% highlight bash %}
$ scalingo -a myapp db-tunnel SCALINGO_MONGO_URL
scalingo -a myapp db-tunnel SCALINGO_MONGO_URL
Building tunnel to my-db.mongo.dbs.scalingo.eu:30000
You can access your database on '127.0.0.1:10000'
{% endhighlight %}

### Dump

The command definition is:
{% highlight bash %}
$ mongodump -u <username> -p <password> -h <host>:<port> -d <db> 
{% endhighlight %}

Applied to our example:

{% highlight bash %}
$ mongodump -u my-db -p pass -h 127.0.0.1:10000 -d my-db
{% endhighlight %}

The command will create a dump directory in your current working directory.

As you can see we're using the host and port provided by the tunnel, not those of the URL

### Restore

The command definition is:
{% highlight bash %}
$ mongorestore -u <username> -p <password> -h <host>:<port> -d <db> <dump directory>
{% endhighlight %}

With our example:
{% highlight bash %}
$ mongorestore -u my-db -p pass -h 127.0.0.1:10000 -d my-db dump
{% endhighlight %}

## Dump and Restore from Scalingo one-off container

You can dump and restore your database remotely using
[the command-line-tool]({% post_url cli/2015-09-18-command-line-tool %})
and a one-off container (see [application tasks]({% post_url app/2014-10-02-tasks %})).
The advantage of this method is the network.
From your workstation you don’t always have a good bandwidth. From our infrastructure,
data transfers will be way faster.

### Dump

{% highlight bash %}
$ scalingo -a myapp run bash

[00:00] Scalingo ~ $ mongodump -u user -p pass -h my-db.mongo.dbs.scalingo.com:30000 my-db

# Do something with the dump, i.e send it with FTP or to an external server
{% endhighlight %}

### Restore


{% highlight bash %}
$ scalingo -a myapp run bash

# Get a dump from a remote place, with 'curl' or 'ftp'

[00:00] Scalingo ~ $ mongorestore -u my-db -p pass -h my-db.mongo.dbs.scalingo.com:30000 -d my-db dump
{% endhighlight %}

After exiting the one-off container, the dump will be lost, you've to do something with it in the container.
