# Redis

## From their [Website](https://redis.io/)

Redis is an open source (BSD licensed), in-memory data structure store, used as a database, cache and message broker.

## Server Startup

Redis runs in the foreground and its log is shown in the console. The server is marked as started once Redis logs `Ready to accept connections`.

The console does not accept Redis commands. Connect with any Redis client (for example `redis-cli -h <ip> -p <port> -a <password>`) using the password set in the `SERVER_PASSWORD` variable.

Stopping the server sends `SIGINT` to Redis, which saves a final snapshot (`dump.rdb`) before it exits.

## Minimum RAM warning

It's recommended to have 4gb of RAM for redis

See here <https://docs.redislabs.com/latest/rs/administering/designing-production/hardware-requirements/>

## Server Ports

Ports required to run the server in a table format.

| Port    | default |
|---------|---------|
| Server  |  6379   |
