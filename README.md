# CLIProxyAPI 用量统计面板

[English](README.en.md) | 简体中文

CLIProxyAPI 用量统计面板是一个独立的浏览器静态页面，用于从 CLIProxyAPI 管理接口查看用量统计。

CLIProxyAPI 用量统计面板是一个独立的浏览器静态页面，用于查看 `codex-token-usage` 插件统计的用量数据。

这个页面是单文件静态 HTML 应用。它通过 CLIProxyAPI 管理接口读取插件在服务端 SQLite 中持久化的用量统计，按 key、认证账号和模型展示汇总（含成本、缓存命中率、平均延迟），并在页面内累积请求明细。

## 功能

- 读取 `GET /v0/management/plugins/codex-token-usage/summary?window=<all|today|24h|7d|30d>&limit=2000`，工具栏可切换时间范围（全部/今天/最近 24 小时/最近 7 天/最近 30 天）
- 按 key 展示服务端统计：请求数、输入/输出 token、缓存命中率、成本、平均延迟、最近活跃
- 按认证账号和模型展示服务端聚合统计
- 使用明细列表展示近期请求（含思考等级），支持账号/模型筛选和搜索
- 可在脚本开头的 `KEY_ALIASES` 表中为 key 配置别名（按 key 末 4 位匹配），显示为「别名 (掩码key)」，备注显示在悬停提示中
- 读取不删除服务端数据；明细在页面标签页内累积保留
- 作为静态页面运行，不需要额外后端服务

## 使用要求

- 已启用管理 API 的 CLIProxyAPI
- 服务端已安装并启用 `codex-token-usage` 插件
- 浏览器可以访问你的 CLIProxyAPI 管理接口

页面默认使用的管理接口地址：

```text
http://127.0.0.1:8317/v0/management
```

## 使用方法

1. 下载或克隆这个仓库。
2. 在浏览器中打开 `usage.html`。
3. 输入你的 CLIProxyAPI 管理 API 地址。
4. 输入你的 Management key。
5. 点击刷新统计，从插件读取服务端数据。

也可以直接打开 `static/usage.html`。

## 重要说明

- 仓库中不包含 Management key。使用时请在浏览器中输入你自己的密钥。
- 统计汇总读取自插件的服务端数据库，读取不会删除数据，可随时重新刷新。
- 请求明细接口每次只返回最近若干条，页面会在当前标签页内累积；关闭标签页会丢失明细，汇总不受影响。
- `KEY_ALIASES` 别名表随仓库一起公开，请勿填入敏感映射。
- 如果浏览器阻止 `file://` 请求，可以用任意静态 Web 服务器托管这个目录，再打开本地 URL。


## 安全

不要提交你的 Management key 或其他 CLIProxyAPI 私有凭据。本项目默认让 Management key 输入框保持为空。

## 许可证

MIT
