# Deployment of Frontend

## Update `nuxt.config.js`
Change the axios **base_url** to your deployed backend URL

## Docker 
1) Update your image repository URL in `docker-compose.yml` (E.g DockerHub)
2) Run the following commands

```
docker-compose down

docker-compose build

docker tag {image_url} {image_url}:latest

docker push {image_url}:latest
```

3) Access your VM and copy the `docker-compose.yml`
4) Run `docker-compose up -d`
5) To verify if the docker container is running, run `docker ps`


## Set up your VM Networking

Route your traffic using nginx to proxy to port 3000 (127.0.0.0:3000)
[nginx](https://linuxiac.com/reverse-proxy-with-nginx/)

