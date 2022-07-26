创建节点

```CQL
//创建单个
create (variable:Lable {key1:value1, key2:value2}) return variable
//批量创建,用逗号分隔多个括号
create (variable1:Lable1 {key11:value11, key12:value12}),(variale2:Lable2 {key21:value21,key22:value22}) return variable
//增加节点属性
match (s:Student) where s.name="lucy" set s.age=25 return s
//删除节点属性
match (s:Student) where s.name="lucy" remove s.age return s
```



