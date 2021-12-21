# MongoDB Sharding Minimal Example
You can find a minimal production environment setup of a MongoDB Cluster in this fork. Each MongoDB instance is setted up with Docker Compose.
Each Shard has different physical machine, sample machine IPs in this example are: 10.0.1.67, 10.0.0.12.

All external IPs are allowed to reach mongoDB containers in this fork. If you use this as a production cluster, I strongly recommend to bind necessary IPs only. To do it, change the mongod commands in the docker compose YAMLs.

[Associated YouTube Playlist](https://www.youtube.com/watch?v=LBthwZDRR-c&list=PL34sAs7_26wPvZJqUJhjyNtm7UedWR8Ps)
