# oracleDb-ngrok

 ./buildContainerImage.sh -v 19.3.0 -t oracle-db:0.1 -e     



docker run -d -it --name oracledb -p 1521:1521 -p 5500:5500 \
  -e ORACLE_PDB=TESTDB -e ORACLE_PWD=zxcv1234 \
  -v /Users/greatkid/Nokia3310/oracleDB/docker-images/OracleDatabase/SingleInstance/dockerfiles/persistent:/opt/in/container oracle-db:0.1 
