# astrbot_plugin_openai_responses

这是一个为 AstrBot 提供 **OpenAI Responses API**（`/v1/responses`）支持的插件。

插件会注册一个新的 Provider `type`：`openai_responses_plugin`。

## 安装

1. 将本插件仓库安装到 AstrBot（插件市场 / WebUI / 指令均可）。
2. 安装或更新后，建议 **重启 AstrBot**。

## 配置（WebUI，推荐）

### 1) 新增 Provider Source

- `id`：自定义唯一 ID（例如 `openai_responses_p`）
- 类型：`OpenAI (Responses, Plugin)`（即 `type=openai_responses_plugin`）
- `key`：OpenAI API Key（支持多个）
- `api_base`：默认 `https://api.openai.com/v1`

可选常用项：
- `timeout`：超时秒数（默认 `120`）
- `proxy`：代理地址（如 `http://127.0.0.1:7890`）
- `reasoning_effort`：`low/medium/high/xhigh`
- `prompt_cache_retention`：可选 `in_memory` 或 `24h`。默认留空，表示不显式发送该参数，交由上游使用默认缓存策略。
- `log_prompt_cache`：是否输出缓存命中摘要日志。开启后会记录 `hit`、`input_cached`、`input_other`、`output` 等字段，便于观察是否命中缓存降费。

其余配置项（如 `codex_*`、`tool_fallback_*`）建议保持默认，通常无需手动调整。

### 2) 新增服务提供商（Provider）

- `provider_source_id`：填写上一步的 `id`
- `model`：例如 `gpt-4o-mini` 或 `gpt-5.3-codex`
- `id`：自定义（例如 `openai_responses_p/gpt-4o-mini`）

最后将 `provider_settings.default_provider_id` 指向你刚创建的 provider `id`。

## 使用建议

- 若你主要是日常聊天/工具调用，默认配置即可。
- 需要更显式地控制 OpenAI Prompt Caching 时，可按需设置 `prompt_cache_retention`。
- `24h` 属于显式 opt-in，会改变缓存保留时长语义；若你对数据保留、Zero Data Retention 或代理兼容性敏感，建议先在测试环境验证。
- 若要确认是否真正命中缓存并减少计费，可开启 `log_prompt_cache`，观察日志中的 `hit` 与 `input_cached`。
- 使用 Codex 系列模型时，建议不要关闭 `codex_*` 相关默认开关。
- 若更新插件后行为异常，优先重启 AstrBot 再重试。

## 更新日志

- 详细变更记录见 `CHANGELOG.md`。
- 版本号使用语义化版本规范（SemVer）。

## 开发与测试（开发者可选）

> 建议在 AstrBot 运行环境（或 AstrBot 仓库 checkout）中运行本插件测试，以确保 `astrbot` 依赖可用。

### 安装开发依赖

```bash
python3 -m venv .venv
.venv/bin/python -m pip install -r requirements-dev.txt
```

### 运行单元测试

```bash
python3 -m pytest -q
```
