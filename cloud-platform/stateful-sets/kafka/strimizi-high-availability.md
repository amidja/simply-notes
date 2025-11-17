
## Topics Replication
- - - - - - - - - - - - - - - - - - - - - - - - - - - - 

How to insure that topics are replicated for high availability?

Topic configuration specifies a replication factor of at least 3 and a minimum number of in-sync replicas to 1 less than the replication factor.

```yaml
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

```

## New Section
- - - - - - - - - - - - - - - - - - - - - - - - -- - 




   
Strimzi components
 Strimzi administrator roles (strimzi-admin) - todo
 Standalone Topic Operator (topic-operator)
 Standalone User Operator (user-operator)
 Strimzi Drain Cleaner (drain-cleaner) - todo


# 