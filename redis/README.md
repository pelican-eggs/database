# Redis

## From their [Website](https://redis.io/)
Redis is an open source (BSD licensed), in-memory data structure store, used as a database, cache and message broker.

## Console
The server console accepts Redis commands directly. Type any Redis command (`PING`, `SET foo bar`, `GET foo`, `INFO`, etc.) and the response will appear inline. Authentication is handled automatically using the `SERVER_PASSWORD` variable, so there's no need to run `AUTH`.

Note: each command runs in a fresh `redis-cli` connection, so stateful operations like `MULTI`/`EXEC` transactions, `SUBSCRIBE`, and `MONITOR` won't work from the console. For those, connect with an external client over the server's port.

## Stopping the Server
The stop button sends `SHUTDOWN SAVE` to Redis, which writes a final RDB snapshot before exiting. Data persists across restarts.

## Minimum RAM warning
It's recommended to have 4gb of RAM for redis. See <https://docs.redis.com/latest/rs/installing-upgrading/install/plan-deployment/hardware-requirements/>.

## Server Ports
| Port    | default |
|---------|---------|
| Server  |  6379   |