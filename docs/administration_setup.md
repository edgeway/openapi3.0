# Setting up the fleet

What we need apart form system requirements?

* A Public IP assigned and port 443 and 9000 opened for public access.
* Allocated DNS names for api gateway and storage proxy.
* SSL Certificates for api gateway.
* SSL Certificates for storage proxy.
* Docker volumes
  * persistant storage for artefacts
  * MongoDb
  

## Certificates and Keys

"keygen" tool is part of integration package, this tool will generate the required certificates.
```shell
API_GATEWAY_DOMAIN_NAME="mender.example.com"  # NB! replace with your server's public domain name
STORAGE_PROXY_DOMAIN_NAME="$API_GATEWAY_DOMAIN_NAME"  # change if you are hosting the Storage Proxy on a different domain name (not the default)
```

Now, generate the keys.

```shell 
CERT_API_CN=$API_GATEWAY_DOMAIN_NAME CERT_STORAGE_CN=$STORAGE_PROXY_DOMAIN_NAME ../keygen
```

Output will be keys-generated folder with below contents.

```shell
├── keys-generated
│   ├── certs
│   │   ├── api-gateway
│   │   │   ├── cert.crt
│   │   │   └── private.key
│   │   └── server.crt
│   │   └── storage-proxy
│   │       ├── cert.crt
│   │       └── private.key
│   └── keys
│       ├── deviceauth
│       │   └── private.key
│       └── useradm
│           └── private.key
├── prod.yml
└── run
```
> Use the same directory structure as this is referred in YML files. 

## Persistent storage

Persistent storage of backend services's data is implemented using the docker volumes. "integration" packge has the configuration to mount to below docker volumes.

**mender-artifacts** - artifact objects storage.

**mender-db** - mender services databases data

**mender-elasticsearch-db** - elasticsearch service database data

**mender-redis-db** - redis service database data

Creat the docker volumes with below command

```shell
docker volume create --name=mender-artifacts
docker volume create --name=mender-db
docker volume create --name=mender-elasticsearch-db
docker volume create --name=mender-redis-db
```

