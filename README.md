# HiClaw Docker Compose 部署

一键部署 [HiClaw](https://github.com/hunter2x/hiclaw) 多智能体管理平台。

## 快速开始

```bash
git clone https://github.com/hunter2x/hiclaw-docker-compose.git
cd hiclaw-docker-compose
```

### 1. 修改环境变量

编辑 `.env` 文件，**必须修改**以下两项：

| 变量 | 说明 |
|------|------|
| `HICLAW_LLM_API_KEY` | 你的 LLM API Key |
| `HICLAW_ADMIN_PASSWORD` | 管理员密码（至少8位） |

> 中国大陆用户默认使用阿里云百炼 Token Plan，无需修改 `HICLAW_OPENAI_BASE_URL`。

### 2. 启动

```bash
docker compose up -d
```

### 3. 访问

| 服务 | 地址 |
|------|------|
| **Element Web（聊天）** | http://localhost:18088 |
| **Higress Console** | http://localhost:18001 |

首次登录使用 `.env` 中配置的 `HICLAW_ADMIN_USER` / `HICLAW_ADMIN_PASSWORD`。

## 架构

```
┌──────────────────────────────────────────┐
│            hiclaw-controller             │
│  (Embedded 嵌入式容器)                     │
│                                          │
│  ┌─────────┐ ┌──────────┐ ┌───────────┐ │
│  │ Matrix  │ │  MinIO   │ │  Higress  │ │
│  │ :6167   │ │  :9000   │ │  :8080    │ │
│  └─────────┘ └──────────┘ └───────────┘ │
│                                          │
│  ┌─────────────────┐ ┌────────────────┐ │
│  │ Controller API  │ │ Element Web    │ │
│  │ :8090           │ │ :8088          │ │
│  └─────────────────┘ └────────────────┘ │
└──────────────────────────────────────────┘
         │ 动态创建
         ▼
┌─────────────────┐  ┌─────────────────┐
│ hiclaw-manager  │  │ hiclaw-worker-* │
│ (Manager Agent) │  │ (Worker Agents) │
└─────────────────┘  └─────────────────┘
```

## 文件说明

| 文件 | 用途 |
|------|------|
| `docker-compose.yaml` | 容器编排配置 |
| `.env` | 环境变量（部署前修改） |

## 端口

| 端口 | 服务 |
|------|------|
| 18080 | AI Gateway |
| 18001 | Higress Console |
| 18088 | Element Web |
| 18888 | Manager Console |

## 镜像仓库

- 中国大陆：`registry.cn-hangzhou.aliyuncs.com`（默认）
- 国际：`docker.io`

修改 `.env` 中 `HICLAW_REGISTRY` 即可切换。
