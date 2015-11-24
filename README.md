# DevOps_HW4 : Advanced Docker

Initial Setup:

### 1. File IO:

Create a droplet on digital ocean.

Connect to the droplet using follwing command:
```
ssh root@<ip_address of droplet>
```

Copy the Dockerfile from your local directory to the droplet.

Install docker using following command:
```
curl -sSL https://get.docker.com/ | sh
```

Create a container using the following command which uses the Dockerfile created in the repository.
```
sudo docker build -t task1 .
```

The following script uses socat to map file access to read file container and expose over port 9001:
```
docker run -d --name container_server file-io socat tcp-l:9001,reuseaddr,fork system:'cat output.txt',nofork
```

The following script creates a linked container to the server container created above.
```
sudo docker run -td --link container_server:server --name container_client task1
```

Then we execute this linked client container and install curl. Using the curl command, the client container fetches the file output.txt from the server container at port 9001.
```
sudo docker exec -it container_client bash
sudo apt-get install curl
curl server:9001 
```

### Ambassador Pattern:

Create two droplets Host1 and Host2.

Copy their respective docker-compose.yml files to the Host1 and Host2 folders.

Install docker compose on both the hosts:
```
curl -L https://github.com/docker/compose/releases/download/1.5.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

Host1 is the server and Host2 is the client.

For Host1, do ```docker-compose up```
This will create two containers on Host1:
1. redis
2. redis-ambassador-server

For Host2 as well, do ```docker-compose up```
This creates the folowwing container on Host2:
1. redis-ambassador-client

Run the following command to create a new container which is linked to the redis-ambassador-client.
```
sudo docker run -it --link redis_ambassador_client:redis --name client_cli relateiq/redis-cli
```
This enables us to access the client-cli and use the set and get requests with redis.

