# A minimal nginx configuration to publis api rest

This repo contains code for building a simple static website served using an Nginx container inside Docker.

# Api

Let's imagine a Soccer API base url in some server of your organization:

> http://10.10.10.78:8085

With some endpoint

> /livescore/now

First attempt to consume this endpoint will be:

> curl http://10.10.10.78:8085/livescore/now ...

Awful right? So, if we want something like this:

> curl http://soccer.api.com/livescore/now ...

Yo need to configure some load balancer. In this repository I will show you how to do this with **NGINX**

# Nginx proxy pass

Just configure the file : **default.conf**

```
server {
    listen  80;

    location ~ ^/livescore/now/(.*)$ {
        proxy_pass   http://10.10.10.78:8085/$1$is_args$args;
    }
}
```

Or ff you have a purchased certificate with domain: **soccer.api.com**

```
server {
    listen  80;
    server_name soccer.api.com;

    location ~ ^/livescore/now/(.*)$ {
        proxy_pass   http://10.10.10.78:8085/$1$is_args$args;
    }
}
```

# Docker build

To build a Docker image from the Dockerfile, run the following command from inside this directory

```sh
$ docker build -t my-nginx-image .
```

# Docker run

To run the image in a Docker container, use the following command

```sh
$ docker run -itd --name my-nginx --publish 80:80 my-nginx-image
```

This will start serving the nginx site on port 80.

# The end

![https://raw.githubusercontent.com/jrichardsz/static_resources/master/hannibal-smith-happy-face.jpeg](https://raw.githubusercontent.com/jrichardsz/static_resources/master/hannibal-smith-happy-face.jpeg)

You could offer your api with out port

> curl http://my-nginx-ip/livescore/now ...

Or if you have a purchased domain:

> curl http://soccer.api.com/livescore/now ...
