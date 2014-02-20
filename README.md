# airquality docker deployment

Deploys the [airquality app](https://github.com/javouhey/airquality) using [docker](https://www.docker.io/).

## Docker containers

Docker allows us to apply the SRP (single responsibility principle) to our deployment process.

<img src="airquality-docker-logo.png" width="650" alt="docker containers"/>

## How it works

Some lessons::

* I chose to use [supervisord](http://supervisord.org/) to be the main entrypoint for each container because some processes don't handle `docker stop` commands well. e.g. `mongod` will not know how to handle the `SIGTERM` signal.
* When I run the `mongodb` container, I mount a host volume. An alternative is to use [data only container](https://github.com/toffer/docker-data-only-container-demo) for the volumes.

### Build the images

*Note* I will add the instructions for the `web` container soon.

```sh
$ docker build --rm=true -t raverun/mongodb mongodb

$ docker build --no-cache=true --rm=true -t raverun/poll twitter

$ docker images
REPOSITORY       TAG     IMAGE ID      CREATED       VIRTUAL SIZE
raverun/poll     latest  29a4254dcd4e  8 hours ago   235.9 MB
raverun/mongodb  beta1   5cc91ce65ea3  25 hours ago  451.9 MB
```

### Run the images

Run the following in order.

#### mongodb

```sh
$ docker run -d -expose 44444 --name="mongo" 
             -v /home/me/hostdir:/mongowork:rw raverun/mongodb:beta1
```

#### twitter

This container will poll twitter for air quality reports & saves to mongodb.

```sh
$ docker run -d --name="tweeps" -e CONSUMER_KEY=key -e CONSUMER_SECRET=secret1 
                -e OAUTH_TOKEN=token -e OAUTH_TOKEN_SECRET=secret2 
                -link mongo:mongolink raverun/poll:latest
```
The `-link` will inject the exposed ports & IP address into our `twitter` container. This is an example of the environment variables that wll be injected::

```sh
MONGOLINK_PORT=tcp://172.17.0.2:44444
MONGOLINK_PORT_44444_TCP=tcp://172.17.0.2:44444
MONGOLINK_PORT_44444_TCP_ADDR=172.17.0.2
MONGOLINK_PORT_44444_TCP_PORT=44444
MONGOLINK_PORT_44444_TCP_PROTO=tcp
```
#### summary

You can see what is running::

```sh
$ docker ps

CONTAINER ID  IMAGE                 COMMAND               STATUS        PORTS      NAMES
29a4254dcd4e  raverun/poll:latest   /usr/bin/supervisord  Up 3 seconds             tweeps 
5cc91ce65ea3  raverun/mongodb:beta1 usr/bin/mongod --con  Up 10 seconds 44444/tcp  mongo
```

You can send other signals to the running containers::
```sh
$ docker kill --signal=INT 5cc91ce65ea3
```
You can stop & restart::
```sh
$ docker stop 29a4254dcd4e   
$ docker start 29a4254dcd4e 
```

> The end
