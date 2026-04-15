# 检索索引：本文档收录【FastAPI】【核心基础】知识点，内容100%来自官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

FastAPI 是一个现代、快速（高性能）的 Web 框架，用于基于标准 Python 类型提示使用 Python 3.7+ 构建 API。它的主要特点包括：快速、强大的类型提示、自动文档生成、基于标准（OpenAPI 和 JSON Schema）、易于学习、代码简洁、健壮性强、基于 Starlette 和 Pydantic。

## 2. 核心知识点（结构化层级）

### 2.1 基础路由

- HTTP 方法：GET、POST、PUT、DELETE、PATCH
- 路径参数：Path、Query、Body
- 查询参数：可选参数、默认值、必填参数
- 请求体：Pydantic 模型
- 响应模型：Response Model

### 2.2 数据验证

- Pydantic 模型：字段类型、默认值、验证器
- 字段类型：str、int、float、bool、list、dict、datetime
- 验证器：@validator、字段验证、自定义验证
- 枚举：Enum 类
- 可选类型：Optional、Union

### 2.3 路径操作配置

- 状态码：status_code
- 标签：tags
- 摘要：summary
- 描述：description
- 响应描述：response_description
- 弃用：deprecated

### 2.4 请求体

- 嵌套模型：Nested Models
- 列表：List[T]
- 集合：Set[T]
- 字典：Dict[str, T]
- 特殊类型：EmailStr、UrlStr
- 请求体字段：Field

### 2.5 查询参数和字符串验证

- Query：默认值、可选值、必填值
- 字符串验证：min_length、max_length、regex
- 数值验证：ge、gt、le、lt
- 多个查询参数
- 必需查询参数

### 2.6 路径参数和数值验证

- Path：路径参数验证
- 数值验证：ge、gt、le、lt
- 路径参数与查询参数的顺序

### 2.7 Cookie、Header、响应

- Cookie：Cookie 参数
- Header：Header 参数
- 响应状态码：status_code
- 响应头：Response.headers
- 响应 Cookies：Response.set_cookie

### 2.8 表单数据和文件上传

- 表单数据：Form
- 文件上传：File、UploadFile
- 多文件上传：List[UploadFile]
- 表单与文件混合

### 2.9 错误处理

- HTTPException：HTTP 异常
- 自定义异常处理器
- 验证错误处理
- 异常响应

### 2.10 依赖注入

- 依赖函数：Depends
- 类依赖
- 嵌套依赖
- 子依赖
- 路径操作装饰器依赖
- 全局依赖

## 3. 官方标准语法 / API

```python
from fastapi import FastAPI, Path, Query, Body, Depends, HTTPException
from pydantic import BaseModel, Field, validator
from typing import Optional, List, Dict
from datetime import datetime

# 创建 FastAPI 应用
app = FastAPI(title="My API", version="1.0.0")

# Pydantic 模型定义
class Item(BaseModel):
    name: str = Field(..., title="Item Name", min_length=2, max_length=50)
    description: Optional[str] = Field(None, title="Item Description", max_length=300)
    price: float = Field(..., gt=0, description="Price must be greater than 0")
    tax: Optional[float] = Field(None, ge=0)
    tags: List[str] = []
    created_at: datetime = Field(default_factory=datetime.now)
    
    @validator('name')
    def name_must_not_be_empty(cls, v):
        if not v.strip():
            raise ValueError('Name cannot be empty')
        return v.title()

# 基础路由
@app.get("/")
async def root():
    return {"message": "Hello World"}

# 路径参数
@app.get("/items/{item_id}")
async def read_item(
    item_id: int = Path(..., title="The ID of the item to get", ge=1),
    q: Optional[str] = Query(None, min_length=3, max_length=50)
):
    result = {"item_id": item_id}
    if q:
        result.update({"q": q})
    return result

# POST 请求与请求体
@app.post("/items/", response_model=Item, status_code=201)
async def create_item(item: Item):
    return item

# PUT 请求
@app.put("/items/{item_id}")
async def update_item(
    item_id: int,
    item: Item = Body(..., embed=True)
):
    return {"item_id": item_id, **item.dict()}

# DELETE 请求
@app.delete("/items/{item_id}", status_code=204)
async def delete_item(item_id: int):
    return None

# 依赖注入示例
def common_parameters(
    q: Optional[str] = None,
    skip: int = 0,
    limit: int = 100
):
    return {"q": q, "skip": skip, "limit": limit}

@app.get("/items/")
async def read_items(commons: dict = Depends(common_parameters)):
    return commons

# 错误处理
@app.get("/items/{item_id}/error")
async def read_item_with_error(item_id: int):
    if item_id == 0:
        raise HTTPException(status_code=400, detail="Item ID cannot be zero")
    return {"item_id": item_id}
```

## 4. 官方示例代码（可运行）

```python
# main.py - 完整的 FastAPI 应用示例
from fastapi import FastAPI, Depends, HTTPException, status
from fastapi.security import OAuth2PasswordBearer, OAuth2PasswordRequestForm
from pydantic import BaseModel, EmailStr
from typing import Optional, List
from datetime import datetime, timedelta
from jose import JWTError, jwt
from passlib.context import CryptContext

# 配置
SECRET_KEY = "your-secret-key-keep-it-safe"
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30

# 密码加密
pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")

# OAuth2
oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

# 模拟数据库
fake_users_db = {
    "johndoe": {
        "username": "johndoe",
        "full_name": "John Doe",
        "email": "johndoe@example.com",
        "hashed_password": "$2b$12$EixZaYVK1fsbw1ZfbX3OXePaWxn96p36WQoeG6Lruj3vjPGga31lW",
        "disabled": False,
    }
}

fake_items_db = []

# Pydantic 模型
class Token(BaseModel):
    access_token: str
    token_type: str

class TokenData(BaseModel):
    username: Optional[str] = None

class UserBase(BaseModel):
    username: str
    email: Optional[EmailStr] = None
    full_name: Optional[str] = None
    disabled: Optional[bool] = None

class UserCreate(UserBase):
    password: str

class User(UserBase):
    class Config:
        orm_mode = True

class ItemBase(BaseModel):
    title: str
    description: Optional[str] = None

class ItemCreate(ItemBase):
    pass

class Item(ItemBase):
    id: int
    owner_id: str
    created_at: datetime
    
    class Config:
        orm_mode = True

# FastAPI 应用
app = FastAPI(title="FastAPI 完整示例", version="1.0.0")

# 辅助函数
def verify_password(plain_password, hashed_password):
    return pwd_context.verify(plain_password, hashed_password)

def get_password_hash(password):
    return pwd_context.hash(password)

def get_user(db, username: str):
    if username in db:
        user_dict = db[username]
        return User(**user_dict)

def authenticate_user(fake_db, username: str, password: str):
    user = get_user(fake_db, username)
    if not user:
        return False
    if not verify_password(password, user.hashed_password):
        return False
    return user

def create_access_token(data: dict, expires_delta: Optional[timedelta] = None):
    to_encode = data.copy()
    if expires_delta:
        expire = datetime.utcnow() + expires_delta
    else:
        expire = datetime.utcnow() + timedelta(minutes=15)
    to_encode.update({"exp": expire})
    encoded_jwt = jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)
    return encoded_jwt

async def get_current_user(token: str = Depends(oauth2_scheme)):
    credentials_exception = HTTPException(
        status_code=status.HTTP_401_UNAUTHORIZED,
        detail="Could not validate credentials",
        headers={"WWW-Authenticate": "Bearer"},
    )
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        username: str = payload.get("sub")
        if username is None:
            raise credentials_exception
        token_data = TokenData(username=username)
    except JWTError:
        raise credentials_exception
    user = get_user(fake_users_db, username=token_data.username)
    if user is None:
        raise credentials_exception
    return user

async def get_current_active_user(current_user: User = Depends(get_current_user)):
    if current_user.disabled:
        raise HTTPException(status_code=400, detail="Inactive user")
    return current_user

# 路由
@app.post("/token", response_model=Token)
async def login_for_access_token(form_data: OAuth2PasswordRequestForm = Depends()):
    user = authenticate_user(fake_users_db, form_data.username, form_data.password)
    if not user:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Incorrect username or password",
            headers={"WWW-Authenticate": "Bearer"},
        )
    access_token_expires = timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES)
    access_token = create_access_token(
        data={"sub": user.username}, expires_delta=access_token_expires
    )
    return {"access_token": access_token, "token_type": "bearer"}

@app.get("/users/me/", response_model=User)
async def read_users_me(current_user: User = Depends(get_current_active_user)):
    return current_user

@app.post("/users/", response_model=User)
async def create_user(user: UserCreate):
    if user.username in fake_users_db:
        raise HTTPException(status_code=400, detail="Username already registered")
    hashed_password = get_password_hash(user.password)
    user_dict = user.dict()
    user_dict["hashed_password"] = hashed_password
    del user_dict["password"]
    fake_users_db[user.username] = user_dict
    return User(**user_dict)

@app.post("/items/", response_model=Item, status_code=201)
async def create_item(
    item: ItemCreate,
    current_user: User = Depends(get_current_active_user)
):
    item_dict = item.dict()
    item_id = len(fake_items_db) + 1
    db_item = Item(
        **item_dict,
        id=item_id,
        owner_id=current_user.username,
        created_at=datetime.now()
    )
    fake_items_db.append(db_item)
    return db_item

@app.get("/items/", response_model=List[Item])
async def read_items(
    skip: int = 0,
    limit: int = 10,
    current_user: User = Depends(get_current_active_user)
):
    return fake_items_db[skip : skip + limit]

@app.get("/items/{item_id}", response_model=Item)
async def read_item(
    item_id: int,
    current_user: User = Depends(get_current_active_user)
):
    for item in fake_items_db:
        if item.id == item_id:
            if item.owner_id != current_user.username:
                raise HTTPException(status_code=403, detail="Not enough permissions")
            return item
    raise HTTPException(status_code=404, detail="Item not found")

@app.delete("/items/{item_id}", status_code=204)
async def delete_item(
    item_id: int,
    current_user: User = Depends(get_current_active_user)
):
    for i, item in enumerate(fake_items_db):
        if item.id == item_id:
            if item.owner_id != current_user.username:
                raise HTTPException(status_code=403, detail="Not enough permissions")
            fake_items_db.pop(i)
            return
    raise HTTPException(status_code=404, detail="Item not found")

# 运行命令: uvicorn main:app --reload
# 文档地址: http://127.0.0.1:8000/docs
```

## 5. 官方注意事项 / 最佳实践

1. **性能优化**
   - 使用异步函数（async def）提高并发性能
   - 合理使用响应模型减少数据传输
   - 使用依赖注入避免代码重复
   - 数据库操作使用异步驱动

2. **安全实践**
   - 使用 OAuth2 和 JWT 进行身份验证
   - 密码使用 bcrypt 等强加密算法
   - 验证所有输入数据
   - 使用 HTTPS 生产环境

3. **代码组织**
   - 按功能模块组织代码
   - 使用 Pydantic 模型定义数据结构
   - 分离路由、模型、业务逻辑
   - 使用依赖注入管理组件

4. **文档与测试**
   - 利用自动生成的 Swagger UI 和 ReDoc
   - 为每个路径操作添加描述和标签
   - 编写单元测试和集成测试
   - 使用 pytest 进行测试

## 6. 官方文档参考链接

- 官方文档：https://fastapi.tiangolo.com/
- 官方教程：https://fastapi.tiangolo.com/tutorial/
- Pydantic 文档：https://pydantic-docs.helpmanual.io/
- Starlette 文档：https://www.starlette.io/
