Tree Map

- root: Entry
- comparator
  1. t=this.root
  2. t为null, 就创建新Entry并赋值给this.root上
  3. parent = t
  4. t与k进行比较, 决定left 或是 right
  5. addEntry()
  6. Entry{left,right,parent, key,value, color:BLACK}
  7. fixAfterInsertion





# 红黑树

## 性质

1. 节点是红或者黑
2. 根节点为黑
3. 叶子节点(NIL节点), 都是黑色
4. 红节点的两个孩子节点都是黑色(从叶子到根的所有路径上不存在两个连续的红色节点)
5. 任意一节点到每一个叶子节点所有路径包含相同的黑色节点

## 自平衡策略

- **左旋**
  对于当前节点, 如果右子节点为红, 左子节点为黑, 则进行左旋
  
  ```mermaid
  graph TB
  P((P))
  L((L))
  R((R))
  RR((RR))
  RL((RL))
  P-->L
  P-->R
  R-->RL
  R-->RR
  
  P2((P))
  L2((L))
  R2((R))
  RR2((RR))
  RL2((RL))
  P2-->L2
  P2-->RL2
  R2-->P2
  R2-->RR2
  ```
  
- **右旋**
  对于当前结点, 如果左子, 左孙子结点均为红色, 则执行右旋转
- **变色**
  对于当前结点, 如果左, 右子结点均为红色, 则执行变色

## 插入策略

- 新插入的节点一律为红

- 新插入节点是根节点
  插入后, 违背[性质2](#性质), 根节点不能为黑
  执行变色操作
  
- 父节点为黑直接插入

```mermaid
graph TB
P((P))
X((X))
B((B))
P-->X
P-->B

style P fill:black,stroke-width:0px,color:white
```

- 如果父类是祖父的左节点
  - 如果叔叔点是红色
    父节点, 叔叔节点, 祖父节点全部变色
    当前节点左边移动到祖父节点
  - 如果叔叔节点是黑色
    - 当前节点是父节点的右节点
      将当前节点切换为父节点
      左旋当前节点(切换后的父节点)
      当前节点的父节点变黑
      当前节点的祖父节点变红
      右旋当前节点的祖父节点

```mermaid
graph TB
G((G))
P((P))
U((U))
X((X))
G-->P-->X
G-->U

G2((G))
P2((P))
U2((U))
X2((X))
G2-->P2-->X2
G2-->U2


style G fill:black,stroke-width:0px,color:white
style U2 fill:black,stroke-width:0px,color:white
style P2 fill:black,stroke-width:0px,color:white
```

```mermaid
graph TB
G((G))
P((P))
U((U))
B((B))
X((X))
G-->P
P-->X
P-->B
G-->U

style G fill:black,stroke-width:0px,color:white
style U fill:black,stroke-width:0px,color:white
style B fill:black,stroke-width:0px,color:white

G2((G))
P2((P))
U2((U))
B2((B))
X2((X))
G2-->P2
P2-->X2
P2-->B2
G2-->U2

style P2 fill:black,stroke-width:0px,color:white
style U2 fill:black,stroke-width:0px,color:white
style B2 fill:black,stroke-width:0px,color:white

P3((P))
X3((X))
G3((G))
U3((U))
B3((B))

P3-->G3
G3-->B3
P3-->X3
G3-->U3

style P3 fill:black,stroke-width:0px,color:white
style U3 fill:black,stroke-width:0px,color:white
style B3 fill:black,stroke-width:0px,color:white
```



```mermaid
graph TB
G((G))
P((P))
U((U))
B((B))
X((X))
G-->P
P-->X
P-->B
G-->U

style G fill:black,stroke-width:0px,color:white
style U fill:black,stroke-width:0px,color:white
style B fill:black,stroke-width:0px,color:white

G2((G))
P2((P))
X2((X))
B2((B))
U2((U))

G2-->X2
X2-->P2
P2-->B2
G2-->U2

style U2 fill:black,stroke-width:0px,color:white
style B2 fill:black,stroke-width:0px,color:white

G3((G))
P3((P))
X3((X))
B3((B))
U3((U))

X3-->G3
X3-->P3
P3-->B3
G3-->U3

style U3 fill:black,stroke-width:0px,color:white
style B3 fill:black,stroke-width:0px,color:white
style X3 fill:black,stroke-width:0px,color:white
```



当前节点为根节点：

​	根节点直接变黑

当前节点的父节点是黑：

​	什么都不做

当前节点的父节点、叔父节点都是红色的：

​	1.父节点、叔父节点都变为黑色

​	2.祖父节点变为红色

​	3.指针移动到祖父节点，继续判断

当前节点父节点为红，叔父节点为黑，当前节点靠近叔父节点：

​	1.指针移动到父节点

​	2.以父节点为支点，向叔父节点相反方向旋转

当前节点父节点为红，叔父节点为黑，当前节点远离叔父节点：

​	1.指针移向父节点，父节点变为黑色

​	2.指针移向祖父节点，祖父节点变为红色

​	3以祖父节点为支点，向叔父节点方向旋转

 

最后一步，总能保证最上层节点为黑色。所以最多经历三步变化总能保证红黑树特性

 