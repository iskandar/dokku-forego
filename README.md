# dokku-forego

dokku-forego is a plugin for [dokku][dokku] that injects
[forego][forego] to manage Procfile processes.

Only works with dokku 0.3.19+.

## Installation

```
# Install the plugin:
git clone https://github.com/iskandar/dokku-forego.git /var/lib/dokku/plugins/forego
```

## What it does

Normally, dokku only runs the `web` process within Procfile. The
dokku-forego plugin will run all process types (web, worker, etc.) in your Procfile.


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
