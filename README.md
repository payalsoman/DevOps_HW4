# DevOps_HW4 : Advanced Docker

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
##### GIF

![hw4payal_1](https://cloud.githubusercontent.com/assets/14048728/11356263/34feb38e-922a-11e5-9b7e-47956c073af7.gif)

### Ambassador Pattern:

Create two droplets Host1 and Host2.

Copy their respective docker-compose.yml files to the Host1 and Host2 folders.

Install docker compose on both the hosts:

```
curl -L https://github.com/docker/compose/releases/download/1.5.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

Host1 is the client and Host2 is the server.

For Host2, do ```docker-compose up```
This will create two containers on Host2:

1. redis
2. redis-ambassador-server

For Host1, do ```docker-compose run redis_cli```

This creates the folowwing containers on Host1:

1. redis-ambassador-client
2. redis_cli

This enables us to access the client cli and use the set and get requests with redis.

##### GIF

![hw4payal_2](https://cloud.githubusercontent.com/assets/14048728/11356264/379b651a-922a-11e5-80aa-1c55c5731395.gif)

### Docker Deploy
Follow all the steps of the Deployment workshop to set up the deploy directory structure.

Clone the app repo, and set the following remotes.

```
git remote add blue file://$ROOT/blue.git

git remote add green file://$ROOT/green.git
```

You can now push changes in the following manner.

```
git push green master

git push blue master
```

You may have to create a simple commit before pushing.

After push, go to your machine's green and blue ports to see the changes made.

##### GIF

![hw4payal_3](https://cloud.githubusercontent.com/assets/14048728/11356267/38dc4e26-922a-11e5-92ec-1767d992131f.gif)

