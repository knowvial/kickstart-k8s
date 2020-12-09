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
cat <<EOF > inndex.html
  <head>
    <title>My first docker app</title>
    <link rel="stylesheet" href="http://yui.yahooapis.com/3.5.1/build/cssreset/cssreset-min.css">    
    <link rel="stylesheet" href="styles.css">
  </head>
  <body>
      <section id="header">
      </section>

      <section id="main">
      </section>

      <section id="footer">
      </section>
      
  </body>
  </html>
EOF
```