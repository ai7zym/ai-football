# Football Prediction API Mini

## 项目状态说明

- 数据截止时间：当前已统计并整理至 2026 年 6 月 15 日。
- 可视化演示站点：[www.ai7zym.online](https://www.ai7zym.online)
- 当前完整目前世界杯单向预测80%准确率，比分大概在60左右。
- 由于数据库原始数据导出数据量巨大，没有上传至仓库，需要原始数据的直接找我拿。后续持续更新
- 数据统计进度：已完成小组赛全部比赛的数据统计与整理。
- 数据库文件：已导出完整 SQL 文件，提供给大家直接使用。
- 使用方式：导入 SQL 文件后，修改 `backend/.env` 中的数据库配置和 API Key，即可启动后端服务。

这是一个面向足球比赛的数据同步、模型预测与智能分析系统。

它不是一个简单的赛程查询接口，也不是只靠人工经验或盘口做判断的小脚本，而是一套完整的足球预测后端：从 API-Football 拉取原始数据，沉淀到 MySQL 本地数据仓库，再通过特征工程、XGBoost、Poisson 比分分布和 DeepSeek LLM 组合生成预测结果。

系统最终可以对指定比赛输出胜平负概率、大小球概率、进球期望、Top 3 参考比分、让球方向、置信度、核心数据依据和深度分析报告。简单说，它把“数据、模型、分析、接口”串成了一条能实际运行的足球预测流水线。

当前仓库为 `mini` 分支，主要保留后端服务、同步任务、预测逻辑和已训练模型文件，不包含前端工程。

## 系统亮点

### 完整的数据闭环

系统不是临时请求一场比赛就结束，而是先建立自己的足球数据底座。联赛、赛季、球队、球员、积分榜、赛程、实时赛况等数据会进入本地 MySQL，后续查询、预测、训练和复盘都围绕这份数据展开。

### 多模型组合预测

预测链路不是单一模型一锤定音，而是多层组合：

- XGBoost 负责结构化概率预测，包括胜平负、大小球、主客队进球期望。
- Poisson 分布负责把进球期望转成参考比分分布。
- DeepSeek LLM 负责综合模型结果、球队背景、比赛上下文，生成更像分析师的最终判断。

### 结果可解释

系统不会只返回一句“主胜”。它会返回一组可分析的数据：

- 主胜、平局、客胜概率。
- 大小 2.5 球概率。
- 主队、客队进球期望。
- Top 3 参考比分。
- LLM 给出的胜负方向、比分、让球、大小球、置信度。
- `brief`、`core_data`、`deep_report` 等分析文本。

这让预测结果可以被前端展示、策略系统消费，也可以用于赛后复盘。

### 支持模型回退

系统可以优先使用联赛专属模型。如果某个联赛没有专属模型，或样本不足，则回退到赛事组模型，再回退到全局模型。这样既能利用热门联赛的专门数据，也能保证冷门赛事仍然有可用预测。

### API 和命令行都能用

你可以把它作为后端 API 接入自己的前端、机器人、脚本或自动化任务，也可以直接在命令行里用 `tools/manual_predict.py` 选择比赛并查看预测结果。

### 可赛后回填命中情况

比赛结束后，同步实际比分即可回填预测命中结果。后续可以用这些数据评估模型表现、筛选策略、优化训练数据。

## 预测流程

```text
API-Football
  |
  v
MySQL local data warehouse
  |
  v
Feature engineering
  |
  v
XGBoost probability prediction
  |
  v
Poisson score distribution
  |
  v
DeepSeek LLM analysis
  |
  v
predictions table
  |
  v
REST API / CLI output
```

## 核心功能

- 数据同步：同步联赛、赛季、球队、球员、积分榜、赛程和实时赛况。
- 本地落库：把 API-Football 数据保存到 MySQL，形成可复用的数据资产。
- 比赛查询：支持按日期、联赛、赛季、球队、状态分页查询比赛。
- 单场预测：对指定 `fixture_id` 执行模型预测与 LLM 分析。
- 批量预测：扫描符合条件的比赛并批量生成预测结果。
- 调度任务：支持手动触发、启动、停止、修改同步和预测任务。
- 结果查询：统一返回比赛基础信息、XGBoost 输出、LLM 输出和赛后命中情况。
- 模型训练：支持基于本地数据重新生成特征并训练模型。
- API 文档：启动后通过 Swagger 直接查看和调试接口。

## 适合场景

- 搭建自己的足球数据仓库。
- 构建足球比赛预测 API。
- 研究结构化数据模型与 LLM 分析结合的预测方式。
- 为前端看板、机器人、自动化脚本提供预测能力。
- 做赛前分析、赛后复盘、模型表现统计和策略筛选。

> 说明：本系统输出仅用于技术研究和数据分析，不构成投注建议。

## 技术栈

- Python 3.12+
- FastAPI / Starlette / Uvicorn
- MySQL / SQLAlchemy / Alembic
- XGBoost / scikit-learn / pandas / numpy / scipy
- API-Football v3
- DeepSeek Chat Completions 兼容接口

## 目录结构

```text
.
|-- backend/
|   |-- main.py                  # FastAPI 入口
|   |-- requirements.txt         # Python 依赖
|   |-- .env.example             # 环境变量示例
|   |-- alembic/                 # 数据库迁移脚本
|   |-- app/
|   |   |-- api/                 # REST API 路由
|   |   |-- core/                # 配置、日志、白名单等
|   |   |-- db/                  # SQLAlchemy session
|   |   |-- models/              # ORM 模型
|   |   |-- schemas/             # Pydantic schema
|   |   `-- services/            # 同步、调度、预测服务
|   |-- prediction/
|   |   |-- models/              # 已训练模型文件
|   |   |-- features.py          # 特征生成
|   |   |-- predict.py           # 单场预测逻辑
|   |   `-- train.py             # 模型训练入口
|   `-- tools/
|       |-- manual_predict.py    # 交互式手动预测
|       |-- batch_predict.py     # 批量预测
|       `-- async_sync_subdata.py
`-- README.md
```

## 快速开始

### 1. 克隆 mini 分支

```bash
git clone -b mini https://gitee.com/myymwl/apifootball.git
cd apifootball/backend
```

### 2. 创建虚拟环境

Windows PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Linux / macOS:

```bash
python -m venv .venv
source .venv/bin/activate
```

### 3. 安装依赖

```bash
pip install -r requirements.txt
```

如果安装 ML 相关依赖较慢，建议使用可访问的 PyPI 镜像源。

### 4. 配置环境变量

复制配置文件：

Windows PowerShell:

```powershell
Copy-Item .env.example .env
```

Linux / macOS:

```bash
cp .env.example .env
```

编辑 `backend/.env`：

```env
DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=
DB_NAME=football

API_FOOTBALL_KEY=
API_FOOTBALL_BASE_URL=https://v3.football.api-sports.io

DEEPSEEK_API_KEY=
DEEPSEEK_BASE_URL=https://api.deepseek.com/v1
DEEPSEEK_MODEL=deepseek-chat

CACHE_TTL=3600
```

说明：

- `API_FOOTBALL_KEY` 用于同步 API-Football 数据。
- `DEEPSEEK_API_KEY` 用于 LLM 预测分析；不配置时，依赖 LLM 的输出可能不可用。
- `.env` 必须放在 `backend/` 目录下，因为配置默认从当前运行目录读取。

### 5. 初始化数据库

创建 MySQL 数据库：

```sql
CREATE DATABASE football CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

如果使用仓库提供的数据库导出文件，可以直接导入：

```bash
mysql -u root -p football < dump-football-api-202606151047.sql
```

导入后只需要确认 `backend/.env` 中的数据库连接信息与本地 MySQL 一致。

执行迁移：

```bash
alembic upgrade head
```

### 6. 启动服务

在 `backend/` 目录下执行：

```bash
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

健康检查：

```bash
curl http://127.0.0.1:8000/api/health
```

Swagger 文档：

```text
http://127.0.0.1:8000/docs
```

## 数据同步流程

建议按以下顺序初始化数据：

```bash
curl -X POST http://127.0.0.1:8000/api/scheduler/leagues/sync
curl -X POST http://127.0.0.1:8000/api/scheduler/teams/sync
curl -X POST http://127.0.0.1:8000/api/scheduler/fixtures/sync
curl -X POST http://127.0.0.1:8000/api/scheduler/standings/sync
curl -X POST http://127.0.0.1:8000/api/scheduler/players/sync
```

实时赛况同步：

```bash
curl -X POST http://127.0.0.1:8000/api/scheduler/fixtures/live
```

通用调度触发接口：

```bash
curl -X POST http://127.0.0.1:8000/api/scheduler/{task_id}/trigger
```

可用 `task_id`：

| task_id | 说明 |
| --- | --- |
| `league_sync` | 同步联赛与赛季 |
| `team_sync` | 同步球队与球场 |
| `player_sync` | 同步球员 |
| `standing_sync` | 同步积分榜 |
| `fixture_daily` | 同步赛程 |
| `fixture_live` | 同步实时赛况 |
| `auto_predict` | 自动预测 |
| `backfill_pred` | 回填预测结果 |

## 常用接口

| 方法 | 路径 | 说明 |
| --- | --- | --- |
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
| PATCH | `/api/fixtures/{fixture_id}/category` | 设置比赛分类 |
| POST | `/api/predict/{fixture_id}` | 执行单场预测 |
| GET | `/api/predictions` | 查询预测结果 |
| GET | `/api/scheduler/status` | 调度任务状态 |
| POST | `/api/scheduler/{task_id}/trigger` | 手动触发任务 |
| POST | `/api/scheduler/{task_id}/start` | 启动任务 |
| POST | `/api/scheduler/{task_id}/stop` | 停止任务 |
| PATCH | `/api/scheduler/{task_id}` | 修改任务配置 |
| GET | `/api/scheduler/logs` | 调度日志 |

## 使用示例

按日期查询比赛：

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

## 命令行工具

在 `backend/` 目录下运行。

交互式手动预测：

```bash
python tools/manual_predict.py
```

批量预测：

```bash
python tools/batch_predict.py
```

重新生成特征并训练模型：

```bash
python prediction/features.py
python prediction/train.py
```

## 预测结果结构

`GET /api/predictions` 返回的数据主要分为四部分：

- `basic`：比赛、球队、联赛、时间、状态、分类等基础信息。
- `xgb`：模型分组、胜平负概率、大小球概率、进球期望、Top 3 比分。
- `llm`：LLM 给出的胜负、比分、让球、大小球、摘要和深度分析。
- `result`：完赛后的实际比分与命中情况；未完赛时通常为 `null`。

## 常见问题

### 预测失败或返回 400

通常需要检查：

- `fixtures` 表中是否存在对应 `fixture_id`。
- 该比赛是否有足够的球队、联赛、积分榜、近期战绩等数据。
- 对应联赛是否已启用。
- `prediction/models/` 下模型文件是否存在。
- 如果需要 LLM 分析，确认 `DEEPSEEK_API_KEY` 可用。

### 数据库连接失败

检查：

- MySQL 服务是否已启动。
- `backend/.env` 中 `DB_HOST`、`DB_PORT`、`DB_USER`、`DB_PASSWORD`、`DB_NAME` 是否正确。
- 是否已经创建数据库并执行 `alembic upgrade head`。
- 启动命令是否在 `backend/` 目录下执行。

### 同步不到比赛

可能原因：

- API-Football key 无效或额度不足。
- 联赛未启用。
- 查询日期没有比赛。
- API-Football 返回的数据范围与本地筛选条件不匹配。

## 安全说明

不要提交以下内容：

- `.env`
- API Key
- 数据库密码
- 日志文件
- 本地虚拟环境
- IDE 或 Agent 私有配置

仓库已在 `.gitignore` 中忽略 `.env`、`.venv/`、`logs/` 等本地文件。

## License

当前仓库未声明明确 License。使用、二次开发或商用前，请先确认项目授权方式。
