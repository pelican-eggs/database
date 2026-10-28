# Dragonfly Dev

## From their [GitHub](https://github.com/dragonflydb/dragonfly)

Dragonfly is an in-memory data store built for modern application workloads. It is fully compatible with Redis and Memcached APIs and requires no code changes to adopt.

## Experimental

This egg installs the daily development build of Dragonfly from the [dragonfly-dev](https://github.com/dragonflydb/dragonfly/pkgs/container/dragonfly-dev) package, which is built every night from the `main` branch. It is not a release: it can have bugs, and a snapshot saved by a development build may not load in an older Dragonfly release. Use the Dragonfly egg for anything you depend on, and keep backups.

It is useful to try fixes and features before they are released. For example, Dragonfly 2.0.2 can't import data saved by Redis 8.6 and newer, and the development build can.

## Development Build

The `Development Build` variable selects the build:

* `ubuntu` (default) installs the newest daily build.
* `ubuntu-<commit>`, for example `ubuntu-f7fe6c4`, installs the build of one commit of the `main` branch. The available tags are listed on the [package page](https://github.com/dragonflydb/dragonfly/pkgs/container/dragonfly-dev).

Reinstall the server to update to the newest build or to apply a change, your data is kept.

The development builds are only published as container images. The installer downloads the image from `ghcr.io` (about 140 MB) and only keeps the `dragonfly` binary, which runs on the `ghcr.io/parkervcp/yolks:redis_8` image. The binary comes with debug symbols, which the installer strips to save disk space. The commit is printed at the end of the installation and on every start (`Starting dragonfly df-v...`).

## Server Startup

Dragonfly runs in the foreground and its log is shown in the console. The server is marked as started once Dragonfly logs `listening on`.

The console does not accept commands. Connect with any Redis client (for example `redis-cli -h <ip> -p <port> -a <password>`) using the password set in the `SERVER_PASSWORD` variable.

Stopping the server sends `SIGINT` to Dragonfly, which saves a final snapshot before it exits.

## Memory and Threads

Dragonfly needs at least 256 MB of memory per thread. The `Threads` variable defaults to `1`, raise it together with the server memory.
Setting it to `0` uses every CPU core of the host, which usually needs more memory than the server has.

## Persistence

Data is saved to `/home/container` as `dump-*.dfs` files on shutdown and on the `Snapshot Schedule` cron (every 5 minutes by default).

## Importing data from Redis

Switch a Redis server to this egg and reinstall it. No other step is needed:

* The installer copies the Redis `dump.rdb` to `backups/<date>-redis-<version>/`.
* Dragonfly imports `dump.rdb` on its first start. From then on it saves and loads its own `dump-*.dfs` files, and the old `dump.rdb` is no longer used.
* Unlike Dragonfly 2.0.2, the development build also reads the data format of Redis 8.6 and newer, as long as the data only uses types Dragonfly supports. Data from Redis modules, or types that only exist in newer Redis versions, may not be supported. In that case Dragonfly stops on start with an error and leaves `dump.rdb` as it is.

Going back to Redis is not automatic, because Dragonfly saves in its own format. To return to Redis with the original data, stop the server and copy `dump.rdb` back from `backups/`.

## License

Dragonfly is released under the [Business Source License 1.1](https://github.com/dragonflydb/dragonfly/blob/main/LICENSE.md). Running it for your own projects is fine, but offering it to third parties as a hosted in-memory data store service (for example, selling Dragonfly servers to customers) requires a commercial license from DragonflyDB.

## Server Ports

Ports required to run the server in a table format.

| Port    | default |
|---------|---------|
| Server  |  6379   |
