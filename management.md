Management Cheat sheet (from https://docs.janusgraph.org/latest/indexes.html)
### Transactions
#### Close current transactions before managing
 ```
 graph.tx().rollback()
 ```
#### close all open transactions
```
size = graph.getOpenTransactions().size();
for(i=0;i<size;i++){  graph.getOpenTransactions().getAt(0).rollback()}
```
### Close open management
```
mgmt = graph.openManagement()
mgmt.getOpenInstances()
mgmt.forceCloseInstance(#MANAGEMENT ID) //remove an instance
mgmt.commit()
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
