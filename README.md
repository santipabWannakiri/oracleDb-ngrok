# oracleDb-ngrok
## Introduction
## Building Oracle Database container images
1. Clone repository
```git
git clone https://github.com/oracle/docker-images
```
2. Download oracle database version as per you want
```git
https://www.oracle.com/database/technologies/oracle-database-software-downloads.html#db_free
```
3. Put the zip file `LINUX.xxxx_xxxxxx_db_home.zip` from step 2 into `dockerfiles/19.3.0` folder.
   
6. Build docker image by using command
```sh
   ./buildContainerImage.sh -v 19.3.0 -t oracle-db:0.1 -e     
```
7. After the installation done, Check your images with command
```sh
  docker images -a   
```
8. You can test to start the oracle database container as the following command
 ```sh
docker run -d -it --name oracledb -p 1521:1521 -p 5500:5500 \
  -e ORACLE_PDB=TESTDB -e ORACLE_PWD=zxcv1234 \
  -v ./persistent:/opt/in/container oracle-db:0.1 
 ``` 
## Building Ngrok container images
To build Ngrok as a container and configuration the `ngrok.yml`  --> [Ngrok config](https://github.com/santipabWannakiri/spring-boot-container-cicd?tab=readme-ov-file#ngrok)\
However, to connect to an Oracle database, it must be done via TCP protocal.\
Therefore, we must modify the ngrok.yml file to enable TCP at the following.
 ```yml
version: '2'
authtoken: {Your token}
log_level: debug
log_format: logfmt
log: /var/log/ngrok.log
connect_timeout: 30s
tunnels:
  oracle-db:
    addr: host.docker.internal:1521
    proto: tcp
 ```
> Ngrok is going to random generate a host and port for accessing the database.
> If you desire fixed addresses, you may need to consider an upgrade plan at the Ngrok dashboard.\
> To verify the current host and port, navigate to `http://localhost:4040/inspect/http`

## Building Docker Compose
In this step, we will ramp up all the containers using Docker Compose at the following
 ```yml
version: '4.0'
services:
  oracledb:
    image: oracle-db:0.1
    container_name: oracledb
    networks:
      - docker-daemon
    environment:
      - ORACLE_PDB={database_name}
      - ORACLE_PWD={password_your_db}
    ports:
      - "1521:1521"
      - "5500:5500"
    volumes:
      - ./persistent:/opt/in/container

  ngrok:
    image: ngrok/ngrok:latest
    container_name: ngrok
    restart: unless-stopped
    command:
      - "start"
      - "--all"
      - "--config"
      - "/etc/ngrok.yml"
    volumes:
      - ./ngrok_home/ngrok.yml:/etc/ngrok.yml
      - ./ngrok_home/log/ngrok.log:/var/log/ngrok.log
    ports:
      - 4040:4040
    depends_on:
      - oracledb

networks:
  docker-daemon:
    driver: bridge
 ```

When we are done creating a Docker Compose file to start the container, use the following command: 
```sh
   docker compose -f ./{docker_compose_file} up -d    
```

To check status of container 
```sh
  docker ps -a
```
