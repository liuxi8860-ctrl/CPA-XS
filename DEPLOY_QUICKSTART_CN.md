# CPA-X V3.1 快速部署清单

## 1. 适合谁

适合已经在服务器上运行了 `CLIProxyAPI / cliproxyapi`，现在只想快速把 `CPA-X` 面板装上去的人。

## 2. Linux 最快步骤

```bash
git clone https://github.com/liuxi8860-ctrl/CPA-XS.git
cd CPA-X
cp .env.example .env
bash scripts/install.sh
python3 scripts/doctor.py --write-env
```

然后编辑 `.env`，至少补齐：

- `CLIPROXY_PANEL_MANAGEMENT_KEY`
- `CLIPROXY_PANEL_MODELS_API_KEY`

如果 doctor 没自动识别出来，还要手动确认：

- `CLIPROXY_PANEL_CLIPROXY_SERVICE`
- `CLIPROXY_PANEL_CLIPROXY_DIR`
- `CLIPROXY_PANEL_CLIPROXY_CONFIG`
- `CLIPROXY_PANEL_CLIPROXY_BINARY`
- `CLIPROXY_PANEL_CLIPROXY_LOG`
- `CLIPROXY_PANEL_AUTH_DIR`

最后执行：

```bash
systemctl restart cliproxy-panel
systemctl status cliproxy-panel --no-pager
```

访问：

```text
http://服务器IP:8080
```

## 3. Windows 最快步骤

```powershell
git clone https://github.com/liuxi8860-ctrl/CPA-XS.git
cd CPA-X
copy .env.example .env
powershell -ExecutionPolicy Bypass -File scripts/install.ps1
```

## 4. Docker 最快步骤

```bash
docker compose up -d --build
```

说明：

- Docker 更适合监控、查看、读配置、看日志
- 不适合依赖 `systemctl` 的服务控制和自动更新

## 5. 部署完成后的验收

至少检查这几项：

1. 首页能打开
2. `/api/status` 能返回正常 JSON
3. 服务状态、请求统计、Token 数都能显示
4. 模型列表能加载
5. 日志区不是空白报错

## 6. 常见问题

### 页面能打开但没数据

优先检查：

- `CLIPROXY_PANEL_CLIPROXY_API_BASE`
- `CLIPROXY_PANEL_CLIPROXY_API_PORT`
- `CLIPROXY_PANEL_MANAGEMENT_KEY`

### 能看到页面但不能控制服务

通常是：

- 不是 Linux
- 没有 systemd
- `CLIPROXY_PANEL_CLIPROXY_SERVICE` 写错了

### 配置不能直接改

默认就是关闭的。只有你明确需要时，才把：

```text
CLIPROXY_PANEL_CONFIG_WRITE_ENABLED=true
```

## 7. 发布建议

如果你要直接发给别人或上传 GitHub，建议保留：

- `README.md`
- `README_CN.md`
- `DEPLOY_QUICKSTART_CN.md`
- `.env.example`
- `.env.docker.example`
- `scripts/install.sh`
- `scripts/install.ps1`
- `scripts/doctor.py`
