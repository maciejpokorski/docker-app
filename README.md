# whiteaster-app

## Description

Simple docker based devops architecture implementation

![architecture schema](https://i.imgur.com/dFkpp60.png)

### Traefik2
auto generated SSL
Let’s Encrypt and redirection www to non-www.
- http://localhost > https://localhost
- http://www.localhost > https://localhost
- https://www.localhost > https://localhost

### Nginx + Dummy container
Simple generic content

### Others
Smallest stable version

## Getting Started

### Dependencies

* Docker-compose
* Ubuntu

### Installing


* How/where to download your program

* Any modifications needed to be made to files/folders

### Executing program

* How to run the program
* Step-by-step bullets
```
code blocks for commands
```

## Help

Any advise for common problems or issues.
```
command to run if program contains helper info
```

## Authors

Contributors names and contact info

ex. Maciej Pokorski  

## Version History

* 0.1
    * Initial Release

## Acknowledgments
https://docs.docker.com/compose/
https://doc.traefik.io/traefik/getting-started/quick-start/
https://medium.com/swlh/alpine-slim-stretch-buster-jessie-bullseye-bookworm-what-are-the-differences-in-docker-62171ed4531d
https://www.nginx.com/blog/nginx-1-6-1-7-released/
