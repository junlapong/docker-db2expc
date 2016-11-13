DB2 Express-C Docker
====================

from https://hub.docker.com/r/ibmcom/db2express-c/

```
docker pull ibmcom/db2express-c
```

## 1 - Start a container
```
docker run -it -p 50000:50000 -e DB2INST1_PASSWORD=db2inst1-pwd -e LICENSE=accept ibmcom/db2express-c:latest bash
```

## 2 - Start DB2 and create sample DB
Please switch to the default db2 instance user db2inst1 to start DB2 instance and create a sample database if you want :
```
$ su - db2inst1
$ db2start
$ db2sampl

### commit to new image ###
docker ps -a
docker commit CONTAINER_ID db2express-c:sampledb
docker run -it -p 50000:50000 db2express-c:sampledb bash
docker run -d -p 50000:50000 db2express-c:sampledb db2start
```
The time of creating a sample database depends on your system performance, which may take several minutes.
You can create another database using db2 create db <dbname> command.

## 3 - Note
1) Start as a daemon
You can start a container as a daemon with DB2 services up/running :
```
docker run -d -p 50000:50000 -e DB2INST1_PASSWORD=db2inst1-pwd -e LICENSE=accept ibmcom/db2express-c:latest db2start
```
db2start, db2 services start automatically and remote client can connect to it at port 50000

2) Mount a volume
While starting a Docker container, you can mount a volume from a directory on the Docker host like the following command :
```
docker run -it -p 50000:50000 -e DB2INST1_PASSWORD=db2inst1-pwd -e LICENSE=accept   -v  $(pwd):/share  ibmcom/db2express-c:latest bash
```

/share, referring to mount point at "/share" in the Docker.
$(pwd), the current directory on Docker host while running Docker command, which is mounted by Docker container. It can also be any existing directory on Docker host, like /tmp, /opt, etc.
If you want to use the mounted volume for DB2 system or data storage, please update DB2 manager configuration according to IBM Knowledge Center

3) DB2 is deployed in the Docker Engine in:

 /opt/ibm/db2/V10.5
