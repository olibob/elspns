#Upgrade

Reference: [https://www.elastic.co/guide/en/elasticsearch/reference/current/rolling-upgrades.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/rolling-upgrades.html)

## Prerequisites:

It is assumed you are following the guide for a rolling upgrade.

## Sequence

Replace `localhost` and add credentials according to your setup.

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

`IMPORTANT`: This a basic upgrade that does not consider plugins at this stage.

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
