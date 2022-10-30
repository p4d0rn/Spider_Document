# 一、What's Neo4j 

Neo4j是一个NOSQL图形数据库，它将数据存储在图上而不是表中。

使用场景

* 欺诈检测
* 实时推荐引擎
* 知识图谱
* 主数据管理
* 身份和访问管理

# 二、CQL

图查询语言Cypher

1.    Finding graph data

`MATCH (a:Person {name:'Tom Hanks'}) RETURN a`

2.    Add nodes

`CREATE (a:Person {name:'Brie Larson', born:1989}) RETURN a`

`CREATE (a:Movie {title:'Captain Marvel', released:2019, tagline:'Everything begins with a (her)o.'}) RETURN a`

3.    Remove nodes

`MATCH (a:Person {name:'Brie Larson'}) DETACH DELETE a`

4.    Update nodes

```
MERGE (a:Person {name:'Brie Larson'}) 
ON CREATE SET a.born = 1989 
ON MATCH SET a.stars = COALESCE(a.stars, 0) + 1
RETURN a
```

ON CREATE  设置新建节点的值
ON MATCH  更新存在节点的值

# py2neo

#### 1. 连接Neo4j数据库

```python
from py2neo import Graph, Node, Relationship
graph = Graph("http://localhost:7474", username="neo4j", password="neo4j")
```

默认端口7474，默认用户名和密码均为neo4j

#### 2. 节点建立

```python
node_1 = Node("Dog", name="chalk")
node_2 = Node("Dog", name="noodle")
graph.create(node_1)
graph.create(node_2)
```

a. 节点类型(label)：Dog
b. 基本属性(property_key=property_value) name=chalk、noodle

#### 3. 节点关系建立

```python
r1 = Relationship(node_1, 'FRIENDS_TO', node_2)
r1['count'] = 1
r2 = Relationship(node_2, 'FRIENDS_TO', node_1)
r2['count'] = 2
graph.create(r1)
graph.create(r2)
```

#### 4. 节点/关系的属性更新

```python
r1['count'] += 1
graph.push(r1)  # 使用push来更新
```

#### 5. 节点查找

a. 通过属性值查找

```python
dog_search = graph.nodes.match('Dog', name='noodle').first()
dog_search['hobby'] = 'play'
graph.push(dog_search)
print(dog_search) # (_1:Dog {hobby: 'play', name: 'noodle'})
```

b. 通过关系查找

```python
dog_search = RelationshipMatch(graph)
print(dog_search.first())
```

