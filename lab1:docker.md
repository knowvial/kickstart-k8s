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
  <head>
    <title>My first docker app</title>
    <link rel="stylesheet" href="http://yui.yahooapis.com/3.5.1/build/cssreset/cssreset-min.css">    
    <link rel="stylesheet" href="https://raw.githubusercontent.com/knowvial/kickstart-k8s/main/resources/styles.css">
  </head>
  <body>
      <section id="header">Kickstart K8s in a day</section>
      <section id="main">This is a sample static website.</section>
      <section id="footer">Visit us at https://bigdatatrunk.com/</section>
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
docker run -d --name my-web-app1 my-web-app
docker run -d --name my-web-app2 my-web-app
docker ps
```

> Execute commands inside one of the containers.
```
docker exec -it my-web-app1 /bin/bash
ls
exit
```

> Optional: upload the docker image to Docker Hub. 
> Signup on https://hub.docker.com/.
```
docker tag my-web-app <docker-hub-user-name>/my-web-app
```