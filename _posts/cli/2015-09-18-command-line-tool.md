---
title: CLI Command Line Tool
modified_at: 2015-11-05 17:21:00
category: app
tags: cli interface app
---

## Installation

We provide a command line tool (CLI) able to interact with the platform.

You can find the download link and changelog here [http://cli.scalingo.com](http://cli.scalingo.com)

### Linux & MacOS X

You need to download the binary and put it in your `$PATH`. Downloading and running the `install` script on [http://cli.scalingo.com](http://cli.scalingo.com) is recommended, i.e:
{% highlight bash %}
curl -O https://cli-dl.scalingo.io/install && bash install
{% endhighlight %}

### Windows

We highly recommend to use [git-bash](https://git-for-windows.github.io/) to have an all-in-one deployment environment.

Then, you need to download Scalingo command-line tool:

* [Windows 64 bits users](http://cli-dl.scalingo.io/release/scalingo_latest_windows_amd64.zip)
* [Windows 32 bits users](http://cli-dl.scalingo.io/release/scalingo_latest_windows_386.zip)

Place the `scalingo.exe` file in the path you want, e.g "C:/Program Files".

From git-bash add this path to your `$PATH` environment variable:

{% highlight bash %}
$ export PATH=$PATH:/c/Program\ Files/
{% endhighlight %}

Now you should be able to run `scalingo.exe` from git-bash.

Note that you set `$PATH` for this specific git-bash instance and that you should add the command line above to a `.bashrc` file at the root of your `$HOME`:

{% highlight bash %}
$ echo "export PATH=$PATH:/c/Program\ Files/" >> $HOME/.bashrc
{% endhighlight %}

Now `scalingo.exe` will be available from git-bash for your next sessions.

## Tips

* You can use the environment variable `SCALINGO_APP` instead of using the `--app` flag
* If your current directory is the base directory of your project
  and that your git repository has a remote named 'scalingo', you
  don't need to specify `--app <name>` it will be detected automatically.
  * If you want to specify the remote name, you can do it by using `--remote` or `-r` flag followed
    by the name

## Command Completion

### Bash

* Make sure bash completion is installed. If you use a current Linux in a non-minimal installation, bash completion should be available. On a Mac, install with `brew install bash-completion`

* Get bash completion script in the directory:
  * Linux users `/etc/bash_completion.d/`:
  {% highlight bash %}
    sudo curl "https://raw.githubusercontent.com/Scalingo/cli/master/cmd/autocomplete/scripts/scalingo_complete.bash" -o /etc/bash_completion.d/scalingo_complete.sh
  {% endhighlight %}
  * Mac users `/usr/local/etc/bash_completion.d/`:
  {% highlight bash %}
    sudo curl "https://raw.githubusercontent.com/Scalingo/cli/master/cmd/autocomplete/scripts/scalingo_complete.bash" -o /usr/local/etc/bash_completion.d/scalingo_complete.sh
  {% endhighlight %}
* Reload your shell in order to make the completion available:

  {% highlight bash %}
  exec bash -l
  {% endhighlight %}

### Zsh

* Create a directory `~/.zsh/completion/` :

  {% highlight bash %}
  mkdir -p ~/.zsh/completion
  {% endhighlight %}

* Get zsh completion script in the directory `~/.zsh/completion/` :

  {% highlight bash %}
  curl "https://raw.githubusercontent.com/Scalingo/cli/master/cmd/autocomplete/scripts/scalingo_complete.zsh" > ~/.zsh/completion/scalingo_complete.zsh
  {% endhighlight %}

* Make sure the completion script will be loaded, by adding to the following line to your `~/.zshrc` :

  {% highlight bash %}
  source ~/.zsh/completion/scalingo_complete.zsh
  {% endhighlight %}

* Reload your shell:

  {% highlight bash %}
  exec zsh -l
  {% endhighlight %}

## Features

### Create new apps
`scalingo create`
{% highlight bash %}
scalingo create my-new-app

# Create a new app with a custom GIT remote
scalingo create my-new-app --remote staging
scalingo create my-new-app --remote production
scalingo create my-new-app --remote custom
{% endhighlight %}

### Setup your account SSH keys
`scalingo keys|keys-add|keys-remove`
{% highlight bash %}
scalingo keys
scalingo keys-add "Laptop SSH key" $HOME/.ssh/id_rsa.pub
scalingo keys-remove "Laptop SSH key"
{% endhighlight %}

### Configure their environment
`scalingo env|env-set|env-unset`
{% highlight bash %}
scalingo -a myapp env
scalingo -a myapp env-set NODE_ENV=production
scalingo -a myapp env-unset NODE_ENV
{% endhighlight %}

### Configure custom domain names
`scalingo domains|domains-add|domains-ssl|domains-remove`
{% highlight bash %}
scalingo -a myapp domains
scalingo -a myapp domains-add example.com
scalingo -a myapp domains-ssl example.com --cert file.crt --key file.key
scalingo -a myapp domains-remove example.com
{% endhighlight %}

### Manage collaborators of the application
`scalingo collaborators|collaborators-add|collaborators-remove`
{% highlight bash %}
scalingo -a myapp collaborators
scalingo -a myapp collaborators-add user@example.com
scalingo -a myapp collaborators-remove user@example.com
{% endhighlight %}

### List existing addons and plans
`scalingo addons-list|addons-plans`
{% highlight bash %}
scalingo addons-list
scalingo addons-plans scalingo-mongodb
{% endhighlight %}

### Manage addons of your applications
`scalingo addons|addons-add|addons-remove|addons-upgrade`
{% highlight bash %}
scalingo -a myapp addons
scalingo -a myapp addons-add scalingo-mongodb 1g
scalingo -a myapp addons-remove myapp_12345
scalingo -a myapp addons-upgrade myapp_12345 2g
{% endhighlight %}

### Read and watch the logs
`scalingo logs`
{% highlight bash %}
# Display the last 1000 lines of log
scalingo -a myapp logs -n 1000

# Follow the logs of your application in real-time
scalingo -a myapp logs -f

# Filter your logs by container type
scalingo -a myapp logs -F "web"
scalingo -a myapp logs --filter "worker"
scalingo -a myapp logs -F "web|worker"
{% endhighlight %}

### Run custom job
`scalingo run`
{% highlight bash %}
scalingo -a myapp run bundle exec rails console

# Define custom environment variables into the one-off container
scalingo -a myapp run --env CUSTOM_ENV=value --env ENV_EXAMPLE=custom

# Upload a file to the one-off container (target is /tmp/uploads)
scalingo -a myapp run --file ./dump.sql
{% endhighlight %}

### Get metrics of your application
`scalingo stats`

{% highlight bash %}
# Get the current stats of the containers of your app
scalingo -a myapp stats

# Display and update every 10 seconds the stats of the containers of your app
scalingo -a myapp stats --stream
{% endhighlight %}

### Access your database
`scalingo mongo-console|redis-console|mysql-console|pgsql-console|db-tunnel`
{% highlight bash %}
scalingo -a myapp mongo-console
scalingo -a myapp redis-console
scalingo -a myapp mysql-console
scalingo -a myapp pgsql-console

# Build an encrypted tunnel to access your database
scalingo -a myapp db-tunnel MONGO_URL
{% endhighlight %}

### Manage containers, scale
`scalingo ps|restart|scale`
{% highlight bash %}scalingo -a myapp ps
scalingo -a myapp restart web:1
scalingo -a myapp scale web:2
scalingo -a myapp scale worker:2:L
scalingo -a myapp scale clock:0
{% endhighlight %}

##Configuration

### Application Detection

We try to detect automatically the name of your application according to:

* `SCALINGO_APP` environment variable
* `-a|--app`     flag of the command line to specify an application name
* `-r|--remote`  flag of the command line to specify a remote GIT
* scalingo remote of your GIT repository
