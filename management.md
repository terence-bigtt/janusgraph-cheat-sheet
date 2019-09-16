Management Cheat sheet (from https://docs.janusgraph.org/latest/indexes.html)
### Transactions
#### Close current transactions before managing
 ```
 graph.tx().rollback()
 ```
 
#### Commit transaction

 ```
 graph.tx().commit()
 ```
 
#### Multithreading

A transaction will commit by default what happens in the LocalThread. To support multithreading, one must use threaded tx:
```
graph.tx().createThreadedTransaction()
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
see https://groups.google.com/forum/#!topic/janusgraph-users/E0speKxetgM

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

Note that this command will wait for an index to be installed. If the index is already in use, it will timeout.

### Reindex the existing data
```
mgmt = graph.openManagement()
mgmt.updateIndex(mgmt.getGraphIndex("nameAndAge"), SchemaAction.REINDEX).get()
mgmt.commit()
```

### Schema
```
mgmt.printSchema()
```
