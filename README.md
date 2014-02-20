# airquality docker deployment

Deploys the [airquality app](https://github.com/javouhey/airquality) using [docker](https://www.docker.io/).

## Docker containers

Docker allows us to apply the SRP (single responsibility principle) to our deployment process.

<img src="airquality-docker-logo.png" width="650" alt="docker containers"/>

## How it works

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

TBD
