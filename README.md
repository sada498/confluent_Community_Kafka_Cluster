# Installing kafka cluster
## Setting up own three broker kafaka cluster using free confluent Community on *Ubuntu 20.04 Focal Fossa LTS.*

> Note: All three server should be more then 6GB Ram.

### 1.Install the Confluent public key. This key is used to sign the packages in the APT repository.
    wget -qO - https://packages.confluent.io/deb/6.0/archive.key | sudo apt-key add -
### 2. Add the repository
    sudo add-apt-repository "deb [arch=amd64] https://packages.confluent.io/deb/6.0 stable main"
### 3. Update apt-get and install the entire Confluent community platform.
    sudo apt-get update && sudo apt-get install confluent-community-2.13

### 4. Configure the **hosts** file on all three nodes:
    sudo vi /etc/hosts

### 5. Add all three private IP address to **hosts** file
    <Master Private IP> Master
    <Node1 Private Ip> Node1
    <Node2 Private Ip> Node2
### 6. save and exit from all three servers
### 7. edit the Zookeeper config file on all three servers
    sudo vi /etc/kafka/zookeeper.properties
### 8. Remove the contents in the config file and add the below code.
    tickTime=2000
    dataDir=/var/lib/zookeeper/
    clientPort=2181
    initLimit=5
    syncLimit=2
    server.1=Master:2888:3888
    server.2=Node1:2888:3888
    server.3=Node2:2888:3888
    autopurge.snapRetainCount=3
    autopurge.purgeInterval=24
### 9. save and exit from all three servers
### 10. add Zookeeper ID for all servers
     sudo vi /var/lib/zookeeper/myid   
> Note : After open that file just asigin Id.

    on Master server 1, enter 1 
    On Node1 server 2, enter 2
    On node2 server 3, enter 3

> Note: if you have probleam with saving file in Vim use below command 

    cd /var/lib
    sudo mkdir zookeeper
    cd zookeeper
    sudo vi myid
     Asign Id for zookeeper server 

     :w !sudo tee %
### save and exit from vim
# Kafka configuration
### 1. Edit the config fie on all three servers.
    sudo vi /etc/kafka/server.properties

### 2. we need edit the 3 areas in config file
       1. broker.id
       2. advertised.listeners
       3. zookeeper.connect
    

#### Master Server 
    broker.id=1
    advertised.listeners=PlaInText://Master:9092
    zookeeper.connect=Master:2181
#### Node1 server
    broker.id=2
    advertised.listeners=PlaInText://Node1:9092
    zookeeper.connect=Master:2181
#### Node2 server
    broker.id=3
    advertised.listeners=PlaInText://Node2:9092
    zookeeper.connect=Master:2181
# Start Kafka Confluent Platform

### 1.Start and enable the zookeeper srvices in all Three nodes.
    sudo systemctl start confluent-zookeeper

    sudo systemctl enable confluent-zookeeper
### 2. Start and enable the Kafka srvices in all Three nodes.
    sudo systemctl start confluent-kafka

    sudo systemctl enable confluent-kafka
### 3. server on each should be **active (running)** on all three nodes
    sudo systemctl status confluent*
### 4. check on each server 
    kafka-topics --list --bootstrap-server localhost:9092

result should be
    
    __confluent.support.metrics



 
 