# 免费无限使用 ChatGPT Copilot 高级模型教程

基于 YouTube 视频 `sLjTilvm2RQ` 的字幕整理

## 概述

本教程介绍如何通过 GitHub 账号的免费额度，结合自动化脚本和代理 API，实现 ChatGPT Copilot 高级模型（o1、o3、CodeX 等）的免费无限使用。

**原理**：GitHub 账号注册的 ChatGPT 免费账号有少量额度，通过自动化脚本批量注册 GitHub 账号并生成 Token，再将 Token 上传到第三方代理 API，该 API 支持标准 OpenAI 接口格式，配置后即可在各个平台使用高级模型。

## 前置要求

- 一台可以访问 GitHub 的远程 VPS（或服务器）
- 已安装 Docker 和 Docker Compose
- 已安装 Python 和 npm（用于运行注册脚本）
- 基本的命令行操作能力

## 步骤一：部署 API 代理

1. 在 VPS 上创建工作目录，例如 `Claude-Proxy`：
   ```bash
   mkdir Claude-Proxy && cd Claude-Proxy
   ```

2. 从 GitHub 项目（如 `CURL/ProxyAPI` 的 fork）获取配置文件。根据教程，执行以下命令生成配置文件：
   ```bash
   # 具体命令需从项目文档复制
   curl -sS https://raw.githubusercontent.com/.../Auto > Auto
   chmod +x Auto
   ./Auto
   ```
   这会生成 `config.yaml` 和 `docker-compose.yaml` 两个文件。

3. 修改 `config.yaml`：
   - 找到 `management_api` 部分，将 `allow_remote` 改为 `true`
   - 设置一个 `secret_key`（管理后台的密钥）
   - 保存修改

4. 启动 Docker 容器：
   ```bash
   docker-compose up -d
   ```

5. 访问管理后台：`http://<你的服务器IP>:8317/management.html`
   - 输入 `secret_key` 登录
   - 如果端口被占用，可在 `docker-compose.yaml` 中修改前端映射端口（后面端口不变，前面换一个）

## 步骤二：生成 GitHub Token 认证文件

1. 获取批量注册脚本（如 `OpenAI-regest.py`）。教程提到该脚本会使用临时邮箱无限注册 GitHub 账号并生成 Token。

2. 确保服务器已安装 Python 依赖：
   ```bash
   npm install -g yarn   # 或者 pip install -r requirements.txt
   ```

3. 运行脚本：
   ```bash
   python OpenAI-regest.py
   ```
   脚本大约每 26 秒注册一个账号，成功后自动保存 Token 到文件（如 `tokens.json`）。

4. 定期从服务器下载生成的 Token 文件到本地：
   - 在脚本运行目录中找到生成的 Token 文件
   - 下载保存备用

## 步骤三：上传 Token 到代理 API

1. 在管理后台的「认证文件」页面，点击「上传文件」按钮。
2. 选择本地的 Token 文件（如 `tokens.json`）上传。
3. 上传后界面会显示上次更新时间，表示 Token 已生效并可用。

## 步骤四：配置 API 密钥和模型

1. 在「配置面板」->「API 密钥」列表中，可以复制默认密钥或添加新密钥。
2. 在「管理模型」页面，添加模型：
   - 选择上面配置的认证文件（来源）
   - 选择模型名称（如 `gpt-4o`, `o1-preview`, `o3`, `codex-mini` 等）
   - 保存
3. 之后在支持自定义 API 的客户端中，使用该密钥和模型名称即可调用。

## 步骤五：使用 API

代理 API 提供标准 OpenAI 接口格式。配置完成后，可以在 OpenRouter、各种 AI 客户端等平台中使用。

示例请求：
```bash
curl http://<服务器IP>:8317/v1/chat/completions \
  -H "Authorization: Bearer <你的API密钥>" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-4o",
    "messages": [{"role": "user", "content": "Hello"}]
  }'
```

## 注意事项

- ⚠️ 本教程方法可能违反 GitHub、OpenAI 的服务条款，请谨慎使用，仅用于学习研究。
- 自动化注册脚本可能触发风控导致封禁，建议控制频率。
- 代理 API 安全性需自行保障，不要暴露在公网。
- 免费额度不稳定，不保证长期可用。
- 视频中提到的项目（CURL/ProxyAPI）及注册脚本需自行从作者渠道获取（如 Telegram 频道）。

## 参考视频

- **主教程视频** (默认新窗口观看)：
  <a href="https://www.youtube.com/watch?v=sLjTilvm2RQ" target="_blank">https://www.youtube.com/watch?v=sLjTilvm2RQ</a>
- **补充视频** (默认新窗口观看)：
  <a href="https://www.youtube.com/watch?v=JVcUJoWbsLs" target="_blank">https://www.youtube.com/watch?v=JVcUJoWbsLs</a>

## 资源链接

- 字幕文件：[youtube-subtitle-20260301-104643.srt](./youtube-subtitle-20260301-104643.srt)
- 代理 API 项目：需从视频描述或作者频道获取
- 注册脚本：需从视频描述或 Telegram 频道获取

---

*本教程由 AI 根据视频字幕自动整理生成，如有疑问请参考原视频。*
