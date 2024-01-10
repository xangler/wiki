# Elasticsearch

## elasticsearch 使用
```bash
# 系统状态/节点状态/indices状态三连
curl -X GET 'elasticsearch-default:9200/_cat/health?v'
curl -X GET 'elasticsearch-default:9200/_cat/nodes?v'
curl -X GET 'elasticsearch-default:9200/_cat/indices?v'
# indices 二连
curl -X GET 'elasticsearch-default:9200/_cat/templates?v'
curl -X GET 'elasticsearch-default:9200/_template/namespace'
# 查找统计
curl -X GET 'elasticsearch-default:9200/namespaces/_search' -H 'Content-Type: application/json' -d '{"query":{"match_all":{}}, "size":1}'
curl -X GET 'elasticsearch-default:9200/namespaces/_count' -H 'Content-Type: application/json' -d '{"query":{"bool":{"must":{"terms":{"description":["default"]}}}}}'
curl -X GET 'elasticsearch-default:9200/namespaces/_count' -H 'Content-Type: application/json' -d '{"query":{"terms":{"description":["default"]}}}'
```