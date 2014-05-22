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
This means that if you have multiple "web" processes they will each try to listen on the same `PORT` environment
variable. For this to work properly you should use the socket option [SO_REUSEPORT](https://lwn.net/Articles/542629/).
If that is not available then you will need to stick with a single web process.

Rather than editing the file manually you can use the command:

    dokku scale myapp web=1 worker=6

This will generate a new `SCALE` file and then deploy the app. An app rebuild will __not__ happen.
It will just kill and restart your application.

Adding the `SCALE` file is done by copying it into the container. This adds another layer to the
container's AUFS. As there is a max number of layers you may need to occasionally run a rebuild
(try `dokku rebuild myapp`) to rebase the container.

## Thanks

This plugin is heavily based on [dokku-logging-supervisord][dokku-logging-supervisord], which is in turn based on
 [dokku-supervisord](https://github.com/statianzo/dokku-supervisord) and [dokku-persistent-storage](https://github.com/dyson/dokku-persistent-storage)

## License

This plugin is released under the MIT license. See the file [LICENSE](LICENSE).

[dokku]: https://github.com/progrium/dokku
[forego]: https://github.com/ddollar/forego
[dokku-logging-supervisord]: https://github.com/progrium/dokku