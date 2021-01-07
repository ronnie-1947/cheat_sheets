NGIN    X
============

- [NGIN    X](#ngin----x)
  - [Nginx installation on Ubuntu](#nginx-installation-on-ubuntu)


## Nginx installation on Ubuntu

| Command                       | Description                            |
| ----------------------------- | -------------------------------------- |
| `sudo apt-get install nginx ` | Install nginx on Ubuntu and start it   |
| `ps aux  grep nginx`    | Show nginx processes running on system |



```
events {

}

http {


    include mime.types;

    server{

        listen 80;
        server_name 167.99.35.107;

        root /bloggingtemplate;

        location =/contactus {
            return 200 "Hello from a custom location!!";
        }

        location /find {
            return 200 "$hostname \n $connection_requests \n $nginx_version"
        }
    }
}

```