<!-- MinIO Stack Description -->

## MinIO Stack for OpenCTI

Welcome to the MinIO stack, where we store and manage your data with the power of object storage. This Docker Compose configuration sets up a robust MinIO cluster to handle all your storage needs in the OpenCTI world. Let's dive into the details!

### Docker Compose Configuration

This stack is designed to ensure the durability and availability of your data. Check out what it offers:

- **MinIO Instances**: We've got four MinIO instances ready to go: `minio1`, `minio2`, `minio3`, and `minio4`. These instances are powered by the `minio/minio:${MINIO_VERSION}` image. Each instance is configured to run the MinIO server with two data drives (`data1` and `data2`). We set the console address to `:9001` and provide the necessary environment variables, such as the root user and password, for secure access. We also configure a health check to ensure the MinIO server stays up and running. Each MinIO instance is placed on nodes that match the `${NODE_HOSTNAME_MINIO}` constraint. Our fluffy bunnies need their space!

- **Nginx Proxy**: Nginx takes the stage once again as our trusty proxy. We use the `nginx:1.19.2-alpine` image and provide it with the `proxy_minio` config. This Nginx proxy is a resilient one, with a restart policy in case of failures. We place it on a node that matches the `${NODE_HOSTNAME_MINIO}` constraint. Our proxy game is strong!

By combining the power of multiple MinIO instances and the Nginx proxy, this stack ensures high availability and fault tolerance for your precious data. You can trust your bunnies to keep your files safe and sound!

### Nginx Proxy Configuration

The Nginx proxy configuration for MinIO is where the magic happens. Brace yourself for the server blocks:

```nginx
user nginx;
worker_processes auto;

error_log /var/log/nginx/error.log warn;
pid /var/run/nginx.pid;

events {
    worker_connections 4096;
}

http {
    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log /var/log/nginx/access.log main;
    sendfile on;
    keepalive_timeout 65;

    upstream minio {
        server minio1:9000;
        server minio2:9000;
        server minio3:9000;
        server minio4:9000;
    }

    upstream console {
        ip_hash;
        server minio1:9001;
        server minio2:9001;
        server minio3:9001;
        server minio4:9001;
    }

    server {
        listen 9000;
        listen [::]:9000;
        server_name localhost;

        ignore_invalid_headers off;
        client_max_body_size 0;
        proxy_buffering off;
        proxy_request_buffering off;

        location / {
            proxy_set_header Host $http_host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;

            proxy_connect_timeout 300;
            proxy_http
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            
            chunked_transfer_encoding off;

            proxy_pass http://console;
        }
    }
}