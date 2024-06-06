# oracleDb-ngrok

1. Clone repository https://github.com/oracle/docker-images
2. Download oracle database as per you want https://www.oracle.com/database/technologies/oracle-database-software-downloads.html#db_free
3. Put the zip file from step 2 into  dockerfiles/19.3.0 folder.
4. Build docker image by using command  ./buildContainerImage.sh -v 19.3.0 -t oracle-db:0.1 -e     
  



docker run -d -it --name oracledb -p 1521:1521 -p 5500:5500 \
  -e ORACLE_PDB=TESTDB -e ORACLE_PWD=zxcv1234 \
  -v /Users/greatkid/Nokia3310/oracleDB/docker-images/OracleDatabase/SingleInstance/dockerfiles/persistent:/opt/in/container oracle-db:0.1 
