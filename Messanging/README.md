<!-- RabbitMQ Stack Description -->

## RabbitMQ Stack for OpenCTI

Ah, RabbitMQ! The magical message broker that keeps everything hopping. This Docker Compose configuration sets up a RabbitMQ stack to handle all your messaging needs in the OpenCTI world. Get ready for some epic messaging action!

### Docker Compose Configuration

This stack is built to withstand the fury of your messaging demands. Check out what it's got:

- **RabbitMQ Instances**: We got four RabbitMQ instances ready to chew on messages: `rabbitmq1`, `rabbitmq2`, `rabbitmq3`, and `rabbitmq4`. These instances are powered by the mighty `rabbitmq:${RABBITMQ_VERSION}` image. We set up the environment with some default user and password, 'cause security is important, even in the rabbit kingdom. Each RabbitMQ instance has its own volume to store its precious data. We're deploying replicas, but just one per instance, 'cause we don't want too many fluffy bunnies crowding around. We also place them on nodes that match the `${NODE_HOSTNAME_MESSAGING}` constraint. Gotta keep those bunnies organized!

- **Nginx Proxy**: Our favorite proxy, Nginx, is here to keep the RabbitMQ instances in line. The `nginx:1.19.2-alpine` image takes the stage, and we provide it with the `proxy_rabbitmq` config to guide its actions. This proxy is no pushover, restarting on failure up to three times, with a little delay and a maximum window of 120 seconds. Just to be safe, you know? We make sure to place this Nginx proxy on a node that matches the `${NODE_HOSTNAME_MESSAGING}` constraint. Watch out for those sneaky bunnies!

By combining the power of RabbitMQ instances and the Nginx proxy, this stack is ready to handle all your messaging traffic. It's like a rabbit party with Nginx as the bouncer, making sure only the right messages get through!

### Nginx Proxy Configuration

The Nginx proxy config for RabbitMQ is where the magic happens. It's like a secret recipe for rabbit stew. Brace yourself for the stream and the HTTP server blocks:

```nginx
user nginx;
worker_processes auto;

error_log /var/log/nginx/error.log warn;
pid /var/run/nginx.pid;

events {
    worker_connections 4096;
}

stream {
    upstream rabbitmq_tcp_backend {
        server rabbitmq1:5672;
        server rabbitmq2:5672;
        server rabbitmq3:5672;
        server rabbitmq4:5672;
    }

    server {
        listen 5672;
        proxy_pass rabbitmq_tcp_backend;
    }
}

http {
    upstream rabbitmq_http_backend {
        server rabbitmq1:15672;
        server rabbitmq2:15672;
        server rabbitmq3:15672;
        server rabbitmq4:15672;
    }

    server {
        listen 15672;
        server_name localhost;
        location / {
            proxy_pass http://rabbitmq_http_backend;
            proxy_set_header Host $http_host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_set_header X-NginX-Proxy true;
            proxy_connect_timeout 300;
            proxy_http_version 1.1;
            proxy_set_header Connection "";
        }
    }
}
