# Redis

## From their [Website](https://redis.io/)

Redis is an open source (BSD licensed), in-memory data structure store, used as a database, cache and message broker.

## Redis Version

This egg uses the `ghcr.io/parkervcp/yolks:redis_8` image, which is rebuilt every week from the official Redis 8 image and ships the latest Redis 8 release available for its base image.
The exact version is printed in the console on every start (`Redis version=...`).

## Server Startup

Redis runs in the foreground and its log is shown in the console. The server is marked as started once Redis logs `Ready to accept connections`.

The console does not accept Redis commands. Connect with any Redis client (for example `redis-cli -h <ip> -p <port> -a <password>`) using the password set in the `SERVER_PASSWORD` variable.

Stopping the server sends `SIGINT` to Redis, which saves a final snapshot (`dump.rdb`) before it exits.

## Migrating from an older Redis

Switch the server to this egg and reinstall it. No other step is needed:

* The installer copies the existing data (`dump.rdb`, and `appendonlydir` if present) to `backups/<date>-redis-<version>/`, once per Redis version that wrote it, so reinstalling again does not pile up copies.
* Redis 8 loads `dump.rdb` on the next start. This was tested with data written by Redis 5.0, 6.2, 7.4 and 8.2.
* The existing `redis.conf` is kept.

A newer Redis can read older data, but not the other way round (for example, Redis 8.10 writes a newer file format than Redis 8.2 and 7.4). To go back to an older version, stop the server and copy `dump.rdb` back from `backups/`.

If the egg is switched without reinstalling, the data still loads, but no backup is made. Backups use disk space, so delete the old ones once you no longer need them.

## Minimum RAM warning

It's recommended to have 4gb of RAM for redis

See here <https://docs.redislabs.com/latest/rs/administering/designing-production/hardware-requirements/>

## Server Ports

Ports required to run the server in a table format.

| Port    | default |
|---------|---------|
| Server  |  6379   |
