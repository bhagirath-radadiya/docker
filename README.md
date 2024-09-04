###### To check version of docker
```
docker -v
docker --version
```

###### To check service status
```
service docker status
```

###### To view docker image list
```
docker images
```

**NOTE : image_repo == image name**

###### To delete docker image
**NOTE: you can not delete image if that image's container is exist.**
```
docker rmi <image_repo> OR docker rmi <image_id> OR docker rmi <image_repo_1> <image_repo_2> ... OR docker rmi <image_id_1> <image_id_2> ...
```

###### To search image on docker hub through terminal
```
docker search <image_repo>
you can search image from docker hub in browser
```

###### To download image from docker hub
```
docker pull <image_repo>
```

###### To create and run container both at a time using existing image
```
docker run -it --name <container_name_which_you_want> <image_repo> /bin/bash    # image mathi container bani ne user ne direct container ni andar lai jase
docker run -idt --name <container_name_which_you_want> <image_name>             # image mathi container bani be background ma run rese. pachi jyare in thavu hoy tyare thai javase.
```


###### To view existing container (current running only)
```
docker ps OR docker container ls
```

###### To view all existing container
```
docker ps -a OR container ls -a
```

###### To start container
```
docker start <container_name> OR docker start <container_name_1> <conteainer_name_2> ...
```

###### To stop container
```
docker stop <container_name> OR docker stop <container_name_1> <conteainer_name_2> ...
```

###### To go inside container
```
docker attach <container_name>
# ekvar container ma aavi gaya pachi Ctrl + P, then Ctrl + Q thi container bandh karya vina bar jai sakay che.
```

###### To delete container
```
docker rm <container_name> OR docker rm <container_name_1> <conteainer_name_2> ...
```

###### To check diffrence or changes from base image to current image
```
docker diff <container_name>
```

###### To create image of updated container
```
docker commit <container_name_whose_image_you_want> <new_image_repo>
```

###### To stop all containers
```
docker stop $(docker ps -aq)
```

###### To remove all containers
```
docker rm -f $(docker ps -aq)
```

###### To remove all images
```
docker rmi -f $(docker images -aq)
```


###### To create volume in container
```
docker run -it --name <container_name> -v <local_system_path>:<container_path> ubuntu /bin/bash
```
**NOTE : ek bar container without volume ban gaya to volume add nahi ho sakta bad me. is problem ko solve karne ke liye kuch ways hai. 
(1) current container ki image banake new created images se vapas container banaye with volume. 
(2) agar current container jo state pe hai vaha pe volume ki khas jaroorat ho to...
docker cp <container_name>:<container_path> <local_system_path>
command se pahele local se data copy kar le then operation karke docker image banale then vapas naya container banaye with volume.**

###### To share volume with another container (container to container)
```
docker run -it --name <new_container_name> --privileged=true --volumes-from <old_container_name_where_volume_exist> ubuntu /bin/bash
```

**NOTE: once you create image of that container which have volume and the convert that image to container then you get only volum directory only not volume data.**

###### To share voume with host system (container to host)
```
docker run -it --name <container_name> --privileged=true -v <local_system_path>:<container_path> ubuntu /bin/bash
local_system_path : /home/bs/doc_vol
container_path : /doc_vol
```

###### To view all volumes
```
docker volume ls
```

###### To create new volume
```
docker volume create <volume_name>
```

###### To delete existing volume
```
docker volume rm <volume_name>
```

###### To remove all unused docker volume
```
docker volume prune
```

###### To inspect volume
```
docker volume inspect <volume_name>
```

###### To inspect docker container
```
docker container inspect <container_name>
```

###### To create network or create port
```
docker run -it --name <container_name> -p <host_port>:<container_port> <image_name> /bin/bash
```

###### To view all network
```
docker network ls
```

###### To inspect network
```
docker network inspect <network_name>
```

###### To create structure using docker compose file
**NOTE : go to docker compose yaml file directory OR use -f for specific docker yaml file path**
```
docker-compose up	# if your yml file in current dir
docker-compose -f <file_path> up	# if yml file is not in current dir
```

###### To delete structure using docker compose file
```
docker-compose down 	# in this command volume not delete
docker-compose down --volume	# using this command we can remove volume as well
```

###### To create/start/stop/remove/restart docker container using docker compose
```
docker-compose create/start/stop/rm/restart
```
**NOTE : this is only work on container like create, start, stop, and remove container. not work on network.**

###### To pause and unpause docker container using docker compose
```
docker-compose pause/unpause
```

###### To view logs of container
```
docker logs <container_name>
docker-compose logs <container_name>
```

###### To scale docker container
```
docker-compose --scale <service_name>=<number_of_instence>
```




---
## DOCKER FILE

NOTE : Docker file name must be "Dockerfile"

Docker file parts like FROM, RUN, MAINTAINER, COPY, ADD, EXPOSE, WORKDIR, CMD, ENTRYPOINT, ENV, ARG

FROM -> for base image. this command must be on top of the docker file.
RUN -> To execute commands. It will create alayer in image. (work when container create from image)
MAINTAINER -> AUthor/Owner/Description
COPY -> Copy file from local system(docker vm) we need to provide source, destination (we can not download file from internet and any remote repo).
ADD -> Similar to COPY but, It provides feature to download file from internet, also we extract file at docker image side.
EXPOSE -> To expose ports such as port 8080 for tomcat, port 80 for nginx etc.
WORKDIR -> To set working dicrectory for a container.
CMD -> Excute commands but diuring container creation.
ENTRYPOINT -> Similar to CMD, But has higher priority over CMD, first commands will be executed by ENTRYPOINT only.
ENV -> Environment variables.

HOW TO CREATE CONTAINER FROM DOCKERFILE ?
1	create file with name "Dockerfile"
2	add instruction in dockerfile
3 	Build dockerfile to create image
4 	Run image to create container

Sample docker file
file name : Dockerfile
'''
FROM ubuntu
RUN echo "this is testing od docker file" > /tmp/testing.txt
'''

To create image from docker file
docker build -t <image_repo> <docker_file_directory_path>

To create image and up container using docker compose file
docker-compose -f <docker compose file> up -d --build


<table class="table table-bordered">
<tbody><tr>
<th class="ts">Command</th>
<th class="ts">Use Case</th>
<th class="ts">Example</th>
</tr>
<tr>
<td><b>docker run</b></td>
<td>Create and start a new container</td>
<td>docker run -it ubuntu bash</td>
</tr>
<tr>
<td><b>docker ps</b></td>
<td>List running containers</td>
<td>docker ps</td>
</tr>
<tr>
<td><b>docker ps -a</b></td>
<td>List all containers (including stopped ones)</td>
<td>docker ps -a</td>
</tr>
<tr>
<td><b>docker start</b></td>
<td>Start a stopped container</td>
<td>docker start my_container</td>
</tr>
<tr>
<td><b>docker stop</b></td>
<td>Stop a running container</td>
<td>docker stop my_container</td>
</tr>
<tr>
<td><b>docker restart</b></td>
<td>Restart a container</td>
<td>docker restart my_container</td>
</tr>
<tr>
<td><b>docker rm</b></td>
<td>Remove a stopped container</td>
<td>docker rm my_container</td>
</tr>
<tr>
<td><b>docker rmi</b></td>
<td>Remove an image</td>
<td>docker rmi my_image</td>
</tr>
<tr>
<td><b>docker images</b></td>
<td>List all Docker images</td>
<td>docker images</td>
</tr>
<tr>
<td><b>docker pull</b></td>
<td>Download an image from a registry</td>
<td>docker pull nginx</td>
</tr>
<tr>
<td><b>docker build</b></td>
<td>Build an image from a Dockerfile</td>
<td>docker build -t my_image </td>
</tr>
<tr>
<td><b>docker exec</b></td>
<td>Run a command in a running container</td>
<td>docker exec -it my_container bash</td>
</tr>
<tr>
<td><b>docker logs</b></td>
<td>View the logs of a container</td>
<td>docker logs my_container</td>
</tr>
<tr>
<td><b>docker cp</b></td>
<td>Copy files/folders between a container and the local filesystem</td>
<td>docker cp my_container:/path/to/file /local/path</td>
</tr>
<tr>
<td><b>docker inspect</b></td>
<td>Display detailed information on a container or image</td>
<td>docker inspect my_container</td>
</tr>
<tr>
<td><b>docker network ls</b></td>
<td>List all Docker networks</td>
<td>docker network ls</td>
</tr>
<tr>
<td><b>docker network create</b></td>
<td>Create a new Docker network</td>
<td>docker network create my_network</td>
</tr>
<tr>
<td><b>docker-compose up</b></td>
<td>Create and start containers using a docker-compose.yml file</td>
<td>docker-compose up</td>
</tr>
<tr>
<td><b>docker-compose down</b></td>
<td>Stop and remove containers, networks, images, and volumes</td>
<td>docker-compose down</td>
</tr>
<tr>
<td><b>docker volume ls</b></td>
<td>List all Docker volumes</td>
<td>docker volume ls</td>
</tr>
<tr>
<td><b>docker volume create</b></td>
<td>Create a new Docker volume</td>
<td>docker volume create my_volume</td>
</tr>
<tr>
<td><b>docker info</b></td>
<td>Display system-wide information</td>
<td>docker info</td>
</tr>
<tr>
<td><b>docker stats</b></td>
<td>Display a live stream of container resource usage statistics</td>
<td>docker stats</td>
</tr>
<tr>
<td><b>docker attach</b></td>
<td>Attach local standard input, output, and error streams to a container</td>
<td>docker attach my_container</td>
</tr>
</tbody></table>
