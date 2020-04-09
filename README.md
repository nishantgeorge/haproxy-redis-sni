A Docker Compose setup of two TLS-enabled Redis servers behind an HAProxy instance, using SNI.

There are 2 Redis databases, with host names:
1. `redis-1.acme.com`
1. `redis-2.acme.com`

Both of these host names resolve to the same host: the HAProxy instance. The HAProxy instance is configured
with the mapping of Redis database host name to IP address, and to use SNI to redirect the connection to the 
right database.


### Demo

1. Start the containers.
   ```bash
   docker-compose up -d
   ```
1. Start a shell on the `client` container.
   ```bash
   docker-compose exec client bash
   ```
1. Run the following Python code:
   ```python
   import redis

   r = redis.Redis(host='redis-1.acme.com', ssl=True, ssl_cert_reqs='none')
   r.ping()  # Should succeed
   ```

Note that SNI support is only available in `redis-py` starting with version `3.1.0`.
