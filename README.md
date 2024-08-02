# DATASCIENTEST JENKINS EXAM

## python-microservice-fastapi

Learn to build your own microservice using Python and FastAPI

## How to run??

- Make sure you have installed `docker` and `docker-compose`
- Run `docker-compose up -d`
- Head over to http://localhost:8080/api/v1/movies/docs for movie service docs 
 and http://localhost:8080/api/v1/casts/docs for cast service docs

## K8S debugging

```sh
# networking tools
kubectl run tmp-shell --rm -i --tty --image nicolaka/netshoot -n exam -- /bin/bash
# postgres client
kubectl run tmp-postgres --rm -i --tty --image postgres -- /bin/bash
```

## NGINX

```sh
docker run --name my-custom-nginx-container \
-v ./default.conf:/etc/nginx/conf.d/default.conf \
-d -p 8000:80 nginx
```

## POSTGRESQL

```sh
POSTGRES_DB=<YOUR_DB>
POSTGRES_PASSWORD=<YOUR_BUCKET>
POSTGRES_USER=<YOUR_REGION>
```

```sh
# psql "postgres://admin:password@db.exam.svc.cluster.local:5432/cast?sslmode=require"
psql "postgres://admin:password@cast-db.exam.svc.cluster.local:5432/cast_db_dev"
psql "postgres://admin:password@cast-db/cast_db_dev"
psql "postgres://admin:password@movie-db.exam.svc.cluster.local:5432/movie_db_dev"
psql "postgres://admin:password@movie-db/movie_db_dev"
```

```sh
docker run --name postgres \
-e POSTGRES_PASSWORD=password \
-e POSTGRES_USER=admin \
-e POSTGRES_DB=cast-db \
-d postgres

docker inspect $(docker ps -aq) | grep "IPAddress"

docker run -it --rm postgres psql -h 172.17.0.2 -U admin -d storedb
psql -h some-postgres -U postgres
```
