# DATASCIENTEST JENKINS EXAM
# python-microservice-fastapi
Learn to build your own microservice using Python and FastAPI

## How to run??

 - Make sure you have installed `docker` and `docker-compose`
 - Run `docker-compose up -d`
 - Head over to http://localhost:8080/api/v1/movies/docs for movie service docs 
   and http://localhost:8080/api/v1/casts/docs for cast service docs

```sh
docker run --name my-custom-nginx-container \
-v ./nginx_config.conf:/etc/nginx/conf.d/default.conf \
-d -p 81:80 nginx
```

## @TODO

- docker-compose : nginx config KO
- docker push KO : denied: requested access to the resource is denied
- helm chart
- Jenkinsfile
