## Part 1. Ready docker



##### Take the official docker image from **nginx** and download it using `docker pull`

![s](assets/images/part1_01.png)

##### Check for the presence of a docker image via `docker images`

![s](assets/images/part1_02.png)

##### Run the docker image via `docker run -d [image_id|repository]`

![s](assets/images/part1_03.png)

##### Check that the image has started via `docker ps`

![s](assets/images/part1_04.png)

##### View container information via `docker inspect [container_id|container_name]`

![s](assets/images/part1_05.png)

##### Based on the output of the command, determine and place in the report the size of the container, a list of mapped ports and the IP of the container

![s](assets/images/part1_06.png)
![s](assets/images/part1_07.png)
![s](assets/images/part1_08.png)

##### Stop the docker image using `docker stop [container_id|container_name]`

![s](assets/images/part1_09.png)

##### Check that the image has stopped using `docker ps`

![s](assets/images/part1_10.png)

##### Run docker with ports 80 and 443 in the container, mapped to the same ports on the local machine, using the *run* command

![s](assets/images/part1_11.png)
![s](assets/images/part1_12.png)

##### Check that the **nginx** start page is available in the browser at *localhost:80*

![s](assets/images/part1_13.png)

##### Restart the docker container using `docker restart [container_id|container_name]`

![s](assets/images/part1_14.png)

##### Check in any way that the container has started

![s](assets/images/part1_15.png)

## Part 2. Container operations



##### Read the *nginx.conf* configuration file inside the docker container using the *exec* command

![s](assets/images/part2_01.png)

##### Create a *nginx.conf* file on the local machine
##### Configure it along the path */status* to return the server status page **nginx**

![s](assets/images/part2_02.png)

##### Copy the created *nginx.conf* file inside the docker image using the `docker cp` command

![s](assets/images/part2_03.png)

##### Restart **nginx** inside the docker image using the *exec* command

![s](assets/images/part2_04.png)

##### Check that the page with the status of the **nginx** server is sent to the address *localhost:80/status*

![s](assets/images/part2_05.png)

##### Export the container to the *container.tar* file using the *export* command

![s](assets/images/part2_06.png)

##### Stop container

![s](assets/images/part2_07.png)


##### Delete an image via `docker rmi [image_id|repository]` without deleting the containers first

![s](assets/images/part2_08.png)

##### Delete a stopped container

![s](assets/images/part2_09.png)

##### Import the container back using the *import* command

![s](assets/images/part2_10.png)

##### Launch imported container

![s](assets/images/part2_11.png)

##### Check that the page with the status of the **nginx** server is sent to the address *localhost:80/status*

![s](assets/images/part2_12.png)

## Part 3. Mini web server

##### Write a mini server in **C** and **FastCgi**, which will return a simple page with the inscription `Hello World!`
![s](assets/images/part3_01.png)

##### Write your own *nginx.conf*, which will proxy all requests from port 81 to *127.0.0.1:8080*
![s](assets/images/part3_02.png)

Launching the container
> docker run -d -p 81:81 nginx

Copy nginx.conf and main.c to the container
> docker cp nginx/nginx.conf flamboyant_feynman:/etc/nginx

> docker cp main.c flamboyant_feynman:/home

![s](assets/images/part3_03.png)

Connecting to the container console
> docker exec -it [image_id|repository] bash

To exit the console use
> exit

Update the image and install gcc, spawn-fcig and libfcgi-dev
> apt-get update && install -y gcc spawn-fcgi libfcgi-dev

##### Launch the written mini server via *spawn-fcgi* on port 8080
![s](assets/images/part3_04.png)
##### Check that the browser is serving the page you wrote on *localhost:81*
![s](assets/images/part3_05.png)

## Part 4. Your own docker

#### Write your own docker image that:
##### 1) collects the sources of the mini server on FastCgi from [Part 3](#part-3-mini-web server)
##### 2) runs it on port 8080
##### 3) copies the written *./nginx/nginx.conf* inside the image
##### 4) launches **nginx**.

![s](assets/images/part3_06.png)

Copy files to our container

![s](assets/images/part3_07.png)

##### Build a written Docker image via `docker build`, specifying the name and tag

![s](assets/images/part3_08.png)

##### Check using `docker images` that everything was assembled correctly

![s](assets/images/part3_09.png)

##### Run the assembled docker image with mapping 81 ports to 80 on the local machine and mapping the *./nginx* folder inside the container at the address where the **nginx** configuration files are located (see [Part 2]( #part-2-operations-with-container))

![s](assets/images/part3_10.png)

##### Check that the page of the written mini server is available at localhost:80

![s](assets/images/part3_11.png)

##### Add to *./nginx/nginx.conf* the proxying of the page */status*, which should be used to give the server status to **nginx**

![s](assets/images/part3_12.png)

##### Restart docker image

![s](assets/images/part3_13.png)

##### Check that now *localhost:80/status* is serving a page with the status **nginx**

![s](assets/images/part3_14.png)



## Part 5. **Dockle**

##### Scan an image from a previous job via `dockle [image_id|repository]`

If there is no dockle
> brew install goodwithtech/r/dockle

![s](assets/images/part3_15.png)

##### Fix the image so that there are no errors or warnings when checking through **dockle**

![s](assets/images/part5_01.png)
![s](assets/images/part5_02.png)
![s](assets/images/part5_03.png)

## Part 6. Basic **Docker Compose**

##### Write a file *docker-compose.yml*, using which:
##### 1) Raise the docker container from [Part 5](#part-5-tool-dockle) _(it should work on the local network, i.e. you don’t need to use the **EXPOSE** instruction and map ports to the local machine)_
##### 2) Raise a docker container with **nginx**, which will proxy all requests from port 8080 to port 81 of the first container
##### Map port 8080 of the second container to port 80 of the local machine

![s](assets/images/part6_01.png)
![s](assets/images/part6_02.png)

##### Stop all running containers

> docker stop [image_id|repository]

![s](assets/images/part6_04.png)

##### Build and run the project using the `docker-compose build` and `docker-compose up` commands

![s](assets/images/part6_05.png)
![s](assets/images/part6_06.png)

##### Check that the browser serves the page you wrote via *localhost:80*, as before

![s](assets/images/part3_11.png)