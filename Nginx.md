NGIN    X
============

- [NGIN    X](#ngin----x)
  - [Nginx installation on Ubuntu](#nginx-installation-on-ubuntu)
  - [Reverse Proxy](#reverse-proxy)
  - [SSL certificate with Lets Encrypt](#ssl-certificate-with-lets-encrypt)


## Nginx installation on Ubuntu

| Command                       | Description                            |
| ----------------------------- | -------------------------------------- |
| `sudo apt-get install nginx ` | Install nginx on Ubuntu and start it   |
| `ps aux  grep nginx`| Show nginx processes running on system |
| `ulimit -n`| Show number of capable worker connections |



```
user www-data;
worker_processes: auto;

events {
    worker_connections 1024;
}

http {


    include mime.types;

    server{

        listen 80;
        server_name 167.99.35.107;

        root /bloggingtemplate;

        rewrite /guest /contactUs;

        try_files /findUs /contactUs;

        location =/contactUs {
            return 200 "Hello from a custom location!!";
        }

        location /find {
            return 200 "$hostname \n $connection_requests \n $nginx_version";
        }

    }
}

```


## Reverse Proxy

```
    
user www-data;
worker_processes auto;


events {
    worker_connections 1024;
}

http {

    include mime.types;


    server{

        listen 80;
        server_name ripunjoybuddha.site www.ripunjoybuddha.site;

        location / {
            proxy_pass http://localhost:3000;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded $proxy_add_x_forwarded_for;
        }

    }


    server{

        listen 80;
        server_name kolaart.com www.kolaart.com;

        location / {
            proxy_pass http://localhost:3001;
        }
    }
}

```

## Cache in the client side and Micro caching
```
http{

    server{

        listen 80;
        server_name ripunjoybuddha.site www.ripunjoybuddha.site;

        fastcgi_cache_path /tmp/cache_nginx levels=1:2 keys_zone=ZONE_1:100m inactive=60m;
        fastcgi_cache_key "$scheme$request_method$host$request_uri";


        location / {
            proxy_pass http://localhost:3000;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded $proxy_add_x_forwarded_for;

            fastcgi_cache ZONE_1;
            fastcgi_cache_valid 200 60m;
        }

        location ~* \.(css|js|jpg|jpeg|png) {
            add_header Cache-Control public;
            add_header Pragma public;
            add_header Vary Accept-Encoding;
            expires 2M;
        }
    }
}

```

## Caching in Proxy Server
```
http {
    proxy_cache_path  /data/nginx/cache  levels=1:2    keys_zone=STATIC:10m
    inactive=24h  max_size=1g;
    server {
        location / {
            proxy_pass             http://1.2.3.4;
            proxy_set_header       Host $host;
            proxy_buffering        on;
            proxy_cache            STATIC;
            proxy_cache_valid      200  1d;
            proxy_cache_use_stale  error timeout invalid_header updating http_500 http_502 http_503 http_504;
        }
    }
}
```



## Compress files with gzip
```
http{

    gzip on;
    gzip_comp_level 4;
    gzip_types text/plain text/css application/javascript aplication/json application/x-javascript text/xml application/xml application/xml+rss text/javascript;

    server{

        listen 80;
        server_name ripunjoybuddha.site www.ripunjoybuddha.site;

        location / {
            proxy_pass http://localhost:3000;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded $proxy_add_x_forwarded_for;
        }
    }
}

```

## Blacklist and WhiteList IP addresses
```
    location /admin{
        allow 1.2.3.4;
        deny all
    }
```

## Limit Network Bandwidth
```
    location / {
        limit_rate_after 30m;
        limit_rate 100M; #Any limit you want to provide
    }
```


## SSL certificate with Lets Encrypt

| Command                       | Description                            |
| ----------------------------- | -------------------------------------- |
| `sudo add-apt-repository ppa:certbot/certbot` | Step 1 |
| `sudo apt-get update`| Step 2 |
| `sudo apt-get install python3-certbot-nginx`| Step 3 |
| `sudo certbot --nginx -d domain.com -d www.domain.com`| Step 4 |
| `certbot renew --dry-run`| Step 5 |
