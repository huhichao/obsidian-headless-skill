# Obsidian Headless Skill

💎 在 Linux 服务器上使用官方 Obsidian Headless 客户端管理 Obsidian 仓库

## 适用平台

- ✅ **Linux** (主要支持平台)
- ✅ **服务器/无 GUI 环境**
- ❌ Windows/macOS (需要使用官方 Obsidian 桌面应用)

## 简介

本技能提供对 Obsidian Headless 客户端的完整支持，让你可以在没有图形界面的 Linux 服务器上：

- 同步 Obsidian 仓库
- 搜索和读取笔记
- 创建和编辑 Markdown 文件
- 自动化文档管理

## 安装

```bash
npm install -g obsidian-headless
```

命令为 `ob`（不是 `obsidian-headless`）

## 快速开始

### 1. 登录

```bash
# 使用邮箱密码登录
ob login --email your@email.com --password 'yourpass'

# 如果启用了 2FA
ob login --email your@email.com --password 'yourpass' --mfa 123456

# 检查登录状态
ob login
```

### 2. 列出远程仓库

```bash
ob sync-list-remote
```

### 3. 设置同步

```bash
ob sync-setup \
  --vault <vault-id> \
  --path /path/to/local/vault \
  --device-name "Server-Name" \
  --password 'encryption-password'
```

### 4. 启动同步

```bash
# 一次性同步
ob sync --path /path/to/vault

# 持续同步（后台模式）
nohup ob sync --path /path/to/vault --continuous > ~/obsidian-sync.log 2>&1 &
```

## 常用操作

| 命令 | 说明 |
|------|------|
| `ob sync-status --path <path>` | 查看同步状态 |
| `ob sync-list-local` | 列出本地仓库 |
| `ob sync-unlink --path <path>` | 断开仓库同步 |
| `ob logout` | 退出登录 |

## 文件操作

Obsidian 仓库就是普通的 Markdown 文件目录：

```bash
# 读取笔记
cat /path/to/vault/note.md

# 搜索笔记
grep -r "关键词" /path/to/vault --include="*.md"

# 创建笔记
echo "# 新笔记" > /path/to/vault/new.md

# 编辑笔记
vim /path/to/vault/note.md
```

## 故障排除

### "Another sync instance is already running"

```bash
# 1. 停止所有 sync 进程
pkill -9 -f "ob sync"

# 2. 清理锁文件
rm -rf /path/to/vault/.obsidian/.sync.lock

# 3. 重新启动
nohup ob sync --path /path/to/vault --continuous > ~/obsidian-sync.log 2>&1 &
```

### "Failed to validate password"

- 仓库启用了端到端加密
- 必须在 setup 时提供 `--password`
- 密码在 Obsidian 桌面端设置（Settings → Sync → Encryption）

## 系统服务配置

创建 systemd 服务实现开机自启：

```ini
[Unit]
Description=Obsidian Headless Sync
After=network.target

[Service]
Type=simple
User=youruser
ExecStart=/usr/bin/node /path/to/ob sync --path /path/to/vault --continuous
Restart=always

[Install]
WantedBy=multi-user.target
```

## 依赖

- Node.js
- Obsidian Sync 订阅（免费版有限制：1个仓库，1GB存储）

## 相关链接

- [Obsidian 官网](https://obsidian.md)
- [Obsidian Sync](https://obsidian.md/sync)
- [社区论坛](https://forum.obsidian.md)

## License

MIT
