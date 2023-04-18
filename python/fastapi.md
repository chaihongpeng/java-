# Request

python声明多个规则相同的接口并不会报错, fastapi会根据声明的顺序,优先执行被先声明的方法规则

## 路径参数

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/items/{item_id}")
def get_item(item_id: int):
  return {"itme_id": itme_id}
```
item\_id负责接收路径参数,int用于限制类型

## 查询参数


## 查询参数

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/items")
def list_items(skip: int = 0, limit: int = 10):
  return {"skip": skip, "limit": limit}
```

当声明不属于路径参数的其他参数时, 他们会自动解释为查询参数

## 请求体参数

```python
from fastapi import FastAPI
from pydantic import BaseModel

class Item(BaseModel):
  name: str
  description: str | None = None

app = FastAPI()

@app.post("/items")
def create_item(item:Item):
  return item

```

## 表单参数

```python
from typing import Annotated

from fastapi import FastAPI, Form

app = FastAPI()

@app.post("/login")
def login(username: Annotated[str, Form()], password: Annotated[str, Form()]):
  return {"usename": username}
```
