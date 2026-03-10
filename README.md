# Go Handler — 基础 Go 语言能力测试

基于 EdgeOne Pages 的 Go Cloud Functions，通过**文件系统路由**验证 Go 语言在 Serverless 场景下的基础能力。前后端全栈一体化，无需独立后端服务。

## 项目结构

```
go-handler/
├── index.html                          # 前端测试面板（全栈一体化，相对路径请求）
└── cloud-functions/                    # Go 云函数（文件路径 = 路由路径）
    ├── hello.go                        # GET /hello
    ├── health.go                       # GET /health
    ├── echo.go                         # ANY /echo
    └── api/
        ├── users.go                    # GET /api/users
        ├── [id].go                     # GET /api/:id
        ├── files/
        │   └── [[path]].go            # GET /api/files/*
        ├── products/
        │   └── [productId]/reviews/
        │       └── [reviewId].go      # GET /api/products/:pid/reviews/:rid
        └── users/
            └── [userId]/posts/
                └── [postId]/comments/
                    └── [commentId].go # GET /api/users/:uid/posts/:pid/comments/:cid
```

## 测试的 Go 语言能力

| 能力 | 对应路由 | 说明 |
|------|---------|------|
| `net/http` Handler | 全部 | 标准 `http.HandlerFunc` 签名 |
| JSON 序列化 | 全部 | `encoding/json` + `json.NewEncoder` |
| 运行时信息 | `/hello` | `runtime.Version()` 获取 Go 版本 |
| 时间处理 | `/health` | `time.Now()` + RFC3339 格式化 |
| IO 读取 | `/echo` | `io.ReadAll` 读取请求 Body |
| Header/Query 遍历 | `/echo` | `r.Header`、`r.URL.Query()` 遍历 |
| 字符串分割 | `/api/:id` | `strings.Split` 提取路径参数 |
| 切片与复合字面量 | `/api/users` | `[]map[string]interface{}` 构建数据 |
| 字符串前缀处理 | `/api/files/*` | `strings.HasPrefix` + `strings.TrimPrefix` |
| 多级路径解析 | 多动态参数路由 | 按索引提取多个路径段 |

## 路由模式覆盖

每种路由模式保留一个代表性示例：

- **静态路由** — `hello.go` → `/hello`
- **嵌套静态路由** — `api/users.go` → `/api/users`
- **单动态参数** — `api/[id].go` → `/api/:id`
- **双动态参数** — `api/products/[productId]/reviews/[reviewId].go`
- **三层动态参数** — `api/users/[userId]/posts/[postId]/comments/[commentId].go`
- **Catch-All** — `api/files/[[path]].go` → `/api/files/*`

