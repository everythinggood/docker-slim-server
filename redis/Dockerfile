FROM redis:latest

VOLUME "/data"

COPY  /data/ /data/


EXPOSE 6379

CMD ["redis-server","/data/redis.conf"]
