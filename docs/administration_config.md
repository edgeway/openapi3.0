# Configuration

After setting up the infrastructure and environment, now we have to configure. 

All the configurations go into "prod.yml" file under production directory. 


### Storage proxy
Locate the storage-proxy service and add s3.example.com (or your DNS name) under networks.mender.aliases key. The entry should look like this:

```shell
    ...
    storage-proxy:
        networks:
            mender:
                aliases:
                    - s3.example.com
    ...
```

### Minio

Locate the minio service. The keys MINIO_ACCESS_KEY and MINIO_SECRET_KEY control credentials for uploading artifacts into the object store. Since Minio is a S3 API compatible service, these settings correspond to Amazon's AWS Access Key ID and Secret Access Key respectively.

Set MINIO_ACCESS_KEY to mender-deployments. MINIO_SECRET_KEY should be set to a value that can not easily be guessed. We recommend using the pwgen utility for generating the secret. Run the following command to generate a 16-character long secret:

```
pwgen 16 1
```
> ahshagheeD1ooPae

The updated entry should look similar to this:
```
    ...
    minio:
        environment:
            # access key
            MINIO_ACCESS_KEY: mender-deployments
            # secret
            MINIO_SECRET_KEY: ahshagheeD1ooPaeT8lut0Shaezeipoo
    ...

```

### Deployments service

Locate the mender-deployments service. The deployments service will upload artifact objects to minio storage via storage-proxy. For this reason, access credentials DEPLOYMENTS_AWS_AUTH_KEY and DEPLOYMENTS_AWS_AUTH_SECRET need to be updated and DEPLOYMENTS_AWS_URI must point to s3.example.com (or your DNS name).

>Set DEPLOYMENTS_AWS_AUTH_KEY and DEPLOYMENTS_AWS_AUTH_SECRET to the values of MINIO_ACCESS_KEY and MINIO_SECRET_KEY respectively. Set DEPLOYMENTS_AWS_URI to point to https://s3.example.com:9000.

The updated entry should look like this:
```
    ...
    mender-deployments:
        ...
        environment:
            DEPLOYMENTS_AWS_AUTH_KEY: mender-deployments
            DEPLOYMENTS_AWS_AUTH_SECRET: ahshagheeD1ooPaeT8lut0Shaezeipoo
            DEPLOYMENTS_AWS_URI: https://s3.example.com:9000
    ...
```

### API gateway

Locate the mender-api-gateway service. For security purposes, the gateway must know precisely the DNS name allocated to it, which you'll configure via the ALLOWED_HOSTS environment variable.

Change the placeholder value my-gateway-dns-name to a valid hostname; assuming it's mender.example.com, the updated entry should look like this:

```
    ...
    mender-api-gateway:
        ...
        environment:
            ALLOWED_HOSTS: mender.example.com
    ...
```
> Note that if for some reason you need more than 1 DNS name pointing to the gateway, you can supply a whitespace-separated list of hostnames as follows:

```
    ...
    mender-api-gateway:
        ...
        environment:
            ALLOWED_HOSTS: mender1.example.com mender2.example.com
    ...
```

### Device Authentication Service

Locate the mender-device-auth service. Add the environment section if it is absent. In the environment section add the DEVICEAUTH_MAX_DEVICES_LIMIT_DEFAULT variable with an integer value. 0 represents no limit and is the default. The updated entry should look like this:

```
    ...
    mender-device-auth:
        ...
        environment:
            DEVICEAUTH_MAX_DEVICES_LIMIT_DEFAULT: 15
    ...
```

###  Logging

The setup uses Docker's default json-file logging driver, which exposes two important log rotation parameters: max-file and max-size. For selected services - consistently producing a large amount of logs - we override these settings to ensure that at any given time, logs capture about 1 week of a running system's operation.

The reference setup used for determining these parameters (max-file: 10, max-size: 50m) was 200 devices, polling the server every 30 minutes. The total log volume produced was 2.4GB, and this is the recommended amount of disk space to reserve for logging purposes alone.

>These parameters can however be adjusted accordingly to a particular deployment and workload. The deciding factors determining the log volume are:
* the number of devices accessing the system.
* polling frequency

 It is suggested to adjust log rotation parameters only after measuring the actual space usage for a given use case.


 ## Bandwidth: Artifact download and connection limiting

 > This section applies only to Mender with Minio storage backend.


 When a large number of devices is being updated, the uplink connection of the backend can easily get saturated. For this reason storage-proxy container is aware of 2 environment variables:

* MAX_CONNECTIONS - limits the number of concurrent GET requests. Integer, defaults to 100.

* DOWNLOAD_SPEED - limits the download speed of proxy response. String, defaults to 1m (1MByte/s). Detailed format description is provided in nginx documentation of limit_rate variable

These options can be adjusted using a separate compose file with the following entry (example limiting download speed to 512kB/s, max 5 concurrent transfers): 

```
    storage-proxy:
        environment:
            MAX_CONNECTIONS: 5
            DOWNLOAD_SPEED: 512k
```
