# MongoDB in a Container

## Installing MongoDB

To install community version of the database follow these [instructions](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-red-hat/)

The location of  MongoDB RPM [repository](https://repo.mongodb.org/yum/redhat/8/)

### MongoDB with Docker

- [Install MongoDB Community with Docker](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-community-with-docker/)
- [Docker and MongoDB](https://www.mongodb.com/compatibility/docker)
- [Docker Image](https://hub.docker.com/r/mongodb/mongodb-community-server)

export MONGODB_VERSION=6.0.6-ubi8
docker run --name mongodb -d mongodb/mongodb-community-server:$MONGODB_VERSION

podman run --name mongodb -d mongodb/mongodb-community-server:6.0.6-ubi8

docker run --name mongodb -d -p 27017:27017 mongodb/mongodb-community-server:$MONGODB_VERSION

### Random Stuff

https://www.baeldung.com/linux/mongodb-as-docker-container

Liveness and Readiness check: mongosh --eval "db.adminCommand('ping')"

podman run --rm -v /var/run/docker.sock:/var/run/docker.sock docker.io/mongodb/mongodb-community-server:6.0.6-ubi8 node-app:latest
podman run --rm -v /var/run/docker.sock:/var/run/docker.sock mongodb/mongodb-community-server:6.0.6-ubi8 node-app:latest