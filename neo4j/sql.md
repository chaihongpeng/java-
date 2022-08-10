# CQL

#### CREATE

创建节点

#### MATCH

检索有关节点和属性

#### RETURN

返回查询结果

#### WHERE

检索条件

#### DELETE

删除节点和关系

#### REMOVE

删除节点和关系的属性

#### ORDER BY

排序检索数据

#### SET

添加或更新标签

## Cypher语言来描述关系

```CQL
(lihua)<-[:friend]-(wangpeng)-[:son]->(wanggang)
```

## Patterns

#### Patterns for nodes

```cypher
(n)
```

#### Patterns for related nodes

```cypher
(a)-->(b)
(b)-->(b)<--(c)
//命名不是必须的,只有当仅当需要再次引用这些节点时才需要命名
(b)-->()<--(c)
```

#### Patterns for labels

```cypher
(a:User)-->(b)
# label交集查询
(a:User:Admin)-->(b)
```

#### Specifying properties

```cypher
//具体的属性名
(a {name: 'Andy', sport: 'Brazilian Ju-Jitsu'})
(a)-[{blocked: false}]->(b)
```

#### Patterns for relationships

```cypher
//如果不关心方向, 可以省略箭头
(a)--(b)
//关系也可以被命名
(a)-[r]->(b)
//就像节点上的标签一样，关系也可以有类型。
(a)-[r:REL_TYPE]->(b)
//与标签不同,关系只能有一种类型,但是如果想查看多种类型,就可以使用|表示并集
(a)-[r:TYPE1|TYPE2]->(b)
//与节点一样, 关系的名字可以省略
(a)-[:REL_TYPE]->(b)
```

#### Variable-length pattern matching

```cypher
//标识查询三个节点,两个关系的图, 等效于 (a)-->(b)-->(c)
(a)-[*2]->(b)
# 可变长度关系
(a)-[*3..5]->(b)
(a)-[*3..]->(b)
(a)-[*..5]->(b)
//允许任何长度的路径
(a)-[*]->(b)
```
#### Method

```cypher
//获取node的id
id(n)
//将对象包装为数组
collect(n)
```

```cypher
//创建节点
create (n:Person {name:"mark"}) return n
//retur可以省略
create (n:Person {name:"mark"})
//创建关系
match (n:Person {name:"zhang"})
match (m:Person {name:"li"})
create (n)-[r:IS_FRIENDS_WITH]->(m)
//更新节点
match (n:Person {name:"li"})
set n.birthday = date("1975-2-01")
return n
//更新关系
match (n:Person {name:"li"})-[r:IS_FRIENDS_WITH]->(m:Person {name:"zhang"})
set r.startYear=date(2022-01-12)
return r
//删除关系
match (n:Person {name:"li"})-[r:IS_FRIENDS_WITH]->(m:Person {name:"zhang"})
delete r
//删除节点
match (n:Person {name:"li"})
delete m
//删除节点和关系
match (n:Person {name:"li"})
detach delete n
//删除属性
MATCH (n:Person {name: 'Jennifer'})
REMOVE n.birthdate
//删除属性2
MATCH (n:Person {name: 'Jennifer'})
SET n.birthdate = null
//合并, 如果neo4j中存在完全相同的节点,则不会创建新的节点, 可以防止重复创建节点
MERGE (mark:Person {name: 'Mark'})
RETURN mark
//关系合并
MATCH (j:Person {name: 'Jennifer'})
MATCH (m:Person {name: 'Mark'})
MERGE (j)-[r:IS_FRIENDS_WITH]->(m)
RETURN j, r, m
```

#### Where

```cypher
// 否定查询
MATCH (j:Person) WHERE NOT j.name = 'Jennifer' RETURN j;
// 范围查询
MATCH (p:Person) WHERE 3 <= p.yearsExp <= 7 RETURN p;
//判断属性是否存在
MATCH (p:Person) WHERE exists(p.birthdate) RETURN p.name;
```

#### Like

```cypher
// 以xx开头
MATCH (p:Person) WHERE p.name STARTS WITH 'M' RETURN p.name;
//包含xx单词
MATCH (p:Person) WHERE p.name CONTAINS 'a' RETURN p.name;
//以xx结尾
MATCH (p:Person) WHERE p.name ENDS WITH 'n' RETURN p.name;
//支持正则
MATCH (p:Person) WHERE p.name =~ 'Jo.*' RETURN p.name;
// in查询
MATCH (p:Person) WHERE p.yearsExp IN [1, 5, 6] RETURN p.name, p.yearsExp
```

####  Filtering on Patterns

```cypher
//查看在Neo4j公司工作的人的朋友
MATCH (p:Person)-[r:IS_FRIENDS_WITH]->(friend:Person) 
WHERE exists((p)-[:WORKS_FOR]->(:Company {name: 'Neo4j'}))
RETURN p, r, friend;
//查看Jennifer没有在公司工作的朋友
MATCH (p:Person)-[r:IS_FRIENDS_WITH]->(friend:Person) WHERE p.name = 'Jennifer'
AND NOT exists((friend)-[:WORKS_FOR]->(:Company))
RETURN friend.name;
```

#### Query Processing

```cypher
//count()计数twitter非空的人
MATCH (p:Person) RETURN count(p.twitter);
//count()计数所有人
MATCH (p:Person) RETURN count(*);
                              
//collect()将指定的值聚合到列表中
//根据测试,neo4j会检测剩余的部分数据是否完全相同进而进行分组后在执行collect()
MATCH (p:Person)-[:IS_FRIENDS_WITH]->(friend:Person)
RETURN p.name, collect(friend.name) AS friend;

//size() 获取列表的值
MATCH (p:Person)-[:IS_FRIENDS_WITH]->(friend:Person)
RETURN p.name, collect(friend.name) AS friend;

//WITH 将子句值从一部分传递到另一部分
MATCH (a:Person)-[r:LIKES]-(t:Technology)
WITH a.name AS name, collect(t.type) AS technologies
RETURN name, technologies;

//unwind 和collect()作用相反,用于将集合拆解开
WITH ['Graphs','Query Languages'] AS techRequirements
UNWIND techRequirements AS technology
MATCH (p:Person)-[r:LIKES]-(t:Technology {type: technology})
RETURN t.type, collect(p.name) AS potentialCandidates;

//ORDER BY 排序
unwind [6,7,9,1,44,11,3] as item
return item order by item

//distinct 去重
MATCH (user:Person)
WHERE user.twitter IS NOT null
WITH user
MATCH (user)-[:LIKES]-(t:Technology)
WHERE t.type IN ['Graphs','Query Languages']
RETURN DISTINCT user.name

//limit 限制
MATCH (p:Person)-[r:IS_FRIENDS_WITH]-(other:Person)
RETURN p.name, count(other.name) AS numberOfFriends
ORDER BY numberOfFriends DESC
LIMIT 3
```

#### SubQuery

```cypher
// 子查询
MATCH (p:Person)-[r:IS_FRIENDS_WITH]->(friend:Person)
WHERE exists((p)-[:WORKS_FOR]->(:Company {name: 'Neo4j'}))
RETURN p, r, friend

//EXISTS子句
MATCH (p:Person)-[r:IS_FRIENDS_WITH]->(friend:Person)
WHERE EXISTS {
  MATCH (p)-[:WORKS_FOR]->(:Company {name: 'Neo4j'})
}
RETURN p, r, friend

//使用and连接的子查询之间变量不互通
//执行下面例子会报错,找不到t
MATCH (person:Person)-[:WORKS_FOR]->(company)
WHERE company.name STARTS WITH "Company"
AND (person)-[:LIKES]->(t:Technology)
AND size((t)<-[:LIKES]-()) >= 3
RETURN person.name as person, company.name AS company;
//以下方式可以获取到t
MATCH (person:Person)-[:WORKS_FOR]->(company)
WHERE company.name STARTS WITH "Company"
AND EXISTS {
  MATCH (person)-[:LIKES]->(t:Technology)
  WHERE size((t)<-[:LIKES]-()) >= 3
}
RETURN person.name as person, company.name AS company;

//call可以将返回值进行组合并去重
CALL {
	MATCH (p:Person)-[:LIKES]->(:Technology {type: "Java"})
	RETURN p

	UNION

	MATCH (p:Person)
	WHERE size((p)-[:IS_FRIENDS_WITH]->()) > 1
	RETURN p
}
RETURN p.name AS person, p.birthdate AS dob
ORDER BY dob DESC;
```













