# todolist-system

# 簡介（Introduction）

一個 Task 列表的 API，可以新增、修改、刪除 Task，並且用戶可以查看 Task 列表。

## features:
- User 管理
  - 註冊
  - 登入
- Task 管理
  - 新增 Task
  - 修改 Task
  - 刪除 Task
  - 查看 Task 列表

# 架構設計（Architecture Design）

| 項目 (Item)                     | 技術選用 (Technology Choice) |
| ------------------------------- | ---------------------------- |
| 開發語言 (Programming Language) | Go                           |
| Architectural pattern           | Microservices                |
| API 框架 (API Framework)        | mux                          |
| API Style                       | RESTful                      |
| RPC Framework                   | gRPC                         |
| 資料庫 (Database)               | MySQL                        |
| 資料庫 ORM (Database ORM)       | Gorm                         |
| 身份驗證 (Authentication)       | JWT                          |

## 架構圖設計

``` mermaid
graph TD
    A[Client] --> B[API Gateway]
    B --> C[User service]
    B --> D[Task service]
    C --> E[ORM]
    D --> E
    E --> F[MySQL]
```

## Microservices

### API Gateway
- 定義與管理API
- 身份驗證與授權

### protobuf
- 定義服務rpc format 

### Users service
- 用戶相關功能實現
  - 登入
  - 註冊

### Task service
- 任務相關功能實現
  - 新增任務
  - 修改任務
  - 刪除任務
  - 查詢任務列表

## API 詳細規格（API Specification）

### API Resource

- `/users`: users 管理
- `/tasks`: tasks 管理

### Users Resource
| Method | Path            | Description |
| ------ | --------------- | ----------- |
| POST   | /users/register | 註冊新用戶  |
| POST   | /users/login    | 用戶登入    |


#### [POST] /users/register - 註冊新用戶
- request  
  - `email`: string , required , An email address
  - `name`: string , required , min 1 characters , max 20 characters
  - `password`: string , required , min 6 characters , max 20 characters , An alphanumeric string
``` json
{
  "email": "test@example.com",
  "name": "test",
  "password": "abc123"
}
```
- response (201 Created)
  - `message`: string , required 
    - `SUCCESS`: 註冊成功

``` json
{
  "message": "SUCCESS"
}
```

- response (400 Bad Request)
  - `message`: string , required 
    - `FAILED`: 格式不符合

``` json
{
  "message": "FAILED"
}
```

#### [POST] /users/login - 用戶登入
- request
  - `email`: string , required
  - `password`: string , required
- response (200 OK)
  - `message`: string , required
  - `token`: string , required
- response (400 Bad Request)
  - `message`: string , required
    - `FAILED`: 格式不符合
- response (401 Unauthorized)
  - `message`: string , required
    - `FAILED`: 登入失敗
- response (403 Forbidden)
  - `message`: string , required
    - `FAILED`: 找不到用戶

### Tasks Resource
| Method | Path              | Description  |
| ------ | ----------------- | ------------ |
| POST   | /tasks/create     | 新增任務     |
| PUT    | /tasks/update/:id | 更新任務     |
| DELETE | /tasks/delete/:id | 刪除任務     |
| GET    | /tasks/list       | 查詢任務列表 |


#### [POST] /tasks/create - 新增任務
- request  
  - `title`: string , required
  - `description`: string , required
  - `status`: string , required , enum: [pending, in_progress, completed]
- response (201 Created)
  - `message`: string , required
- response (400 Bad Request)

#### [PUT] /tasks/update/:id - 更新任務
- request
  - `title`: string , required
  - `description`: string , required
  - `status`: string , required , enum: [pending, in_progress, done]
- response (200 OK)
- response (400 Bad Request)

#### [DELETE] /tasks/delete/:id - 刪除任務  
- request
  - `id`: string , required
- response (200 OK)
- response (400 Bad Request)

#### [GET] /tasks/list - 查詢任務列表
- request
- response (200 OK)
- response (400 Bad Request)

### Common API response format
- 500 Internal Server Error
  - `message`: string , required
    - `SYSTEM_ERROR`: 系統錯誤
- 401 Unauthorized
  - `message`: string , required
    - `TOKEN_EXPIRED`: token 過期
    - `TOKEN_INVALID`: token 無效
- 403 Forbidden
  - `message`: string , required
    - `USAGE_LIMIT_EXCEEDED`: 使用限制超過
    - `UNAUTHORIZED_ACCESS`: 未授權
- 404 Not Found
  - `message`: string , required
    - `RESOURCE_NOT_FOUND`: 找不到資源
- 400 Bad Request
  - `message`: string , required
    - `INVALID_REQUEST`: 請求格式錯誤

## 身份驗證與授權（Authentication & Authorization）

## 數據庫設計（Database Design）
- 資料庫選擇（SQL, NoSQL）
- 資料表設計（ERD、索引策略）
- 交易管理與一致性考量
- 資料備份與恢復機制

## 測試計畫（Testing Plan）
## 部署與 CI/CD（Deployment & CI/CD）
部署方式（Docker, Kubernetes, Serverless）
CI/CD 流程（GitHub Actions, GitLab CI, Drone CI）
灰度發布與回滾機制

## 擴展性與未來計畫（Scalability & Future Roadmap）
如何擴展 API（負載均衡、微服務）
預計添加的功能

# Future Plan
- 請求流量管理（Rate Limiting & Throttling）
-  日誌與監控（Logging & Monitoring）


