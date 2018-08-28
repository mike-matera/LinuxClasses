In this lesson you'll create a simple web application from scratch and "containerize" it. Lecture slides are[here](https://docs.google.com/presentation/d/1AesHs7txbJ0W6lhle0WuCFpDnkrm9Sgv4szmLKFV6aQ/edit?usp=sharing).

### Commands 

  * docker-compose

### Configuration 

  * docker-compose.yaml

### Further Reading 

  * https://docs.docker.com/compose/

Contents
  - [1 Commands](#TOC_Commands_)
  - [1.1 Configuration](#TOC_Configuration)
  - [1.2 Further Reading](#TOC_Further_Reading)

  - [2 Introduction](#TOC_Introduction)
  - [3 Installation](#TOC_Installation)
  - [4 Composing a Cluster](#TOC_Composing_a_Cluster)

## Introduction 

Last week you used a Dockerfile to create a container instance. Your container ran a single, simple web application. Most web applications, however, require you to have multiple different services. In the Docker world each services belongs in its own container. Docker Compose is an easy way to specify multiple, interlinked, containers and start them all at once. Docker Compose makes it possible to bundle an entire web application into a single file describing how to build the application.

## Installation 

Docker Compose is still very new. To install it you should get the latest bleeding-edge version from GitHub:
[ https://github.com/docker/compose/releases](https://github.com/docker/compose/releases)
The page has instructions for downloading and installing the Docker Compose executable. The installation is as simple as moving the executable to a directory in your path. This is decidedly not the best way to install software but...

## Composing a Cluster 

Compose uses a YAML file to describe your application. Start by creating a directory for your composed application:

```
app-server$ mkdir ~/Wordpress
app-server$
cd ~/Wordpress
```

Now create a file called docker-compose.yaml in that directory and paste the following contents into it:

```
version: '2'
services:
 db:
  image: mysql:5.7
  volumes:
   - db_data:/var/lib/mysql
  restart: always
  environment:
   MYSQL_ROOT_PASSWORD: somewordpress
   MYSQL_DATABASE: wordpress
   MYSQL_USER: wordpress
   MYSQL_PASSWORD: wordpress
 wordpress:
  depends_on:
   - db
  image: wordpress:latest
  ports:
   - "8000:80"
  restart: always
  environment:
   WORDPRESS_DB_HOST: db:3306
   WORDPRESS_DB_USER: wordpress
   WORDPRESS_DB_PASSWORD: wordpress
volumes:
  db_data:
```

Save the file and run the following command.

```
app-server$ docker-compose up
```

Wait for the downloads to complete and you now have a Wordpress blog.