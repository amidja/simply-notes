kubectl  annotate statefulset kafka-cluster-zookeeper strimzi.io/manual-rolling-update=true
kubectl annotate statefulset kafka-cluster-kafka strimzi.io/manual-rolling-update=true

kubectl rollout status sts/kafka-cluster-kafka
kubectl  wait kafka/kafka-cluster --for=condition=Ready --timeout=2h


# Annotating a StrimziPodSet


https://strimzi.io/docs/operators/latest/deploying#con-upgrade-cluster-str

Step 1)
1a) Make sure your Kubernes cluster version is supported

1b) For Kafka to stay operational, topics must also be replicated for high availability.

This requires topic configuration that specifies a replication factor of at least 3 and a minimum number of in-sync replicas to 1 less than the replication factor.

apiVersion: kafka.strimzi.io/v1beta2
kind: KafkaTopic
metadata:
  name: my-topic
  labels:
    strimzi.io/cluster: my-cluster
spec:
  partitions: 1
  replicas: 3
  config:
    # ...
    min.insync.replicas: 2
    # ...
    
    
    
    
    
   
Strimzi components
 Strimzi administrator roles (strimzi-admin) - todo
 Standalone Topic Operator (topic-operator)
 Standalone User Operator (user-operator)
 Strimzi Drain Cleaner (drain-cleaner) - todo


# - - - - - - - - - - - - - - - - - - - - - - - - - -- - 
Task: Insure topics are replicated for high availability.

Topic configuration specifies a replication factor of at least 3 and a minimum number of in-sync replicas to 1 less than the replication factor.

apiVersion: kafka.strimzi.io/v1beta2
kind: KafkaTopic
metadata:
  name: my-topic
  labels:
    strimzi.io/cluster: my-cluster
spec:
  partitions: 1
  replicas: 3
  config:
    # ...
    min.insync.replicas: 2
    # ...
# - - - - - - - - - - - - - - - - - - - - - - - - - -- - 
