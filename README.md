# Football Prediction API Mini

基于 FastAPI + MySQL + XGBoost + Poisson + DeepSeek LLM 的足球比赛数据同步与智能预测系统。

系统负责赛事数据采集、数据落库、特征工程、机器学习预测、比分分布建模、LLM 深度分析、预测结果查询与赛后复盘。当前 `mini` 分支保留后端服务、同步任务、预测逻辑、命令行工具和已训练模型文件，不包含前端工程。

<div align="center">

## 🔴 重要说明

<p>
  <strong>数据截止时间：当前已统计并整理至 2026 年 6 月 15 日</strong>
</p>

<p>
  <strong>可视化演示站点：</strong>
  <a href="https://www.ai7zym.online"><strong>www.ai7zym.online</strong></a>
</p>

<p>
  <strong>已完成小组赛全部比赛的数据统计与整理</strong>
</p>

<p>
  <strong>当前世界杯单向预测准确率约 80%，比分方向约 60% 左右</strong>
</p>

<p>
  <strong>数据库原始导出文件体积较大，未上传至 GitHub 仓库；需要原始 SQL 数据的可以联系作者获取，后续会持续更新</strong>
</p>

<p>
  <strong>本项目仅供技术研究和数据分析参考，不构成任何投注建议</strong>
</p>

</div>

## 🔴 项目亮点

| 能力 | 说明 |
|------|------|
| 完整数据闭环 | 从 API-Football 拉取联赛、球队、球员、积分榜、赛程、实时赛况等数据，沉淀到 MySQL 本地数据仓库 |
| 多模型组合预测 | 使用 XGBoost 预测胜平负、大小球和进球期望，再通过 Poisson 生成比分分布 |
| LLM 深度分析 | DeepSeek LLM 综合模型输出、球队背景、比赛上下文，生成更像分析师的结论 |
| 可解释结果 | 输出概率、比分、让球、大小球、置信度、核心数据和深度分析文本 |
| 模型回退机制 | 优先使用联赛专属模型，没有专属模型时回退到赛事组模型或全局模型 |
| API + CLI 双入口 | 可接入前端、机器人、自动化脚本，也可直接用命令行工具预测比赛 |
| 赛后复盘 | 比赛结束并同步比分后，可回填预测命中情况，用于模型表现统计和策略筛选 |

## 演示站点

| 类型 | 地址 |
|------|------|
| 可视化站点 | [https://www.ai7zym.online](https://www.ai7zym.online) |
| 本地 API 文档 | `http://127.0.0.1:8000/docs` |
| 健康检查 | `http://127.0.0.1:8000/api/health` |

## 交流与联系

| QQ 联系方式 | 交流群 | 微信 |
|:---:|:---:|:---:|
| <img src="11.png" width="220" alt="QQ 联系方式" /> | <img src="22.png" width="220" alt="交流群" /> | <img src="33.jpg" width="220" alt="微信" /> |
| 扫码添加 QQ | 扫码加入交流群 | 扫码添加微信 |

---

## 功能概览

### 数据与同步

| 模块 | 说明 |
|------|------|
| 联赛与赛季 | 同步联赛、杯赛、国家、赛季、覆盖范围等基础信息 |
| 球队与球场 | 同步球队信息、国家队标记、Logo、球场资料 |
| 球员数据 | 同步球员基础资料与赛季维度统计 |
| 积分榜 | 同步排名、积分、净胜球、主客场战绩、近期状态 |
| 赛程数据 | 同步比赛时间、状态、轮次、主客队、比分、半全场结果 |
| 实时赛况 | 支持同步进行中比赛的实时状态和比分 |
| 调度任务 | 支持手动触发、启停、修改同步和预测任务 |

### 预测系统

| 模块 | 说明 |
|------|------|
| 特征工程 | 基于近期状态、主客场表现、积分榜、历史交锋、单场统计生成模型特征 |
| XGBoost | 输出胜平负概率、大小球概率、主客队进球期望 |
| Poisson | 基于进球期望生成 Top 3 参考比分 |
| DeepSeek LLM | 输出最终胜负方向、比分、让球、大小球、摘要和深度分析 |
| 批量预测 | 扫描符合条件的比赛并批量生成预测结果 |
| 结果查询 | 统一返回基础信息、模型输出、LLM 输出和赛后命中情况 |
| 赛后回填 | 完赛后回填真实比分与命中结果，便于复盘 |

## 技术栈

### 后端与数据

| 技术 | 说明 |
|------|------|
| FastAPI | 后端 API 服务 |
| Uvicorn / Starlette | ASGI 服务与 Web 框架基础 |
| SQLAlchemy 2.0 | ORM 与数据库访问 |
| Alembic | 数据库迁移 |
| MySQL | 本地数据仓库 |
| Loguru | 日志管理 |

### 模型与分析

| 技术 | 说明 |
|------|------|
| XGBoost | 结构化数据预测模型 |
| scikit-learn | 训练、评估与特征处理 |
| pandas / numpy / scipy | 数据处理与数学计算 |
| joblib | 模型持久化与加载 |
| Poisson | 比分概率分布建模 |
| DeepSeek API | LLM 综合分析 |

### 工具

| 技术 | 说明 |
|------|------|
| Rich | 命令行美化输出 |
| requests / httpx / aiohttp | HTTP 请求 |
| Typer | CLI 工具支持 |

## 系统要求

### 开发环境

- Python 3.12+
- MySQL 8.0+
- 可访问 API-Football 数据源
- 可访问 DeepSeek 兼容 Chat Completions 接口

### 推荐配置

- 最低 2 核 CPU / 4GB 内存
- 推荐 4 核 CPU / 8GB 内存
- 如果重新训练模型，建议准备更高内存和更完整的数据集

## 项目结构

```text
api-football/
├── backend/
│   ├── main.py                  # FastAPI 入口
│   ├── requirements.txt         # Python 依赖
│   ├── .env.example             # 环境变量示例
│   ├── alembic/                 # 数据库迁移脚本
│   ├── app/
│   │   ├── api/                 # REST API 路由
│   │   ├── core/                # 配置、日志、白名单等
│   │   ├── db/                  # SQLAlchemy session
│   │   ├── models/              # ORM 模型
│   │   ├── schemas/             # Pydantic schema
│   │   └── services/            # 同步、调度、预测服务
│   ├── prediction/
│   │   ├── models/              # 已训练模型文件
│   │   ├── features.py          # 特征生成
│   │   ├── predict.py           # 单场预测逻辑
│   │   ├── train.py             # 模型训练入口
│   │   └── training/            # 训练、评估、数据处理
│   └── tools/
│       ├── manual_predict.py    # 交互式手动预测
│       ├── batch_predict.py     # 批量预测
│       ├── sync_fixtures.py     # 赛程同步工具
│       ├── sync_players.py      # 球员同步工具
│       └── async_sync_subdata.py
├── .gitignore
└── README.md
```

### 服务职责

| 服务 | 默认端口 | 说明 |
|------|----------|------|
| FastAPI Backend | 8000 | 数据同步、赛事查询、预测、调度任务和 API 文档 |
| MySQL | 3306 | 本地数据仓库 |

### 架构说明

- `app/api` 负责对外 REST API。
- `app/services` 负责数据同步、调度任务、自动预测和赛后回填。
- `prediction/features.py` 负责构造模型特征。
- `prediction/predict.py` 负责单场预测主流程。
- `prediction/models/` 存放已训练模型。
- `tools/` 提供命令行预测和同步辅助工具。

## 快速开始

### 方式一：使用 SQL 数据快速启动

如果已经拿到作者提供的数据库 SQL 文件，可以直接导入后运行：

```bash
git clone https://github.com/ai7zym/api-football.git
cd api-football/backend
```

创建虚拟环境：

```bash
python -m venv .venv
```

Windows PowerShell:

```powershell
.\.venv\Scripts\Activate.ps1
```

Linux / macOS:

```bash
source .venv/bin/activate
```

安装依赖：

```bash
pip install -r requirements.txt
```

创建数据库：

```sql
CREATE DATABASE football CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

导入 SQL：

```bash
mysql -u root -p football < dump-football-api-202606151047.sql
```

复制配置文件：

```bash
cp .env.example .env
```

Windows PowerShell:

```powershell
Copy-Item .env.example .env
```

修改 `backend/.env` 后启动服务：

```bash
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

### 方式二：从空数据库初始化

如果没有 SQL 文件，可以通过迁移和同步接口自行初始化：

```bash
cd backend
alembic upgrade head
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

然后按顺序触发同步：

```bash
curl -X POST http://127.0.0.1:8000/api/scheduler/leagues/sync
curl -X POST http://127.0.0.1:8000/api/scheduler/teams/sync
curl -X POST http://127.0.0.1:8000/api/scheduler/fixtures/sync
curl -X POST http://127.0.0.1:8000/api/scheduler/standings/sync
curl -X POST http://127.0.0.1:8000/api/scheduler/players/sync
```

## 配置说明

### 环境变量

`backend/.env.example` 示例：

```env
DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=
DB_NAME=football

API_FOOTBALL_KEY=
API_FOOTBALL_BASE_URL=

DEEPSEEK_API_KEY=
DEEPSEEK_BASE_URL=https://api.deepseek.com/v1
DEEPSEEK_MODEL=deepseek-chat

CACHE_TTL=3600
```

| 变量 | 说明 |
|------|------|
| `DB_HOST` / `DB_PORT` | MySQL 地址与端口 |
| `DB_USER` / `DB_PASSWORD` | MySQL 用户名与密码 |
| `DB_NAME` | 数据库名称 |
| `API_FOOTBALL_KEY` | API-Football Key |
| `API_FOOTBALL_BASE_URL` | API-Football 地址，请自行配置 |
| `DEEPSEEK_API_KEY` | DeepSeek API Key |
| `DEEPSEEK_BASE_URL` | DeepSeek API 地址 |
| `DEEPSEEK_MODEL` | LLM 模型名称 |
| `CACHE_TTL` | 缓存时间 |

> `.env` 必须放在 `backend/` 目录下，因为项目默认从当前运行目录读取配置。

## 常用 API

### 基础查询

| 方法 | 路径 | 说明 |
|------|------|------|
| GET | `/api/health` | 健康检查 |
| GET | `/api/leagues` | 联赛列表 |
| GET | `/api/leagues/{league_id}` | 联赛详情 |
| PATCH | `/api/leagues/{league_id}/toggle` | 启用/停用联赛 |
| GET | `/api/teams` | 球队列表 |
| GET | `/api/teams/{team_id}` | 球队详情 |
| GET | `/api/players` | 球员列表 |
| GET | `/api/players/{player_id}` | 球员详情 |
| GET | `/api/standings` | 积分榜 |
| GET | `/api/fixtures` | 比赛列表 |
| GET | `/api/fixtures/{fixture_id}` | 比赛详情 |

### 预测与调度

| 方法 | 路径 | 说明 |
|------|------|------|
| POST | `/api/predict/{fixture_id}` | 执行单场预测 |
| GET | `/api/predictions` | 查询预测结果 |
| PATCH | `/api/fixtures/{fixture_id}/category` | 设置比赛分类 |
| GET | `/api/scheduler/status` | 调度任务状态 |
| POST | `/api/scheduler/{task_id}/trigger` | 手动触发任务 |
| POST | `/api/scheduler/{task_id}/start` | 启动任务 |
| POST | `/api/scheduler/{task_id}/stop` | 停止任务 |
| PATCH | `/api/scheduler/{task_id}` | 修改任务配置 |
| GET | `/api/scheduler/logs` | 调度日志 |

### 调度任务 ID

| task_id | 说明 |
|------|------|
| `league_sync` | 同步联赛与赛季 |
| `team_sync` | 同步球队与球场 |
| `player_sync` | 同步球员 |
| `standing_sync` | 同步积分榜 |
| `fixture_daily` | 同步赛程 |
| `fixture_live` | 同步实时赛况 |
| `auto_predict` | 自动预测 |
| `backfill_pred` | 回填预测结果 |

## 使用示例

查询指定日期比赛：

```bash
curl "http://127.0.0.1:8000/api/fixtures?date=2026-06-13&page=1&page_size=20"
```

执行单场预测：

```bash
curl -X POST http://127.0.0.1:8000/api/predict/123456
```

查询预测结果：

```bash
curl "http://127.0.0.1:8000/api/predictions?date=2026-06-13&page=1&page_size=20"
```

按分类查询预测结果：

```bash
curl "http://127.0.0.1:8000/api/predictions?category=jingzu&page=1&page_size=20"
```

触发赛后回填：

```bash
curl -X POST http://127.0.0.1:8000/api/scheduler/backfill_pred/trigger
```

## 命令行工具

| 命令 | 说明 |
|------|------|
| `python tools/manual_predict.py` | 交互式手动预测 |
| `python tools/batch_predict.py` | 批量预测 |
| `python prediction/features.py` | 重新生成特征 |
| `python prediction/train.py` | 重新训练模型 |

## 预测结果结构

`GET /api/predictions` 返回的数据主要分为四部分：

| 字段 | 说明 |
|------|------|
| `basic` | 比赛、球队、联赛、时间、状态、分类等基础信息 |
| `xgb` | 模型分组、胜平负概率、大小球概率、进球期望、Top 3 比分 |
| `llm` | LLM 给出的胜负、比分、让球、大小球、摘要和深度分析 |
| `result` | 完赛后的实际比分与命中情况，未完赛时通常为 `null` |

## 常见问题

### 预测失败或返回 400？

通常需要检查：

- `fixtures` 表中是否存在对应 `fixture_id`。
- 该比赛是否有足够的球队、联赛、积分榜、近期战绩等数据。
- 对应联赛是否已启用。
- `prediction/models/` 下模型文件是否存在。
- 如果需要 LLM 分析，确认 `DEEPSEEK_API_KEY` 可用。

### 数据库连接失败？

请检查：

- MySQL 服务是否已启动。
- `backend/.env` 中数据库配置是否正确。
- 是否已经创建数据库并执行 `alembic upgrade head`。
- 启动命令是否在 `backend/` 目录下执行。

### 同步不到比赛？

可能原因：

- API Key 无效或额度不足。
- 联赛未启用。
- 查询日期没有比赛。
- 上游数据范围与本地筛选条件不匹配。

### SQL 文件为什么没有放到仓库？

数据库原始导出文件体积较大，不适合直接提交到 GitHub。当前仓库通过 `.gitignore` 忽略 `*.sql` 文件。需要完整 SQL 数据可以联系作者获取。

## 安全说明

- 不要提交 `.env`。
- 不要提交 API Key。
- 不要提交数据库密码。
- 不要提交本地日志文件。
- 不要提交本地虚拟环境。
- 不要提交 IDE 或 Agent 私有配置。
- `*.sql` 已被忽略，避免误提交大体积数据库导出文件。

## 许可证

当前仓库未声明明确 License。使用、二次开发或商用前，请先确认项目授权方式。

## 免责声明

本项目仅供技术学习、数据分析和模型研究使用，不构成任何投注建议。

- 使用者需要自行承担使用风险。
- 请勿用于任何违法违规用途。
- 预测结果存在不确定性，请理性看待模型输出。
- 作者不对使用本系统造成的任何直接或间接后果负责。
