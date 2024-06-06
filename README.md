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
