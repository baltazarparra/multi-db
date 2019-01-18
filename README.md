# Docker commands

Start
`service docker start`

Checar o que está rodando
`docker ps`

Entrar no container
`docker exec -it postgres /bin/bash`

# The Post-installation steps for Linux documentation reveals the following steps:

- Create the docker group.
`sudo groupadd docker`
- Add the user to the docker group.
`sudo usermod -aG docker $(whoami)`
- Log out and log back in to ensure docker runs with correct permissions.
- Start docker.
`sudo service docker start`

# Multi DataSources

## Install Postgres
```
docker run \
    --name postgres \
    -e POSTGRES_USER=baltazarparra \
    -e POSTGRES_PASSWORD=mypassword \
    -e POSTGRES_DB=heroes \
    -p 5432:5432 \
    -d \
    postgres
```

## Adminer link
```
docker run \
    --name adminer \
    -p 8080:8080 \
    --link postgres:postgres \
    -d \
    adminer
```

## Install Mongo
```
docker run \
    --name mongodb \
    -p 27017:27017 \
    -e MONGO_INITDB_ROOT_USERNAME=admin \
    -e MONGO_INITDB_ROOT_PASSWORD=admin123 \
    -d \
    mongo:4
```

## Mongoclient link
```
docker run \
    --name mongoclient \
    -p 3000:3000 \
    --link mongodb:mongodb \
    -d \
    mongoclient/mongoclient
```

Create user
```
docker exec -it mongodb \
    mongo --host localhost -u admin -p admin123 --authenticationDatabase admin \
    --eval "db.getSiblingDB('herois').createUser({user: 'baltazarparra', pwd: 'admin123', roles: [{role: 'readWrite', db: 'herois'}]})"
```