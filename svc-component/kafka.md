# Kafka

## kafka 使用
```bash
# 查看kafka topic
bin/kafka-topics.sh --command-config config/sasl.properties --bootstrap-server 127.0.0.1:9092 --list
# 查看某个topic的详情
bin/kafka-topics.sh --command-config config/sasl.properties --bootstrap-server 127.0.0.1:9092 --describe --topic demo
# 查看某个topic的最新offset（不支持sasl）
bin/kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list 127.0.0.1:9092 --topic demo --time -1
# 查看某个topic的最旧offset（不支持sasl）
bin/kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list 127.0.0.1:9092 --topic demo --time -2
# 创建producer
bin/kafka-console-producer.sh --producer.config config/sasl.properties --broker-list 127.0.0.1:9092 --topic demo
# 创建consumer
bin/kafka-console-consumer.sh --consumer.config config/sasl.properties --bootstrap-server 127.0.0.1:9092 --topic demo --partition 0 --offset 0 --max-messages 1
# 创建group
bin/kafka-console-consumer.sh --consumer.config config/sasl.properties --bootstrap-server 127.0.0.1:9092 --topic demo --consumer-property group.id=demo
# 查看所有的group
bin/kafka-consumer-groups.sh --command-config config/sasl.properties --bootstrap-server 127.0.0.1:9092 --list
# 查看某个group的offset
bin/kafka-consumer-groups.sh --command-config config/sasl.properties --bootstrap-server 127.0.0.1:9092 --describe --group demo
```

## kafka 配置
```bash
bin/kafka-configs.sh --zookeeper 127.0.0.1:2181/kafka --entity-type topics --entity-name demo --describe
bin/kafka-configs.sh --zookeeper 127.0.0.1:2181/kafka --entity-type topics --entity-name demo --alter --add-config retention.ms=2592000000
```