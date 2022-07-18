# Values And Types

### Property types

- Number
  - Integer
  - Float
- String
- Boolean
- Point
- Temporal
  - Date
  - Time
  - LocalTimeDateTime
  - LocalDateTime
  - Duration


### Structural types

- Node
  - Id
  - Label(s): 
    - 命名规则: 驼峰命名, 并以大写字母开头
  - Map(of properties)
- Relationship
  - Id
  - Type
    - 命名规则: 全大写,并以下滑线分割
  - Map(of properties)
  - Id of the start node
  - Id of the end node
- Path:节点和关系的交替序列

### Composite types

- List
- Map