## Docker Commands

- login and logout in docker registry:

  - [login](https://docs.docker.com/engine/reference/commandline/login/) in docker registry.

    - The command will ask for username and password to login.

    ```
    $ docker login
    # docker login [OPTIONS] [SERVER]
    ```

  - [logout](https://docs.docker.com/engine/reference/commandline/logout/) in docker registry.

    ```
    $ docker logout
    # docker logout [SERVER]
    ```

- [pull](https://docs.docker.com/engine/reference/commandline/pull/) docker image from registry.

  ```
  $ docker pull mongo
  #docker pull [OPTIONS] NAME[:TAG|@DIGEST]
  ```

- list of locally installed [images](https://docs.docker.com/engine/reference/commandline/images/)

  ```
  $ docker images
  #docker images [OPTIONS] [REPOSITORY[:TAG]]
  ```

- [run](https://docs.docker.com/engine/reference/run/) the docker image

  - If docker image is not installed locally, the run command will first pull and then run.

  - By default the latest version of image will executed.

    ```
    $ docker run mongo # running docker in foreground mode
    #docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
    ```

  - For using  specific version of image, use **:IMAGE_VERSION**.

    ```
    $ docker run mongo:4.0 # running docker in foreground mode
    #docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
    ```

  - run in [detach mode](https://docs.docker.com/engine/reference/run/) **(good practise)**

    - Start a docker container in detached mode with a `-d` option. So the container starts up and run in background. That means, you start up the container and could use the console after start-up for other commands.

      ```
      $ docker run -d mongo # output shown is "Container ID" while running in detached mode.
      #docker run -d IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
      ```

  - [name](https://docs.docker.com/engine/reference/run/) the container while running it (naming is helpful when you stop, start or perform similar operations on container)

    - **--name** is used  to define the name.

    ```
    $ docker run  -d -p6001:6379 --name mongo-older mongo:4.0
    ```

- Remove docker container and Image.

  - [Remove (rm)](https://docs.docker.com/engine/reference/commandline/rm/) docker container.

    ```
    $ docker rm CONTAINER_NAME
    #docker rm [OPTIONS] CONTAINER [CONTAINER...]
    ```

  - [Remove (rmi)](https://docs.docker.com/engine/reference/commandline/rmi/) docker image.

    ```
    $ docker rmi IMAGE_ID
    #docker rmi [OPTIONS] IMAGE [IMAGE...]
    ```

- Restart ([stop](https://docs.docker.com/engine/reference/commandline/stop/) and [start](https://docs.docker.com/engine/reference/commandline/start/)) container using container ID.

  ```
  $ docker stop CONTAINER_ID
  #docker stop [OPTIONS] CONTAINER [CONTAINER...]
  
  $ docker start CONTAINER_ID
  #docker start [OPTIONS] CONTAINER [CONTAINER...]docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
  ```

  - [kill](https://docs.docker.com/engine/reference/commandline/kill/) the docker container:

    ```
    $ docker kill CONTAINER_ID
    # docker kill [OPTIONS] CONTAINER [CONTAINER...]
    ```

  - stop VS kill (first preference should be stop)

    - stop: stop will send SIGTERM (terminate signal) to the process and docker will have 10 seconds to clean up like saving files or emitting some messages.
    - kill:  use kill , if container is locked up or if it is not responding. 

  - run VS start commands:

    - run : is used for creating and running a image into container.
    - start: is  used on container which is already created but not running.

- list of [running images](https://docs.docker.com/engine/reference/commandline/ps/) also called containers.

```
$ docker ps
#docker ps [OPTIONS]
```

- list of [Container history](https://docs.docker.com/engine/reference/commandline/ps/) (all running & stopped )

  ```
  $ docker ps -a
  #docker ps [OPTIONS]
  ```

- [Container Networking](https://docs.docker.com/config/containers/container-networking/) - Docker port binding (host and container ports) while running containers.

  - For instance - if you are ruining same image with different versions, that is possible and you can bind those images with different HOST ports.

    - Running redis updated version on host 6000 # port from container port # 6379:

      ```
      $ docker run -p6000:6379 -d redis
      #docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
      ```

    - Running another redis instance having version 4.0 on host 6001 # port from container port # 6379:

      ```
      $ docker run -p6001:6379 -d redis:4.0
      #docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
      ```

    - Now you can cross-check the port binding of redis containers:

      ```
      $ docker ps # look into the PORT column fo each conatiner details.
      ```

      

- Fetch the [logs](https://docs.docker.com/engine/reference/commandline/logs/) of container -using CONTAINER_NAME or CONATNER_ID

  - Get the logs of CONTAINER_NAME  , where name can be get from "docker ps"

  ```
  $ docker logs CONTAINER_NAME  
  #docker logs [OPTIONS] CONTAINER
  ```
  - Get the logs of CONTAINER_ID  , where ID can be get from "docker ps"

  ```
  $ docker logs CONTAINER_ID
  #docker logs [OPTIONS] CONTAINER
  ```

  - For instance to get the last 100 lines of log of container.

    ```
    $ docker logs --tail 100 CONTAINER_NAME
    #docker logs [OPTIONS] CONTAINER
    ```

- To [fetch the terminal](https://docs.docker.com/engine/reference/commandline/exec/) of running container.

  - **-it** : stands for interactive terminal and **/bin/bash** will get the bash for you.

    ```
    $ docker exec -it CONTAINER_ID /bin/bash
    #docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
    ```

    - Now you can use bash commands -navigate dirs, create dir, check logs, top etc.

  - Use **exit** command to get out of bash.

    ```
    root@CONATNIER_ID:/# exit
    ```

  - You may fetch the terminal using CONTAINER_NAME as well.

    ```
    $ docker exec -it CONTAINER_NAME /bin/bash
    #docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
    ```

- [Building image](https://docs.docker.com/engine/reference/commandline/build/) from Docker file.

  - **-t** is **tag** shorthand - below we have tag the docker build file with name and version (IMAGE_NAME:VERSION)

  - **.** denotes the path of current directory where IMAGE_NAME is present.

    ```
    $ docker build -t IMAGE_NAME:VERSION .
    #docker build [OPTIONS] PATH | URL | -
    ```

  - Remove cache before building docker image.

    ```
    $ docker build --no-cache -t IMAGE_NAME:VERSION .
    #docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
    ```

- [push](https://docs.docker.com/engine/reference/commandline/push/) image/repository to docker registry.

  - **TAG_NAME** can be version name.

  ```
  $ docker push IMAGE_NAME:TAG_NAME
  #docker push [OPTIONS] NAME[:TAG]
  ```

- [List (ls)](https://docs.docker.com/engine/reference/commandline/network_ls/) docker networks.

  ```
  $ docker network ls
  #docker network ls [OPTIONS]
  ```