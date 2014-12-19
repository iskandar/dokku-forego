# dokku-forego

dokku-forego is a plugin for [dokku][dokku] that injects
[forego][forego] to manage Procfile processes.

## Installation

```
# Install the plugin:
git clone https://github.com/iskandar/dokku-forego.git /var/lib/dokku/plugins/forego
```

## What it does

Normally, dokku only runs the `web` process within Procfile. The
dokku-forego plugin will run all process types (web, worker, etc.) in your Procfile.


## Scaling

__Note:__ The Scaling feature is 100% compatible with [dokku-logging-supervisord][dokku-logging-supervisord] SCALE
files, so you can switch between the two plugins if you wish.

This plugin supports running multiple of the same process type. At start it checks for a file in the apps home
directory named `SCALE`. The file should be a series of lines of the form `name=<num>` where `name` is the process
name and `<num>` is the number of processes of that type to start.

Example:

    web=1
    worker=5
    clock=1

If the file does not exist then a single process of each type will be created for each process type in
Procfile. Additional lines in the file ignored.

__Note:__ All the processes will run in same Docker container. They do *not* run in separate containers.

Rather than editing the file manually you can use the command:

    dokku scale myapp web=1 worker=6

This will generate a new `SCALE` file and then deploy the app. An app rebuild will __not__ happen.
It will just kill and restart your application.

Adding the `SCALE` file is done by copying it into the container. This adds another layer to the
container's AUFS. As there is a max number of layers you may need to occasionally run a rebuild
(try `dokku rebuild myapp`) to rebase the container.

## Ports

Each item in the `procfile` will be run with a `PORT` environment variable starting with the default value of `5000` and incremented by 100 for each item.

For example, given the following `Procfile`:

    web1: node server.js
    web2: python serve.py
    web3: bundle exec thin start

Ports will be specified like this:

* `web1` will start with `PORT=5000` 
* `web2` will start with `PORT=5100` 
* `web3` will start with `PORT=5200` 

This behaviour is baked in to `forego` - so be aware that the ordering of `Procfile` items is significant!

## Similar plugins

You can also use these plugins to manage Procfile processes:

* [dokku-logging-supervisord][dokku-logging-supervisord]
* [dokku-shoreman](https://github.com/statianzo/dokku-shoreman)
* [dokku-supervisord](https://github.com/statianzo/dokku-supervisord)

## Thanks

This plugin is heavily based on [dokku-logging-supervisord][dokku-logging-supervisord], which is in turn based on
 [dokku-supervisord](https://github.com/statianzo/dokku-supervisord) and [dokku-persistent-storage](https://github.com/dyson/dokku-persistent-storage)

## License

This plugin is released under the MIT license. See the file [LICENSE](LICENSE).

[dokku]: https://github.com/progrium/dokku
[forego]: https://github.com/ddollar/forego
[dokku-logging-supervisord]: https://github.com/sehrope/dokku-logging-supervisord
