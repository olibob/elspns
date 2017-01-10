#Upgrade

Reference: [https://www.elastic.co/guide/en/elasticsearch/reference/current/rolling-upgrades.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/rolling-upgrades.html)

## Prerequisites:

Stop kibana.

```
ansible -i lpns elastic -s -m shell -a 'systemctl stop kibana'
```

## Sequence

1/ Disable shard allocation

```
curl -XPUT 'localhost:9200/_cluster/settings?pretty' -d'
{
  "transient": {
    "cluster.routing.allocation.enable": "none"
  }
}'
```


2/ Stop non-essential indexing

```
curl -XPOST 'localhost:9200/_flush/synced?pretty'
```

3/ run upgrade playbook

4/ Check node is back in the cluster

```
curl -XGET 'localhost:9200/_cat/nodes?pretty'
```

5/ Reenable shard allocation

```
curl -XPUT 'localhost:9200/_cluster/settings?pretty' -d'
{
  "transient": {
    "cluster.routing.allocation.enable": "all"
  }
}'
``
