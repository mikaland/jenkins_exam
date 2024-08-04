# DATASCIENTEST JENKINS EXAM

## python-microservice-fastapi

Learn to build your own microservice using Python and FastAPI

## How to run??

- Make sure you have installed `docker` and `docker-compose`
- Run `docker-compose up -d`
- Head over to <http://localhost:8080/api/v1/movies/docs> for movie service docs
 and <http://localhost:8080/api/v1/casts/docs> for cast service docs

## NGINX

```sh
docker run -d --name my-nginx \
-p 8001:80 \
nginx

curl localhost:8001
```

## POSTGRESQL

```sh
POSTGRES_DB=test
POSTGRES_PASSWORD=password
POSTGRES_USER=admin
```

```sh
docker run -d --name postgres \
-e POSTGRES_PASSWORD=$POSTGRES_PASSWORD \
-e POSTGRES_USER=$POSTGRES_USER \
-e POSTGRES_DB=$POSTGRES_DB \
postgres

POSGRES_HOST=$(docker inspect $(docker ps -q) | jq -r '.[].NetworkSettings.IPAddress')

docker run -it --rm postgres psql -h "$POSGRES_HOST" -U $POSTGRES_USER -d $POSTGRES_DB
docker run -it --rm postgres psql "postgres://$POSTGRES_USER:$POSTGRES_PASSWORD@$POSGRES_HOST:5432/$POSTGRES_DB"
```

## K8S debugging

```sh
# networking tools
kubectl run tmp-shell --rm -i --tty --image nicolaka/netshoot -n default -- /bin/bash
# postgres client
kubectl run tmp-postgres --rm -i --tty --image postgres -n default -- /bin/bash
```
