# 架构草案

## 总体原则

`feilv-auth-center` 在早期阶段采用**单体架构优先**，以减少复杂度、提高开发效率，并确保能快速服务 `fei.lv` 的真实业务场景。

目标不是一开始就做分布式平台，而是先把：

- 用户
- 认证
- 应用接入
- 套餐
- API Key
- License

这些核心领域模型稳定下来。

## 技术栈

### 后端

- Go
- PostgreSQL

### 前端

- SvelteKit

### 增强能力

- Rust WASM（按需）

## 推荐目录结构

```text
.
├── README.md
├── docs/
│   ├── architecture.md
│   ├── mvp.md
│   ├── positioning.md
│   └── roadmap.md
├── backend/
│   ├── cmd/
│   │   └── auth-center/
│   ├── internal/
│   │   ├── app/
│   │   ├── auth/
│   │   ├── user/
│   │   ├── application/
│   │   ├── subscription/
│   │   ├── feature/
│   │   ├── apikey/
│   │   ├── license/
│   │   ├── session/
│   │   ├── audit/
│   │   ├── db/
│   │   └── http/
│   ├── migrations/
│   └── go.mod
├── frontend/
│   ├── src/
│   ├── static/
│   ├── package.json
│   └── svelte.config.js
└── wasm/
    ├── crates/
    └── README.md
```

## 模块划分建议

### 1. user

负责：

- 用户基础信息
- 用户状态
- 用户资料修改

### 2. auth

负责：

- 登录
- 注册
- 密码校验
- 邮箱验证预留
- Token / Session 生成与校验

### 3. session

负责：

- 会话记录
- 登录状态追踪
- SSO 相关基础票据

### 4. application

负责：

- 应用注册
- 应用回调地址
- 域名 / 来源限制
- 应用密钥

### 5. subscription

负责：

- 套餐模型
- 用户订阅关系
- 到期时间
- 状态管理

### 6. feature

负责：

- 功能点定义
- 套餐与功能点关联
- 工具能力开关

### 7. apikey

负责：

- API Key 生成
- 摘要存储
- 启停用
- 使用追踪基础

### 8. license

负责：

- License 生成与校验
- 产品绑定
- 到期时间
- 激活基础字段

### 9. audit

负责：

- 关键行为日志
- 关键授权操作记录

## 部署分层建议

### auth.fei.lv

统一身份与授权入口：

- 登录 / 注册
- 授权确认
- Token / Session 处理

### console.fei.lv

统一控制台：

- 用户资料
- 订阅与套餐
- API Key
- License
- 应用管理（管理员）

### tool subdomains

例如：

- `ssl.fei.lv`
- `regex.fei.lv`
- `json.fei.lv`

这些工具通过 `feilv-auth-center` 完成登录与权限校验。

## 数据库设计原则

### 原则一：核心表先稳定

优先把以下核心表定义稳定：

- users
- sessions
- applications
- plans
- features
- user_subscriptions
- api_keys
- licenses

### 原则二：敏感信息尽量摘要化存储

例如：

- API Key 只保存可检索前缀和哈希摘要
- 密码只保存安全哈希
- 必要时对特定敏感字段加密

### 原则三：为扩展预留，但不过度抽象

比如 license 预留：

- 绑定域名
- 绑定实例
- 激活次数限制

但早期不把所有场景一次性做完。

## SSO 路径建议

### 早期实现

- 中心登录
- 工具侧跳转到 auth center
- 返回短期票据或 token
- 工具服务端向 auth center 验证

### 中期升级

- 规范化授权流程
- 增强应用密钥、回调校验、状态校验
- 必要时向 OAuth2 / OIDC 兼容靠拢

## 前端架构建议

### 控制台优先而不是营销站优先

前端第一阶段重点是：

- 登录流程顺畅
- 控制台信息清晰
- 操作闭环完整

而不是先投入大量精力在视觉系统上。

### 页面建议

- `/login`
- `/register`
- `/dashboard`
- `/account/profile`
- `/account/security`
- `/account/api-keys`
- `/account/licenses`
- `/billing/plan`
- `/admin/applications`

## Rust WASM 的合理使用点

只建议在这些场景考虑引入：

1. 浏览器侧安全处理逻辑
2. 某些高性能编码、签名或校验逻辑
3. 明确能复用到多个工具端的公共模块

不建议把核心业务逻辑放到 WASM 中，以免增加调试与维护成本。

## 演进策略

### 阶段一

单体应用 + 清晰模块

### 阶段二

将高复用能力标准化：

- 鉴权中间件
- 应用接入 SDK
- Feature 校验接口
- License 校验接口

### 阶段三

根据真实流量与复杂度，再决定是否拆分：

- API 服务
- 管理后台
- 授权服务
- 审计服务

在此之前，不建议过早拆服务。
