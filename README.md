# Namaste-Docker-Fundamentals

#### To check docker version

`docker version`

###### To run hello-world image to check docker installation. After command is ran it will create container for "hello-world"

`docker run hello-world`

//"Namaste To Docker world" text will be overriden to the output of busybox container. Busybox have two programs - ls and echo.
// It means this can be run with other image because these program may not exist in other image
`docker run busybox echo Namaste To Docker world`   => Container starts and exit immediately
`docker run busybox ls`
`docker run busybox ping google.com` => Container starts and keeps running. Press ctrl+c to stop

// Now open another terminal to check whether container is running or not
`docker ps`

OUTPUT=> LIKE BELOW LINES
CONTAINER ID   IMAGE     COMMAND             CREATED          STATUS          PORTS     NAMES
2690a93873e5   busybox   "ping google.com"   47 seconds ago   Up 46 seconds             romantic_dewdney

docker ps --all => To see all the HISTORY of conatiner ran or running( All the container ever started up)
`docker ps --all`
OUTPUT =>
**`CONTAINER ID   IMAGE         COMMAND                  CREATED             STATUS                      PORTS     NAMES`**

```
2690a93873e5   busybox       "ping google.com"        3 minutes ago       Exited (0) 37 seconds ago             romantic_dewdney
1ed1b3c4c305   hello-world   "echo Namaste"           6 minutes ago       Created                               stoic_burnell
22c2a2f33ebd   hello-world   "ls"                     7 minutes ago       Created                               tender_ride
8471da313375   busybox       "ls"                     8 minutes ago       Exited (0) 8 minutes ago              happy_jennings
dbf5536e635e   busybox       "echo Namaste"           9 minutes ago       Exited (0) 9 minutes ago              hopeful_maxwell
1e21a33de1b5   busybox       "ls"                     9 minutes ago       Exited (0) 9 minutes ago              intelligent_grothendieck
121c976862b4   busybox       "echo Namaste To Doc…"   10 minutes ago      Exited (0) 10 minutes ago             xenodochial_visvesvaraya
50215c1c3358   hello-world   "/hello"                 18 minutes ago      Exited (0) 18 minutes ago             clever_maxwell
249a0ed248f6   hello-world   "/hello"                 20 minutes ago      Exited (0) 20 minutes ago             compassionate_hertz
5ea1746047f1   hello-world   "/hello"                 58 minutes ago      Exited (0) 58 minutes ago             festive_ishizaka
a4e362e9294f   hello-world   "/hello"                 About an hour ago   Exited (0) 17 minutes ago             distracted_mcnulty
```

###### Later anytime => container can be started with conatiner-id

`docker start -a 2690a93873e5`

###### Difference between => Docker run hello-world & Docker create hello-world

`docker run hello-world`

=> By default `docker run` command will run the container and show us the logs

`docker create hello-world`  and then `docker start -a <container-id>` OR  `docker start <container-id>`

=>  `-a` means attach to the container and watch for output comming from it and printed out to my terminal. But without `-a` , it won't watch for output and prints to terminal

`docker `=> Reference the Docker client

`create` => Try to CREATE the container

`start` => Try to START the container

`<image-name>` => Name of image to use for this container

`<container-id>` => ID of container to start

### Start the container(ever ran in past )

`docker start -a <container-id>`

`docker start -a 121c976862b4`

###### Removing all the stopped container ever ran ( to free up the space )

`docker system prune`

### Retriving Log Outputs

`docker logs <container-id>`

for example: -

```
docker create busybox ping google.comdocker 

start bc452b715415e677f4a7e0f58d0c31527fbee802de1278307753b39ed1da09b6

docker logs bc452b715415e677f4a7e0f58d0c31527fbee802de1278307753b39ed1da09b6

```

### Stopping the containers gracefully

`docker stop <container-id>`

Verify the container running status from below command

`docker ps --all`

### Stopping the containers without gracefully

`docker kill <container-id>`

Verify the container running status from below command

`docker ps --all`


##### Execute an additional command in a container

`docker exec -it` `<container-id> <command>`

`docker` => Reference to the docker client

`exec` => Run another command

`-it` => Allow us to provide input to the container

`<container-id>` => ID of the container

`<command>` => Command to execute

###### Test above command with Redis:

Find the container-id of the redis  `docker ps --all`

`docker exec -it 7eb487bb11f1 redis-cli`

`127.0.0.1:6379> set myValue 5`

```
output => OK
```

`127.0.0.1:6379> get myValue`

```
output => "5"
```

Finally run `ctrl+c` to stop Redis


## Getting a command prompt in a Running CONATINER

`docker exec -it <container-id> sh`

Now, you can run commands related to the running container

for example:

```
docker exec -it b661fbeb9ae4 sh
cd ~/ls
cd /ls
bin  boot  data  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr var
echo Namaste-Shell
Namaste-Shell
redis-cli
127.0.0.1:6379> set name "Raj"
OK
127.0.0.1:6379> get name
"Raj"
127.0.0.1:6379>
```

`sh` => shell command processors just like bash or zsh

Ctrl+C/exit  => to exit from shell command


##### Test shell command with busybox

`docker run -it busybox sh`

```
docker run -it busybox sh
/ # ls
bin    dev    etc    home   lib    lib64  proc   root   sys    tmp    usr    var
/ # ping google.com
PING google.com (142.250.194.78): 56 data bytes
64 bytes from 142.250.194.78: seq=0 ttl=63 time=59.180 ms
64 bytes from 142.250.194.78: seq=1 ttl=63 time=60.597 ms
64 bytes from 142.250.194.78: seq=2 ttl=63 time=61.102 ms
64 bytes from 142.250.194.78: seq=3 ttl=63 time=67.918 ms
64 bytes from 142.250.194.78: seq=4 ttl=63 time=60.160 ms
^C
--- google.com ping statistics ---
5 packets transmitted, 5 packets received, 0% packet loss
round-trip min/avg/max = 59.180/61.791/67.918 ms
/ # ^C/ # echo NamasteBusyBox
NamasteBusyBox
```

`CTRL+D` or Type exit to `exit `from shell

#### Container Islation:

Running same container more than once don't share instance/container space

for example

Run busybox twice in a two terminals

Create file in first container with touch command and then do ls to check the file has been created

Now check the 2nd busybox container by running ls command. You will not find the file. So container runs in an isolation mode

##### ############################### THE END OF COMMANDS ###############################


### Building Custom Images Through Docker Server
