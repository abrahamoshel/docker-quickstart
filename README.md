# Docker QuickStart

## Description

This repo is meant to be a training tool to help others learn the basics of docker and get them comfortable with containerizing applications.

### Drinks

```$ docker run <imagesname> <command>```

If the command that you use makes a change to the base image than that change is not forgotten but it also is not saved.

To save those changes much like git we need to commit the changes. It looks like this (tag is optional):

```$ docker commit <containerID> <repository:tag>```


To see all running containers:

```$ docker ps```

(note if you pass the -l to the ps command you will see all containers even if stopped. Useful for if you just ran a command on a container and want to commmit those changes.)

To see a running containers args and other information like IP address run:

```$ docker inspect <containerID>```

### Starters

Let start with creating a hello world nginx container that we can build off of. To start we will use the base image of `ubuntu` that has the `tag 14.04` and we will install `nginx` on top of it.

  $ docker run ubuntu:14.04 apt-get install nginx -y


Let save the change by running something like this:

  $ docker commit a78ea3cf03d7 abrahamoshel/nginx:install

  $ docker commit <containerID> <username>/<project>:<tag>

If you would like to verify that the image was saved you can list images that have been pulled or created by running:

  $ docker images

Let's run our shinny new image and expose port 80 of the host machine to port 80 on the container:

  $ docker run -it  -p 80:80 abrahamoshel/nginx:install bin/bash

While is it running try to hit port 80 of the host machine. You are not able to hit nginx are. That is because it isn't running. Going back to the prompt of the running container let start nginx.

  $ service nginx start

Now we should be able to hit it. So that means that we should be able to run:

  $ docker run -it  -p 80:80 abrahamoshel/nginx:install service nginx start

If you give it a try you'll see that it doesn't work. The reason is that nginx by default runs in daemon mode so as soon as nginx starts the container stops because nginx goes to the background.

Lets edit the nginx.conf to not run in daemon mode. Simplely add this line outside of the http block

```daemon off;```


Remember if you `exit` out of the container your changes will be lost so let detach from the container this way CTRL+P followed by CTRL+Q.

We can now commit the change like before:

  $ docker commit 563a5a0d9e11 abrahamoshel/nginx:configured

After commiting the change. Go ahead and stop the container from running.

  $ docker kill <containerID>

Lets try to see if we can get the hello world from nginx with the changes we just commited.

  $ docker run -d -p 80:80 abrahamoshel/nginx:configured service nginx start

Visitng the host on port 80 you should be able to see 'Welcome to nginx!' If you didn't go back through and see what step you might have missed.


We probably don't want to do that for every container that we want nginx on so lets automated it with a Dockerfile. This is allow us to create reusable and quickly build containers.

Take a look at the Dockerfile in the root of this project for an example. And see how it looks like the commands that we ran manually. To get nginx up and running from the file all you should have to do is run.

  $ docker build -t <username/repo:tag> .

This tells docker to tag the build and that the dockerfile is in the currect directory. Normally you would not build a docker image outside the directory your in. Besides the Dockerfile build only in the context of the directory that it is located in. But if you did this is how to build from a different location than the current directory.

  $ docker build -t testing - < <path/to/file>


### Sides

Soon to be in the local folder

### Entrées

Soon to be in the prod folder
