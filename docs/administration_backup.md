# Backup and restore

## Docker volumes

Docker volumes can be backed up using regular docker commands, such as docker run or docker cp.

A simple example displaying a method of backing up the contents of the mender-artifacts volume is provided below. The command will start a temporary container using the alpine:latest image, mount the mender-artifacts volume under the /from path and $PWD under /to. Then all files and directories under /from are passed through the tar | gzip pipeline and saved to /to/artifacts-backup.tar.gz. Once the container exits, the file $PWD/artifacts-backup.tar.gz can be verified to have correct contents.

```
docker run -it --rm -v mender-artifacts:/from -v $PWD:/to -w /from \
    alpine \
    ash -c 'tar c * | gzip -9 > /to/artifacts-backup.tar.gz'
```

This method can be applied to all volumes used by the  stack.

## Database

While it is possible to perform a backup of the databases used by Mender services with the volume based method outlined in volumes section, one can also use DB specific tools to dump and restore database contents.

The Mender integration repository provides tools for dumping and restoring all databases.

The tool dump-db will run a mongo container inside the mender network and dump the contents of each DB into $PWD/db-dump/<service-name> directory.

```
../migration/dump-db
```

The tool restore-db will run a mongo container inside the mender network to restore DB dumps previously created with dump-db.

```
../migration/restore-db
```

> Note restore-db and dump-db will automatically stop all Mender services that may access respective DBs.

Once the data has been dumped or restored, the services can be started using

```
./run up -d
```

Occasionally services may get new IP addresses and the API gateway DNS cache may no longer be correct. To refresh the API gateway's cache, run the following command:

```
./run exec mender-api-gateway kill -HUP 1
```