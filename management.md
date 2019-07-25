Management Cheat sheet (from https://docs.janusgraph.org/latest/indexes.html)
### Close current transactions before managing
 ```
 graph.tx().rollback()
 ```

### Open management
```
mgmt = graph.openManagement()
```

### List labels
#### Vertex
```
mgmt.getVertexLabels()
```
#### Edges
```
mgmt.printEdgeLabels()
```

### Indexing

```
name = mgmt.getPropertyKey('name')
age = mgmt.getPropertyKey('age')
mgmt.buildIndex('nameAndAge', Vertex.class).addKey(name).addKey(age).buildMixedIndex("search")
mgmt.commit()
```
In buildMixedIndex, "search" is the keyword appearing in the configuration in index.*search*.backend

### Waiting for index to be ready
```
ManagementSystem.awaitGraphIndexStatus(graph, 'nameAndAge').call()
```
### Reindex the existing data
```
mgmt = graph.openManagement()
mgmt.updateIndex(mgmt.getGraphIndex("nameAndAge"), SchemaAction.REINDEX).get()
mgmt.commit()
```
