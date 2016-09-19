# Redis

Redis running version 3.2.3 on Alpine Linux 3.4

#### Running
```
docker run --name redis -d \
  -p 6379:6379 --restart=always \
  -v /srv/docker/redis:/var/lib/redis \
  strues/redis
```

#### Logging

Append `--logfile /var/log/redis/redis-server.log` when starting the container.

Then view the logs with `docker exec -it redis tail -f /var/log/redis/redis-server.log`

replace "redis" with whatever you named your container. In the case of the example above, it's named redis.
