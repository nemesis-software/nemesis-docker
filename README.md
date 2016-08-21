# nemesis-docker
The docker compose configuration with all of its image configurations(database, redis, search server, nemesis and nginx).

## How to use docker-compose

### Starting the project

`docker-compose up`

MacOS X note

If you get ERROR: Couldn't connect to Docker daemon - you might need to run `docker-machine start default`.
You may need to run this two extra steps:

`docker-machine start default`

`eval "$(docker-machine env default)"`

=======
### Delete volumes
Docker compose tries really hard to keep volumes across container restarts, so if you screw your /var/lib/mysql directory the database from the envvar would not be recreated.
If this happens you can try to delete the volumes with 
````
docker-compose rm -fv 
````

and then 
````
docker-compose up -d
````
 again.

### Initialize the database

After the nemesis application is started you have to initialize the database with the samplestore data.
To do that use

`curl -k -X POST http://nemesis:nemesis@localhost:8111/platform/database/init`

Note: If you are using Windows/Mac where there is no native docker support and you use virtualbox or any other local provider you have to specify their IP address instead of localhost.

Check the virtualbox/local provider IP address with
`docker-machine ls`
You will see output similar to this:

```
NAME      ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER    ERRORS
default   *        virtualbox   Running   tcp://192.168.99.100:2376           v1.10.3
```

Then using the IP address in the "URL" column you have to initialize the nemesis samplestore data with `192.168.99.100` instead of `localhost`:

`curl -k -X POST http://nemesis:nemesis@192.168.99.100:8111/platform/database/init`

## How to use docker-cloud

### Install docker cloud

Linux & Windows

`pip install docker-cloud`

MacOS X

` brew install docker-cloud`

More info https://docs.docker.com/docker-cloud/tutorials/installing-cli/

type `docker-cloud -version` to check it is installed properly.

### Login

`docker login`

Alternativly you can export login information as variables

```
export DOCKERCLOUD_USER=<docker username>
export DOCKERCLOUD_PASS=<docker password>
```

### Create new stack

`docker-cloud stack create --name hello-world -f docker-cloud.yml`

### start the stack

`docker-cloud stack start 46aca402`

### Initialize the database

`curl -k -X POST http://nemesis:nemesis@localhost:8111/platform/database/init`

Note: app.nemesis.aa552bf1.svc.dockerapp.io:8111 is example host from docker so you need to specify your service host
=======
To run it follow:
 - docker-compose up -d
 - docker-compose logs

To initialize the site:
 - curl -k -X POST http://nemesis:nemesis@localhost:8111/platform/database/init

To stop it:
 - docker-compose down

