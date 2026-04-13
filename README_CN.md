# CPA-X 管理面板（V3.1）

[English](README.md) | 中文

`CPA-X` 是一个给 `CLIProxyAPI / cliproxyapi` 使用的监控与管理面板。  
这份副本已经整理成适合直接发布和部署的版本，适合别人拉下来后按文档直接安装。

支持能力：
- 服务状态、健康检查、资源监控
- 请求统计、Token/费用显示
- 日志查看与清空
- 认证文件查看、Codex 账号扫描
- 模型列表、连接测试、API 测试
- 配置查看、校验、重载
- 更新检查与自动更新
- 三套主题：浅色、羊毛纸、暗色

## 适用环境
- 推荐：Linux + systemd
- Python 3.11+
- 目标机器上已经部署并运行 `CLIProxyAPI / cliproxyapi`
- 面板能访问 CPA 管理接口（默认 `http://127.0.0.1:8317`）

说明：
- Windows 可以运行，但 `systemctl` 相关的服务控制与自动更新能力会受限。
- 如果你要的是“最省事直接装”，优先使用 `scripts/install.sh` 或 `scripts/install.ps1`。

## 最快部署

### Linux
```bash
git clone https://github.com/liuxi8860-ctrl/CPA-XS.git
cd CPA-X

cp .env.example .env
bash scripts/install.sh

# 推荐再自动探测一次路径与服务名
python3 scripts/doctor.py --write-env

# 如 doctor 没写出密钥，请手动补到 .env
nano .env

systemctl restart cliproxy-panel
systemctl status cliproxy-panel --no-pager
```

打开：
```text
http://你的服务器IP:8080
```

### Windows
```powershell
git clone https://github.com/liuxi8860-ctrl/CPA-XS.git
cd CPA-X
copy .env.example .env
powershell -ExecutionPolicy Bypass -File scripts/install.ps1
```

## 必填配置

复制 `.env.example` 为 `.env` 后，至少确认下面这些值是正确的：

- `CLIPROXY_PANEL_CLIPROXY_API_BASE`
- `CLIPROXY_PANEL_CLIPROXY_API_PORT`
- `CLIPROXY_PANEL_MANAGEMENT_KEY`
- `CLIPROXY_PANEL_MODELS_API_KEY`
- `CLIPROXY_PANEL_CLIPROXY_SERVICE`
- `CLIPROXY_PANEL_CLIPROXY_DIR`
- `CLIPROXY_PANEL_CLIPROXY_CONFIG`
- `CLIPROXY_PANEL_CLIPROXY_BINARY`
- `CLIPROXY_PANEL_CLIPROXY_LOG`
- `CLIPROXY_PANEL_AUTH_DIR`

如果你不确定路径，先执行：
```bash
python3 scripts/doctor.py --write-env
```

## Docker 部署

仓库已提供：
- `Dockerfile`
- `docker-compose.yml`
- `.env.docker.example`

最短方式：
```bash
docker compose up -d --build
```

容器模式更适合“监控与查看”，不适合依赖 systemd 的“服务控制/自动更新”。

## 当前默认安全策略

- `.env` 不应提交到仓库
- 主配置写回默认关闭：`CLIPROXY_PANEL_CONFIG_WRITE_ENABLED=false`
- 可选启用面板访问密钥：`CLIPROXY_PANEL_PANEL_ACCESS_KEY`
- 默认绑定 `0.0.0.0`，如果你只想本机访问，请改成 `127.0.0.1`

## 发布前建议

如果你准备直接发 GitHub，建议一起保留这些文件：
- `.env.example`
- `.env.docker.example`
- `scripts/install.sh`
- `scripts/install.ps1`
- `scripts/doctor.py`
- `AI_DEPLOY_CN.md`
- `DEPLOY_QUICKSTART_CN.md`

## 附带文档

- 面向 AI 的部署说明：`AI_DEPLOY_CN.md`
- 人类快速部署清单：`DEPLOY_QUICKSTART_CN.md`
- 当前版本说明：`RELEASE_NOTES_V3.1.md`

## 许可协议

MIT License（见 `LICENSE`）
