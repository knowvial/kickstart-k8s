# Exploring docker
In this lab, you will learn how to 
- Create a simple static web application
- Create a docker file
- Package application as a docker image
- Create a docker image
- Create a container from that image
- Check container statistics
- Navigate inside container and execute commands

> Go to https://www.katacoda.com/courses/docker/2

> Run the following command to create a HTML page
```
cat <<EOF > index.html
<html>
  <body>
      <div>Welcome to Kickstart K8s in a day!</div>
  </body>
</html>
EOF
```
> Create docker file
```
cat <<EOF > Dockerfile
FROM ubuntu
RUN apt-get update
RUN apt-get install nginx -y
COPY index.html /var/www/html/
EXPOSE 80
CMD ["nginx","-g","daemon off;"]
EOF
```

> Create an image
```
docker build -t my-web-app .
docker images
```

> Create two containers from the image. Container is an instance of the image.
```
docker run -d --name my-web-app1 -p 80:80 my-web-app
docker run -d --name my-web-app2 -p 3001:80 my-web-app
docker ps
```
> Check the website by clicking "+" button next to "Terminal" and select "View HTTP port 80 on Host 1"
!(/images/lab1-1.jpg)

> Execute commands inside one of the containers.
```
docker exec -it my-web-app1 /bin/bash
ls
exit
```

| Optional: upload the docker image to Docker Hub. 
> Signup on https://hub.docker.com/.
> Enter user id annd password when prompted
```
docker login
```
> Tag the image and upload to Docker Hub (Container registry) 
```
docker tag my-web-app <docker-hub-user-name>/my-web-app
```
