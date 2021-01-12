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


## SSL certificate with Lets Encrypt

| Command                       | Description                            |
| ----------------------------- | -------------------------------------- |
| `sudo add-apt-repository ppa:certbot/certbot` | Step 1 |
| `sudo apt-get update`| Step 2 |
| `sudo apt-get install python3-certbot-nginx`| Step 3 |
| `sudo certbot --nginx -d domain.com -d www.domain.com`| Step 4 |
| `certbot renew --dry-run`| Step 5 |