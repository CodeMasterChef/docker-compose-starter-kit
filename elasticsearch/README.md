# ElasticSearch:
The docker-compose.yml will generate ElasticSearch with `authentication`.

## Step 1: Generate Docker container:
```
$ docker-compose up -d
```
## Step 2: Generate passwords:

Case 1: Prompts you to manually enter password:
```bash
$ docker exec -it my_elasticsearch /bin/bash -c "bin/elasticsearch-setup-passwords interactive"
```

Case 2: Or randomly-generated password:
```bash
$ docker exec my_elasticsearch /bin/bash -c "bin/elasticsearch-setup-passwords auto --batch"

Changed password for user apm_system
PASSWORD apm_system = VNTGAZFolxDhHs04zRk3

Changed password for user kibana
PASSWORD kibana = 874tfcU31Mbm30WT3xLW

Changed password for user logstash_system
PASSWORD logstash_system = pLWJGXz6lFoLFFNB5yPA

Changed password for user beats_system
PASSWORD beats_system = AWVtuXpf2zziMeKnsv8C

Changed password for user remote_monitoring_user
PASSWORD remote_monitoring_user = LO8ULtNprv0ttoeXtrGT

Changed password for user elastic
PASSWORD elastic = JURH2hVl3D7el1txCWir
```

## Step 3: Testing:

we use `curl`, we need to use -u param with clear username and password:

```bash
$ curl --user elastic:JURH2hVl3D7el1txCWir -XGET 'http://localhost:9200/_cluster/health/'

{"cluster_name":"docker-cluster","status":"green","timed_out":false,"number_of_nodes":1,"number_of_data_nodes":1,"active_primary_shards":1,"active_shards":1,"relocating_shards":0,"initializing_shards":0,"unassigned_shards":0,"delayed_unassigned_shards":0,"number_of_pending_tasks":0,"number_of_in_flight_fetch":0,"task_max_waiting_in_queue_millis":0,"active_shards_percent_as_number":100.0}%        

```

With Restful API, we need to add the Authorization in Header. The authorization is using Basic Authorization. So that, we need to use base64 to encode `user:password` . You can use http://base64encode.org to create base64 of `user:password`.

```bash
$ curl --location --request GET 'http://localhost:9200/_cluster/health/' \
--header 'Authorization: Basic ZWxhc3RpYzpKVVJIMmhWbDNEN2VsMXR4Q1dpcg=='s

{"cluster_name":"docker-cluster","status":"green","timed_out":false,"number_of_nodes":1,"number_of_data_nodes":1,"active_primary_shards":1,"active_shards":1,"relocating_shards":0,"initializing_shards":0,"unassigned_shards":0,"delayed_unassigned_shards":0,"number_of_pending_tasks":0,"number_of_in_flight_fetch":0,"task_max_waiting_in_queue_millis":0,"active_shards_percent_as_number":100.0}%  

```