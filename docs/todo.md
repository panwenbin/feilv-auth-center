# TODO

## P0：把项目骨架跑起来

- [ ] 初始化 Go 后端目录结构
- [ ] 初始化 PostgreSQL 连接与配置管理
- [ ] 初始化 SvelteKit 前端项目
- [ ] 确定本地开发方式（单机 / docker compose）
- [ ] 建立第一版数据库迁移机制

## P1：账号体系

- [ ] users 表设计
- [ ] user_credentials 表设计
- [ ] 注册接口
- [ ] 登录接口
- [ ] Session 管理
- [ ] 密码重置流程基础

## P2：应用接入

- [ ] applications 表设计
- [ ] application_domains 表设计
- [ ] 创建应用接口
- [ ] 域名 / 回调地址校验
- [ ] 应用基础密钥管理

## P3：商业化基础

- [ ] plans 表设计
- [ ] features 表设计
- [ ] plan_features 表设计
- [ ] user_subscriptions 表设计
- [ ] API Key 表设计
- [ ] License 表设计
- [ ] 基础 audit_logs 表设计

## P4：第一批页面

- [ ] 登录页
- [ ] 注册页
- [ ] dashboard
- [ ] 账户资料页
- [ ] API Key 页面
- [ ] License 页面
- [ ] 订阅页面
- [ ] 管理应用页面

## P5：第一批真实接入

- [ ] 选择首个接入工具
- [ ] 打通统一登录流程
- [ ] 打通至少一个高级功能权限控制
- [ ] 验证一个真实业务闭环

## 后续待定

- [ ] 邮件验证
- [ ] 二步验证
- [ ] 支付接入
- [ ] 审计日志增强
- [ ] SSO 流程标准化
- [ ] 私有部署授权
- [ ] Rust WASM 模块评估
