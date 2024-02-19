# Janusgraph

## janusgraph 使用
```bash
export JAVA_OPTIONS=""
./bin/gremlin.sh
# 执行janusgraph命令
graph = GraphFactory.open("/etc/janusgraph.properties")
g=graph.traversal()
g.V().hasLabel('ENTITY').has('entity_id','anonymous_xxxx').outE().inV().outE().inV().valueMap()
```