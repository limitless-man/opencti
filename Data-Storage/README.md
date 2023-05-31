<!-- Data Storage Docker Compose Description -->

## Redis Cluster with Nginx Proxy

Oh, you want to talk about Redis, huh? Alright, brace yourself for some clustered madness! This Docker Compose configuration sets up a Redis cluster with an Nginx proxy to handle all the sneaky proxying business.

### Docker Compose Configuration

The `docker-compose.yml` file is packed with services that will blow your mind:

- **Redis Instances**: We got not one, not two, but four Redis instances: `redis1`, `redis2`, `redis3`, and `redis4`. Each of these bad boys is based on the `redis:${REDIS_VERSION}` image. We're making sure they restart always, 'cause we don't want any dead Redis corpses lying around. These instances are connected to the `redis` network and a couple of proxy networks (`opencti_ingest_proxy` and `opencti_analyst_proxy`). We're also deploying them on nodes that match the `${NODE_HOSTNAME_REDIS}` constraint. Safety first, you know.

- **Nginx Proxy**: Our dear friend Nginx steps in as the proxy master. Sporting the `nginx:1.19.2-alpine` image, this bad boy knows how to handle all the incoming requests. The Nginx configuration is served through the `proxy_redis` config, which we conveniently mount into the Nginx container. This proxy is a master of resurrections, restarting on failure up to three times with a slight delay and a max window of 120 seconds. We also make sure it's placed on a node that matches the `${NODE_HOSTNAME_REDIS}` constraint. Gotta keep an eye on that naughty Nginx.

By combining the power of Redis instances and Nginx proxy magic, this setup gives you a clustered Redis system ready to take on any challenge. Just watch out for any rogue cache hits or proxy bottlenecks, ya hear?

### Nginx Proxy Configuration

Oh, baby, the Nginx proxy config is where the real action happens. It's like a backstage pass to the Redis show. Check it out:

```nginx
user nginx;
worker_processes auto;

error_log /var/log/nginx/error.log warn;
pid /var/run/nginx.pid;

events {
    worker_connections 4096;
}

stream {
    upstream redis_backend {
        server redis1:6379;
        server redis2:6379;
        server redis3:6379;
        server redis4:6379;
    }

    server {
        listen 6379;
        proxy_pass redis_backend;
    }
}
