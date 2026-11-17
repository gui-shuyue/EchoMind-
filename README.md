# EchoMind

EchoMind 是一个面向客服与运营场景的多 Agent 智能系统。项目以 FastAPI 为服务入口，将意图识别、Agent 路由、RAG 检索、分层记忆、动态 Skills、性能监控与端到端评测组合为一条完整的对话处理链路。

## 核心能力

- 多策略融合的细粒度意图识别
- General、Technical、Billing 与 Escalation 多 Agent 路由
- 基于 ChromaDB 的知识检索与情景记忆
- Redis + ChromaDB 组成的分层记忆体系
- 可动态加载和刷新的业务 Skills
- Prometheus 指标、健康检查与路由降级
- LLM-as-Judge 意图与回复质量评测

## 处理链路

```text
用户请求
  -> 读取工作记忆、情景记忆和用户画像
  -> 识别意图、紧急度与关键实体
  -> 按需检索知识库
  -> 路由到业务 Agent 并注入 Skills
  -> 生成回复并写回记忆
  -> 采集监控与评测指标
```

## 技术栈

- Python 3.11、FastAPI、Uvicorn
- Anthropic SDK 及兼容 Anthropic 协议的模型服务
- Redis、ChromaDB、Prometheus、Nginx
- Docker 与 Docker Compose

## 快速启动

1. 在项目根目录创建 `.env`，至少配置：

   ```env
   ANTHROPIC_API_KEY=your_api_key
   ANTHROPIC_MODEL=claude-3-5-sonnet-20241022
   REDIS_PASSWORD=echomind123
   ```

2. 启动完整服务：

   ```bash
   docker compose up -d --build
   ```

3. 检查运行状态：

   ```bash
   docker compose ps
   docker compose logs -f echomind
   ```

启动后可访问：

- API：`http://localhost:8000`
- Swagger：`http://localhost:8000/docs`
- Health Check：`http://localhost:8000/health`
- Prometheus：`http://localhost:9090`

## 主要接口

| 功能 | 接口 |
|---|---|
| 对话 | `POST /chat` |
| 知识检索 | `POST /search` |
| 新增知识 | `POST /knowledge/add` |
| 上传知识 | `POST /knowledge/upload` |
| Skills 列表 / 刷新 | `GET /skills` / `POST /skills/reload` |
| 运行监控 | `GET /monitor` |
| 质量评测 | `POST /eval/run` |

## 项目结构

```text
api/          FastAPI 服务入口
agents/       Agent 编排与路由
core/         意图识别、模型工具与 Skills 加载
memory/       Redis + ChromaDB 分层记忆
mcp/          知识库与工具管理
monitor/      运行指标和性能监控
evaluation/   端到端质量评测
skills/       可动态加载的业务规则
tests/        核心模块测试
config/       Nginx 与 Prometheus 配置
```

## License

本项目暂未声明开源许可证。
