## Essential Admin Tasks

```bash
#find sentinel master
redis-cli -a $AUTH -p 26379 sentinel master mymaster

#get nodes role
redis-cli -a $AUTH -p 6379 role
redis-cli -a $AUTH -p 6379 -h sks1a-prd-shd3-redis-ha-cc-announce-2 role


```


###Diagnosing latency issues

[Redis Documentation on how to diagnose latency issues](https://redis.io/docs/latest/operate/oss_and_stack/management/optimization/latency/)

[Redis Documentation on how to enable latency monitoring](https://redis.io/docs/latest/operate/oss_and_stack/management/optimization/latency-monitor/)

```bash
#Enable the latency monitor at runtime, and set threshold to log all the events blocking the server for a time equal or greater to 100 milliseconds.
redis-cli -a $AUTH -p 6379
CONFIG SET latency-monitor-threshold 100

#Disable the latency monitor at runtime
CONFIG SET latency-monitor-threshold 0
```

The user interface to the latency monitoring subsystem is the [`LATENCY`](https://redis.io/commands/latency) command. These are [`LATENCY`](https://redis.io/commands/latency) subcommands :

- [`LATENCY LATEST`](https://redis.io/commands/latency-latest) - returns the latest latency samples for all events.
- [`LATENCY HISTORY`](https://redis.io/commands/latency-history) - returns latency time series for a given event.
- [`LATENCY RESET`](https://redis.io/commands/latency-reset) - resets latency time series data for one or more events.
- [`LATENCY GRAPH`](https://redis.io/commands/latency-graph) - renders an ASCII-art graph of an event's latency samples.
- [`LATENCY DOCTOR`](https://redis.io/commands/latency-doctor) - replies with a human-readable latency analysis report.

