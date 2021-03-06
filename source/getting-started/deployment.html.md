---
guide: Getting Started
section: Deployment
menu: getting-started
---

While you can easily run Lita from your own computer, for a more permanent installation you'll want to deploy it to a remote server. Where you deploy Lita is up to you, but there are a few common patterns for deployment.

### Heroku {#heroku}

There are a few things worth mentioning when deploying Lita to Heroku:

1.  Your Procfile should contain one process:

    ~~~
    web: bundle exec lita
    ~~~

1.  To use the Redis To Go add-on and the HTTP port set by Heroku, configure Lita like this:

    ~~~ ruby
    Lita.configure do |config|
      config.redis[:url] = ENV["REDISTOGO_URL"]
      config.http.port = ENV["PORT"]
    end
    ~~~

1.  Consider using a service like [Uptime Robot](http://www.uptimerobot.com/) to monitor your Lita instance and keep it from [sleeping](https://blog.heroku.com/archives/2013/6/20/app_sleeping_on_heroku) when running on a free dyno. `/lita/info` is a reliable path to hit from the web to keep it running.

### Daemonization {#daemonization}

<div class="alert alert-danger">
  <strong>Warning:</strong>
  This feature is deprecated and will be removed in Lita 5.0. Use your operating system's process manager to daemonize Lita instead.
</div>

Lita has built-in support for daemonization on Unix systems. When run as a daemon, Lita will redirect standard output and standard error to a log file, and write the process ID to a PID file. To start Lita as a daemon, run the command: <kbd>lita -d</kbd>.

There are additional command line flags for specifying the path of the log and PID files, which override the defaults. If an existing Lita process is running when <kbd>lita -d</kbd> is invoked, Lita will abort and leave the original process running, unless the <kbd>-k</kbd> flag is specified, in which case it will kill the existing process. Run the command <kbd>lita help</kbd> for information about all the possible command line flags.
