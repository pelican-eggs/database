# Dragonfly

## From their [GitHub](https://github.com/dragonflydb/dragonfly)

Dragonfly is an in-memory data store built for modern application workloads. It is fully compatible with Redis and Memcached APIs and requires no code changes to adopt.

## Dragonfly Version

The installer downloads the official Dragonfly binary from the [GitHub releases](https://github.com/dragonflydb/dragonfly/releases) (x86_64 or aarch64).
The version is set with the `Dragonfly Version` variable (`2.0.2` by default, or `latest`). Reinstall the server to change it, your data is kept.

The server runs on the `ghcr.io/parkervcp/yolks:redis_8` image.
The exact version is printed in the console on every start (`Starting dragonfly df-v...`).

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
* This was tested with data written by Redis 5.0, 6.2, 7.4 and 8.2 (strings, hashes, lists, sets, sorted sets, TTLs and several databases). Data from Redis modules, or types that only exist in newer Redis versions, may not be supported by Dragonfly.
* Dragonfly 2.0.2 only reads the data format of Redis 8.4 and older. Redis 8.6 and newer save in a newer format, and the `redis_8` image ships the latest Redis 8, so data from the Redis 8 egg can't be imported yet. Dragonfly then stops on start with `RDB Version <n> is not supported` and leaves `dump.rdb` as it is. Switch the server back to the Redis 8 egg to keep using that data.

Going back to Redis is not automatic, because Dragonfly saves in its own format. To return to Redis with the original data, stop the server and copy `dump.rdb` back from `backups/`.

## License

Dragonfly is released under the [Business Source License 1.1](https://github.com/dragonflydb/dragonfly/blob/main/LICENSE.md). Running it for your own projects is fine, but offering it to third parties as a hosted in-memory data store service (for example, selling Dragonfly servers to customers) requires a commercial license from DragonflyDB.

## Server Ports

Ports required to run the server in a table format.

| Port    | default |
|---------|---------|
| Server  |  6379   |
