# Commands and utilities

Below docker volumes to be created before starting server deployment. This is done first time.
```
docker volume create --name=mender-artifacts
docker volume create --name=mender-db
docker volume create --name=mender-elasticsearch-db
docker volume create --name=mender-redis-db
```


Package delivered will contain the YML files.

Follow the bellow steps to launch the server. 
```
$cd smarthing-server
$cd production
$cd ./run start
```

To stop
```
$cd smarthing-server
$cd production
$cd ./run stop
```

To upgrade, update the docker image tags in docker_compose.yml, then

```
$cd smarthing-server
$cd production
$cd ./run up
```

To Remove all the containers

```
$cd smarthing-server
$cd production
$cd ./run rm
```
