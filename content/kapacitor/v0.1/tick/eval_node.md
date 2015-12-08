---
title: EvalNode
note: Auto generated by tickdoc

menu:
  kapacitor_01:
    name: Eval
    identifier: eval
    weight: 60
    parent: tick
---

Evaluates expressions on each data point it receives. 
A list of expressions may be provided and will be evaluated in the order they are given 
and results of previous expressions are made available to later expressions. 
See the property [EvalNode.As](/kapacitor/v0.1/tick/eval_node/#as) for details on how to reference the results. 

Example: 


```javascript
    stream
        .eval(lambda: "error_count" / "total_count")
          .as('error_percent')
```

The above example will add a new field `error_percent` to each 
data point with the result of `error_count / total_count` where 
`error_count` and `total_count` are existing fields on the data point. 



Properties
----------

Property methods modify state on the calling node. They do not add another node to the pipeline, and always return a reference to the calling node.

### As

List of names for each expression. 
The expressions are evaluated in order and the result 
of a previous expression will be available in later expressions 
via the name provided. 

Example: 


```javascript
    stream
        .eval(lambda: "value" * "value", lambda: 1.0 / "value2")
            .as('value2', 'inv_value2')
```

The above example calculates two fields from the value and names them 
`value2` and `inv_value2` respectively. 



```javascript
node.as(names ...string)
```


### Keep

If called the existing fields will be preserved in addition 
to the new fields being set. 
If not called then only new fields are preserved. 

Optionally intermediate values can be discarded 
by passing a list of field names. 
Only fields in the list will be kept. 
If no list is given then all fields, new and old, are kept. 

Example: 


```javascript
    stream
        .eval(lambda: "value" * "value", lambda: 1.0 / "value2")
            .as('value2', 'inv_value2')
        .keep('value', 'inv_value2')
```

In the above example the original field `value` is preserved. 
In addition the new field `value2` is calculated and used in evaluating 
`inv_value2` but is discarded before the point is sent on to children nodes. 
The resulting point has only two fields `value` and `inv_value2`. 


```javascript
node.keep(fields ...string)
```


Chaining Methods
----------------

Chaining methods create a new node in the pipeline as a child of the calling node. They do not modify the calling node.

### Alert

Create an alert node, which can trigger alerts. 


```javascript
node.alert()
```

Returns: [AlertNode](/kapacitor/v0.1/tick/alert_node/)


### Derivative

Create a new node that computes the derivative of adjacent points. 


```javascript
node.derivative(field string)
```

Returns: [DerivativeNode](/kapacitor/v0.1/tick/derivative_node/)


### Eval

Create an eval node that will evaluate the given transformation function to each data point. 
A list of expressions may be provided and will be evaluated in the order they are given 
and results of previous expressions are made available to later expressions. 


```javascript
node.eval(expressions ...tick.Node)
```

Returns: [EvalNode](/kapacitor/v0.1/tick/eval_node/)


### GroupBy

Group the data by a set of tags. 

Can pass literal * to group by all dimensions. 
Example: 


```javascript
    .groupBy(*)
```



```javascript
node.groupBy(tag ...interface{})
```

Returns: [GroupByNode](/kapacitor/v0.1/tick/group_by_node/)


### HttpOut

Create an http output node that caches the most recent data it has received. 
The cached data is available at the given endpoint. 
The endpoint is the relative path from the API endpoint of the running task. 
For example if the task endpoint is at &#34;/api/v1/task/&lt;task_name&gt;&#34; and endpoint is 
&#34;top10&#34;, then the data can be requested from &#34;/api/v1/task/&lt;task_name&gt;/top10&#34;. 


```javascript
node.httpOut(endpoint string)
```

Returns: [HTTPOutNode](/kapacitor/v0.1/tick/http_out_node/)


### InfluxDBOut

Create an influxdb output node that will store the incoming data into InfluxDB. 


```javascript
node.influxDBOut()
```

Returns: [InfluxDBOutNode](/kapacitor/v0.1/tick/influx_d_b_out_node/)


### Join

Join this node with other nodes. The data is joined on timestamp. 


```javascript
node.join(others ...Node)
```

Returns: [JoinNode](/kapacitor/v0.1/tick/join_node/)


### MapReduce

Perform a map-reduce operation on the data. 
The built-in functions under `influxql` provide the 
selection,aggregation, and transformation functions 
from the InfluxQL language. 

MapReduce may be applied to either a batch or a stream edge. 
In the case of a batch each batch is passed to the mapper idependently. 
In the case of a stream all incoming data points that have 
the exact same time are combined into a batch and sent to the mapper. 


```javascript
node.mapReduce(mr MapReduceInfo)
```

Returns: [ReduceNode](/kapacitor/v0.1/tick/reduce_node/)


### Sample

Create a new node that samples the incoming points or batches. 

One point will be emitted every count or duration specified. 


```javascript
node.sample(rate interface{})
```

Returns: [SampleNode](/kapacitor/v0.1/tick/sample_node/)


### Union

Perform the union of this node and all other given nodes. 


```javascript
node.union(node ...Node)
```

Returns: [UnionNode](/kapacitor/v0.1/tick/union_node/)


### Where

Create a new node that filters the data stream by a given expression. 


```javascript
node.where(expression tick.Node)
```

Returns: [WhereNode](/kapacitor/v0.1/tick/where_node/)


### Window

Create a new node that windows the stream by time. 

NOTE: Window can only be applied to stream edges. 


```javascript
node.window()
```

Returns: [WindowNode](/kapacitor/v0.1/tick/window_node/)
