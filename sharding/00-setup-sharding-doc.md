## Set up Sharding using Docker Containers

### Config servers
Start config servers (3 member replica set)
```
docker-compose -f config-server/docker-compose.yaml up -d
```
Initiate replica set
```
mongo mongodb://10.0.1.67:40001
```
```
rs.initiate(
  {
    _id: "cfgrs",
    configsvr: true,
    members: [
      { _id : 0, host : "10.0.1.67:40001" },
      { _id : 1, host : "10.0.1.67:40002" },
      { _id : 2, host : "10.0.1.67:40003" }
    ]
  }
)

rs.status()
```

### Shard 1 servers
Start shard 1 servers (3 member replica set)
```
docker-compose -f shard1/docker-compose.yaml up -d
```
Initiate replica set
```
mongo mongodb://10.0.1.67:50001
```
```
rs.initiate(
  {
    _id: "shard1rs",
    members: [
      { _id : 0, host : "10.0.1.67:50001" },
      { _id : 1, host : "10.0.1.67:50002" },
      { _id : 2, host : "10.0.1.67:50003" }
    ]
  }
)

rs.status()
```

### Mongos Router
Start mongos query router
```
docker-compose -f mongos/docker-compose.yaml up -d
```

### Add shard to the cluster
Connect to mongos
```
mongo mongodb://10.0.1.67:60000
```
Add shard
```
mongos> sh.addShard("shard1rs/10.0.1.67:50001,10.0.1.67:50002,10.0.1.67:50003")
mongos> sh.status()
```
## Adding another shard
### Shard 2 servers
Start shard 2 servers (3 member replica set)
```
docker-compose -f shard2/docker-compose.yaml up -d
```
Initiate replica set 
```
mongo mongodb://10.0.0.12:50004 
```
```
rs.initiate(
  {
    _id: "shard2rs",
    members: [
      { _id : 0, host : "10.0.0.12:50004" },
      { _id : 1, host : "10.0.0.12:50005" },
      { _id : 2, host : "10.0.0.12:50006" }
    ]
  }
)

rs.status()
```
### Add shard to the cluster
Connect to mongos
```
mongo mongodb://10.0.0.12:60000
```
Add shard
```
mongos> sh.addShard("shard2rs/10.0.0.12:50004,10.0.0.12:50005,10.0.0.12:50006")
mongos> sh.status()
```
