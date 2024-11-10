# Kafka


## Installation in MAC

  
🚩 Make sure brew is upto date
    
    brew update
        
🚩 Install Kafka using brew
  
    brew install kafka
🚩 Start Zookeeper server

    zookeeper-server-start /opt/homebrew/etc/kafka/zookeeper.properties

🚩 In new terminal start kafka server
    
    kafka-server-start /opt/homebrew/etc/kafka/server.properties

🚩 Create a Kafka topic

    kafka-topics --create --topic my-topic --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1

🚩 Verify the topic created

    kafka-topics --list --bootstrap-server localhost:9092

🚩 Send Messages as Producer

    kafka-console-producer --topic my-topic --bootstrap-server localhost:9092

🚩 Consume Messages as Consumer

    kafka-console-consumer --topic my-topic --bootstrap-server localhost:9092 --from-beginning

🚩 Stop kafka server

    kafka-server-stop
  
🚩 Stop Zookeeper server

    zookeeper-server-stop

