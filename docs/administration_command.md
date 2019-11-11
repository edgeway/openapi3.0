# Commands and utilities

Below docker volumes to be created before starting server deployment. This is done first time.
<pre><code>
docker volume create --name=mender-artifacts
docker volume create --name=mender-db
docker volume create --name=mender-elasticsearch-db
docker volume create --name=mender-redis-db
</code></pre>


Package delivered will contain the YML files.

Follow the bellow steps to launch the server. 
<pre><code>
$cd smarthing-server
$cd production
$cd ./run start
</code></pre>

To stop
<pre><code>
$cd smarthing-server
$cd production
$cd ./run stop
</code></pre>

To upgrade, update the docker image tags in docker_compose.yml, then

<pre><code>
$cd smarthing-server
$cd production
$cd ./run up
</code></pre>

To Remove all the containers

<pre><code>
$cd smarthing-server
$cd production
$cd ./run rm
</code></pre>