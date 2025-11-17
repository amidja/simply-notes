## Administrative Notes

How to restart Kafka strimzipodset:

```bash

oc get strimzipodset
#kafka-dev-sks1a-npr-shd3-mm2-mirrormaker2   2      2            2              6d19h
#kafka-kafka                                 3      3            3              77d
#kafka-zookeeper                             3      3            3              77d

# This will  restart kafka broker pods 
oc annotate strimzipodset kafka-kafka strimzi.io/manual-rolling-update="true" 

# This will  restart kafka zookeeper pods 
oc annotate statefulset kafka-zookeeper strimzi.io/manual-rolling-update=true

# Check the status of the cluster
oc rollout status sts/kafka-kafka
oc wait kafka/kafka-cluster --for=condition=Ready --timeout=1h

```

## Upgrading Strimzi Operator

[Cluster Upgrade Docs](https://strimzi.io/docs/operators/latest/deploying#con-upgrade-cluster-str)
