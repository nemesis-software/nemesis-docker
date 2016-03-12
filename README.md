# nemesis-docker
The docker compose configuration with all of its image configurations(database, redis, search server, nemesis and nginx).

## How to use docker-compose

### Starting the project

-`docker-compose up`

### Initialize the database

After the nemesis application is started you have to initialize the database with the samplestore data.
To do that use

-`curl -k -X POST http://nemesis:nemesis@localhost:8111/platform/database/init`

Note: If you are using Windows/Mac where there is no native docker support and you use virtualbox or any other local provider you have to specify their IP address instead of localhost.

Check the virtualbox/local provider IP address with
-`docker-machine ls`
You will see output similar to this:

```
NAME      ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER    ERRORS
default   *        virtualbox   Running   tcp://192.168.99.100:2376           v1.10.3
```

Then using the IP address in the "URL" column you have to initialize the nemesis samplestore data with `192.168.99.100` instead of `localhost`:
-`curl -k -X POST http://nemesis:nemesis@192.168.99.100:8111/platform/database/init`
