FROM redis:latest

VOLUME "/data"

COPY  /data/dump.rdb /data/

COPY /data/redis.conf /data/

EXPOSE 6379

CMD ["redis-server","/data/redis.conf"]
