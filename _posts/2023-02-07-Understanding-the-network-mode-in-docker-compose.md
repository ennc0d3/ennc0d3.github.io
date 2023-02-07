# Understanding network_mode in Docker Compose

Docker Compose is a tool for defining and running multi-container docker
applications. When you create a service in a Docker Compose file, it is
automatically connected to a user-defined bridge network created by Compose.
This network allows containers within the same Docker Compose file to
communicate with each other using container names as hostnames.

For example, consider a docker-compose.yml file with two services, web and db:

```
version: '3'
services:
  web:
    image: nginx:alpine
    ports:
      - "80:80"
  db:
    image: postgres:alpine
    environment:
      POSTGRES_PASSWORD: example

```

In this example, both the web and db services are connected to the same
user-defined bridge network created by Docker Compose. This allows the web
service to connect to the db service using db as the hostname, and vice versa.

However, what if you have a scenario where you want to connect a service to a
specific network created by another service in the same Docker Compose file,
rather than the default user-defined bridge network? This is where the
network_mode option comes in.

The network_mode option allows you to specify the network that a service should
be connected to. You can set network_mode to service:[service name] to connect
a service to the network created by another service in the same Docker Compose
file.

For example, consider a docker-compose.yml file with three services, web, db,
and app: 

```
version: '3'
services:
  web:
    image: nginx:alpine
    ports:
      - "80:80"
  db:
    image: postgres:alpine
    environment:
      POSTGRES_PASSWORD: example
    networks:
      - mydbnet
  app:
    image: myapp
    network_mode: "service:db"
    networks:
      - myappnet

networks:
  mydbnet:
    driver: bridge
  myappnet:
    driver: bridge


```

In this example, the db service creates a network named mydbnet, and the app
service connects to that network by specifying network_mode: "service:db". This
allows the app service to connect to the db service using db as the hostname,
while running on a separate network named myappnet. The web service is
connected to the default user-defined bridge network, and it is not able to
communicate with the app service, or vice versa.

In conclusion, the network_mode option in Docker Compose provides a way to
connect a service to a specific network created by another service in the same
Docker Compose file. This allows you to control the network connectivity
between services and separate the network for each service.

References:

[https://docs.docker.com/compose/compose-file/compose-file-v3/](Docker Compose
Reference)
