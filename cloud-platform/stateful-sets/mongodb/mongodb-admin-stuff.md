# MongoDB Admin Stuff

[Server Status Command](https://www.mongodb.com/docs/manual/reference/command/serverStatus/)

```
db.serverStatus().wiredTiger.cache

db.serverStatus().mem
```

## Performance Monitoring

[Monitoring MongoDb - WiredTiger ](https://www.datadoghq.com/blog/monitoring-mongodb-performance-metrics-wiredtiger/)

### Diagnostic Commands
Information regarding the open outgoing connections from the current database instance to other members of th cluster or replica set.
`db.runCommand( { "connPoolStats" : 1 } )`

The getLog only shows the most recent 1024 logged mongod events, and is not a replacement for the MongoDB log file.
`db.adminCommand( { getLog:'global'} ).log.forEach(x => {print(x)})`

Returns information on locks that are currently being held or pending
`db.adminCommand( { lockInfo: 1 } )`

`db.adminCommand("top")`


### Query Plan

[analyze-query-plan](https://www.mongodb.com/docs/manual/tutorial/analyze-query-plan/)


### Mongotop/Mongostat

[mongotop](https://docs.mongodb.com/v4.0/reference/program/mongotop/)
[mongostat](https://docs.mongodb.com/database-tools/mongostat/)
