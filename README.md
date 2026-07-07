# ACPs Agent Starter

这个项目用于给已有 Agent 快速加上 ACPs Leader 能力。

它只保留两种接入方式：

- `skills/acps`：Agent Skill，适合能读文件、执行脚本的 Agent。
- `mcp_server`：MCP 服务，适合支持 MCP 的 Agent 或平台。

本项目不再提供单独的 Agent 后端、聊天接口或前端页面。

## 能做什么

- 通过 ADP 发现合适的 Partner Agent。
- 通过 AIP 给 Partner Agent 创建任务。
- 查询任务状态。
- 在 Partner 需要用户输入时继续任务。
- 在用户确认后完成任务，或取消任务。
- 可选使用 mTLS 证书访问 HTTPS Partner。

## 目录

```text
skills/acps/     # 可复制到 Agent 工作区的 Skill
mcp_server/      # 可独立启动的 MCP Server
```

## 用法一：Skill

```bash
cd skills/acps
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp state/config/config.yaml.example state/config/config.yaml
cp .env.example .env   # 可选
```

然后让你的 Agent 读取 `skills/acps/SKILL.md`，按里面的流程调用 `scripts/` 下的脚本。

常用脚本：

```text
discover.py
start_task.py
get_task.py
continue_task.py
complete_task.py
cancel_task.py
```

简单测试：

```bash
python scripts/discover.py --query "chess game" --limit 3
```

## 用法二：MCP Server

```bash
cd mcp_server
cp .env.example .env
cp state/config/config.yaml.example state/config/config.yaml
bash start.sh
```

默认 SSE 地址：

```text
http://localhost:7004/sse
```

停止：

```bash
bash stop.sh
```

MCP 工具：

```text
discover
start_task
get_task
continue_task
complete_task
cancel_task
```

## 配置

发现服务配置：

```yaml
default_discovery_url: "https://ioa.pub/discovery/acps-adp-v2/discover"
custom_discovery_url: ""
```

如果要改发现服务地址，编辑：

```text
skills/acps/state/config/config.yaml
mcp_server/state/config/config.yaml
```

Leader AIC 和 mTLS 配置可以写在 `.env` 里：

```env
LEADER_AIC=

MTLS_BASE_DIR=
MTLS_CERT_DIR=certs
MTLS_KEY_DIR=private
MTLS_TRUST_BUNDLE=trust-bundle.pem
```

默认证书路径：

```text
skills/acps/certs/
skills/acps/private/
mcp_server/certs/
mcp_server/private/
```

## 参考

ACPs 相关协议说明参考本地文档：

```text
acps-project/acps-specs
```
