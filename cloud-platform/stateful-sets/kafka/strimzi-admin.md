# Administartive Notes

How to restarta kafka strimzipodset:

```bash

oc get strimzipodset
#kafka-dev-sks1a-npr-shd3-mm2-mirrormaker2   2      2            2              6d19h
#kafka-kafka                                 3      3            3              77d
#kafka-zookeeper                             3      3            3              77d

oc annotate strimzipodset kafka-kafka strimzi.io/manual-rolling-update="true" 
# This will  restart kafka broker pods 
```

## Upgrading Strimzi

[Cluster Upgrade Docs](https://strimzi.io/docs/operators/latest/deploying#con-upgrade-cluster-str)
