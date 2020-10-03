# whiteaster-app

## Description
Simple docker based devops architecture implementation

```                        
                                                XXXXXXXX        XXXXXXX
                                              XX       XXX    XXX     XX  XXXXXX
                                          XXX            XXXXX         XXX     X
                                          XX                                  XX
                                           XX           INTERNET              X
                                            XX                       XXXX    XX
                                             XXX      XXX         XXX   XXXXXX
                                               XXXXXXX  XXXXXXXXXXX
                                                             +
                                                             |
                                                             |
                                                             |
                                                             |
                                                    +--------v---------+
                                                    |                  |
                                                    |   Traefik 2      |
                                                    |   Edge router    |
                                                    |                  |
                                                    +---------+--------+
                                                              |
       +---------------------------+--------------------------+
       |                           |                          |
+------v-----------+      +--------v---------+                |
|                  |      |                  |                |             +------------------+
|   Nginx          |      | Dummy-container  |                |             |                  |
|                  |      |                  |                +------------->  PostgreSQL      |
|                  |      |                  |                |             |                  |
+------------------+      +------------------+                |             |                  |
                                                              |             +------------------+
                                                              |
                                                              |
                                                              |             +------------------+
                                                              |             |                  |
                                                              +------------->  Redis           |
                                                              |             |                  |
                                                              |             |                  |
                                                              |             +------------------+
                                                              |
                                                              |
                                                              |             +------------------+
                                                              |             |                  |
                                                              +------------->  RabbitMQ        |
                                                                            |                  |
                                                                            |                  |
                                                                            +------------------+


```

### Traefik2
auto generated SSL
Let's Encrypt and redirection www to non-www.
- http://localhost > https://localhost
- http://www.localhost > https://localhost
- https://www.localhost > https://localhost

### Nginx + Dummy container
Simple generic content

### Others
Smallest stable version

## Getting Started

### Dependencies
* Internet connection
* Docker-compose
* Ubuntu
* Sufficient user rights

### Installing
* clone this repo
* open command line in repo directory
* create .env file to store postgreSQL password as POSTGRES_PASSWORD (for security resons it's not part of repo)
* Builds, (re)creates, starts, and attaches to containers for a service.
```docker-compose up -d```
```
Starting nginx                            ... done
Starting whiteaster-app_rabbitMQ_1        ... done
Starting whiteaster-app_postgreSQL_1      ... done
Starting whiteaster-app_redis_1           ... done
Starting whiteaster-app_dummy-container_1 ... done
Starting whiteaster-app_reverse-proxy_1   ... done
```

### Executing program
* Check running containers
```sudo docker ps```
![containers list](https://i.imgur.com/D57gmk8.png)



* Check Traefik dashboard
```http://127.0.0.1:8080/dashboard/#/```
![dashboard](https://i.imgur.com/j67m3zy.png)

* Test endpoints rediractions

```curl -H Host:dummy-container.docker.localhost http://localhost```

```curl -H Host:nginx-container.docker.localhost http://localhost```

Means that redirect http->https works
```
Moved Permanently_
```


* Add L flag to curl to see actuall service
```curl -kLH Host:dummy-container.docker.localhost http://localhost```
```Hostname: e12149ace723
IP: 127.0.0.1
IP: 172.19.0.3
RemoteAddr: 172.19.0.7:41678
GET / HTTP/1.1
Host: dummy-container.docker.localhost
User-Agent: curl/7.68.0
Accept: */*
Accept-Encoding: gzip
X-Forwarded-For: 172.19.0.1
X-Forwarded-Host: dummy-container.docker.localhost
X-Forwarded-Port: 443
X-Forwarded-Proto: https
X-Forwarded-Server: 7562439a7233
X-Real-Ip: 172.19.0.1
```

```curl -kLH Host:nginx.docker.localhost http://localhost```
```<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

```

## Help
There is some problem with SSL certificate, that's why curl use -k flag to skip the problem

To see more use verbose option:
```curl -vLH Host:nginx.docker.localhost http://localhost```

```
...
TLSv1.3 (OUT), TLS alert, unknown CA (560):
SSL certificate problem: unable to get local issuer certificate
...
```

For future possible to add two networks like:
```
networks:
  web:
    external: true
  backend:
    external: false
```
and assign them accordingly to services for better separation

## Authors

Contributors names and contact info

Maciej Pokorski <maciekpokorski@gmail.com>

## Version History

* 0.1
    * Initial Release (some problem with dynamic SSL certifacte)

## Acknowledgments
https://docs.docker.com/compose/
https://doc.traefik.io/traefik/getting-started/quick-start/
https://medium.com/swlh/alpine-slim-stretch-buster-jessie-bullseye-bookworm-what-are-the-differences-in-docker-62171ed4531d
https://www.nginx.com/blog/nginx-1-6-1-7-released/
https://redis.io/topics/quickstart
https://hub.docker.com/
https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers#Well-known_ports
https://traefik.io/blog/traefik-2-tls-101-23b4fbee81f1/
https://stackoverflow.com/questions/1393280/http-redirect-301-permanent-vs-302-temporary
https://doc.traefik.io/traefik/middlewares/redirectregex/
https://docs.docker.com/compose/compose-file/
https://www.youtube.com/watch?v=Gk9WER6DunE
