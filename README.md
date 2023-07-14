# 07.18 작업 스크립트

### A. EHUB SANDBOX 클러스터 
putty 로그 파일 

ll /opt/confluent/confluent-7.3.2

##### 1. ZOOKEEPER 전환 
###### ZOOKEEPER 1

echo mntr | nc localhost 2181 | grep zk_server_state
echo mntr | nc localhost 2181 | grep zk_synced_followers

CM/ ZOOKEEPER 종료 
ps -ef | grep zoo.cfg 

cp -r /appData/zookeeper /appData/zookeeper_backup

systemctl start confluent-zookeeper
systemctl status confluent-zookeeper

###### ZOOKEEPER 2

echo mntr | nc localhost 2181 | grep zk_server_state
echo mntr | nc localhost 2181 | grep zk_synced_followers

CM/ ZOOKEEPER 종료 
ps -ef | grep zoo.cfg 

cp -r /appData/zookeeper /appData/zookeeper_backup

systemctl start confluent-zookeeper
systemctl status confluent-zookeeper

###### ZOOKEEPER LEADER 

echo mntr | nc localhost 2181 | grep zk_server_state
echo mntr | nc localhost 2181 | grep zk_synced_followers

CM/ ZOOKEEPER 종료 
ps -ef | grep zoo.cfg 

echo mntr | nc localhost 2181 | grep zk_server_state

cp -r /appData/zookeeper /appData/zookeeper_backup

systemctl start confluent-zookeeper
systemctl status confluent-zookeeper

echo mntr | nc localhost 2181 | grep zk_server_state
echo mntr | nc localhost 2181 | grep zk_synced_followers

##### 2. BROKER 전환 
###### BROKER 1

/opt/confluent/confluent-7.3.2/bin/kafka-topics --bootstrap-server [BROKER_HOST_NAME]:9092 --describe --under-replicated-partitions 
로그 확인
/opt/confluent/confluent-7.3.2/bin/zookeeper-shell localhost:2181  get /kafka/controller

CM/ BROKER 종료 

systemctl start confluent-server
systemctl status confluent-server
/opt/confluent/confluent-7.3.2/bin/kafka-topics --bootstrap-server [BROKER_HOST_NAME]:9092 --describe --under-replicated-partitions 
grep -E "ERROR|Exception" /data/log/confluent-kafka/server.log

###### BROKER 2

/opt/confluent/confluent-7.3.2/bin/kafka-topics --bootstrap-server [BROKER_HOST_NAME]:9092 --describe --under-replicated-partitions 
로그 확인
/opt/confluent/confluent-7.3.2/bin/zookeeper-shell localhost:2181  get /kafka/controller

CM/ BROKER 종료 

systemctl start confluent-server
systemctl status confluent-server
/opt/confluent/confluent-7.3.2/bin/kafka-topics --bootstrap-server [BROKER_HOST_NAME]:9092 --describe --under-replicated-partitions 
grep -E "ERROR|Exception" /data/log/confluent-kafka/server.log

###### BROKER CONTROLLER 

/opt/confluent/confluent-7.3.2/bin/kafka-topics --bootstrap-server [BROKER_HOST_NAME]:9092 --describe --under-replicated-partitions 
로그 확인

CM/ BROKER 종료 

systemctl start confluent-server
systemctl status confluent-server
/opt/confluent/confluent-7.3.2/bin/kafka-topics --bootstrap-server [BROKER_HOST_NAME]:9092 --describe --under-replicated-partitions 
grep -E "ERROR|Exception" /data/log/confluent-kafka/server.log

/opt/confluent/confluent-7.3.2/bin/zookeeper-shell localhost:2181  get /kafka/controller
