# Commands and utilities

Below docker volumes to be created before starting server deployment. This is done first time.
```
docker volume create --name=mender-artifacts
docker volume create --name=mender-db
docker volume create --name=mender-elasticsearch-db
docker volume create --name=mender-redis-db
```


Package delivered will contain the YML files.
## Build all up
```
./run up -d

```

## verification 
```
./run ps
```

You should see output as below.

```
                Name                              Command               State           Ports
------------------------------------------------------------------------------------------------------
smarthingprod_mender-action_1           /usr/bin/action --config / ...   Up      8080/tcp
smarthingprod_mender-api-gateway_1      /entrypoint.sh                   Up      0.0.0.0:443->443/tcp
smarthingprod_mender-conductor_1        /srv/start_conductor.sh          Up      8080/tcp, 8090/tcp
smarthingprod_mender-configurations_1   /usr/bin/configurations -- ...   Up      8080/tcp
smarthingprod_mender-deployments_1      /entrypoint.sh --config /e ...   Up      8080/tcp
smarthingprod_mender-device-auth_1      /usr/bin/deviceauth --conf ...   Up      8080/tcp
smarthingprod_mender-elasticsearch_1    /docker-entrypoint.sh elas ...   Up      9200/tcp, 9300/tcp
smarthingprod_mender-gui_1              /entrypoint.sh                   Up      80/tcp
smarthingprod_mender-inventory_1        /usr/bin/inventory --confi ...   Up      8080/tcp
smarthingprod_mender-localconf_1        /usr/bin/localconf --confi ...   Up      8080/tcp
smarthingprod_mender-mongo_1            docker-entrypoint.sh mongod      Up      27017/tcp
smarthingprod_mender-redis_1            /redis/entrypoint.sh             Up      6379/tcp
smarthingprod_mender-useradm_1          /usr/bin/useradm --config  ...   Up      8080/tcp
smarthingprod_minio_1                   /usr/bin/docker-entrypoint ...   Up      9000/tcp
smarthingprod_storage-proxy_1           /usr/local/openresty/bin/o ...   Up      0.0.0.0:9000->9000/tcp

```

Once the system is up, follow command below to create the first user

```
./run exec smarthingprod_mender-useradm /usr/bin/useradm create-user --username=myusername@host.com --password=mysecretpassword --role=admin

```
> two roles are supported as of now : admin and viewer

Follow the bellow steps to launch the server by updating any docker images.
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
