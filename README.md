# codeg-aur

[codeg](https://github.com/xintaofei/codeg)（多 Agent AI 编码工作台）的 Arch Linux 打包仓库。

## 关于（About）

**codeg**（Code Generation）是一个多 Agent AI 编码工作台：在一个桌面应用里统一运行并聚合 Claude Code、Codex、OpenCode、Gemini CLI 等 AI 编码代理的会话，支持主代理向子代理委派任务、多代理协作、MCP/Skills 管理、git worktree 并行开发等。由 [xintaofei/codeg](https://github.com/xintaofei/codeg) 开发，Apache-2.0 许可。

**codeg-aur** 是其 Arch Linux 打包仓库，提供两种安装方式：

| 包 | 类型 | 特点 |
|---|---|---|
| `codeg-git` | 源码构建 | 本地编译，跟随上游 main 最新提交，链接 Arch 官方 webkit2gtk |
| `codeg-bin` | 预编译 | 复用官方发布构建产物，免编译即装即用 |

> 背景：官方分发的 AppImage 存在 WebKit 打包缺陷（捆绑库的 libexec 路径错误、缺少辅助进程），在 Arch 系系统上启动即崩溃。本仓库的源码构建版链接系统 webkit2gtk-4.1，彻底绕开该问题。

## 安装方式（二选一）

### 方式 1：预编译版 codeg-bin（推荐，免编译）

```bash
# 下载安装包后安装：
sudo pacman -U https://github.com/dongliang/codeg-aur/releases/latest/download/codeg-git-0.23.5.r2077.gaa0c4d6-1-x86_64.pkg.tar.zst

# 或克隆仓库后本地安装：
git clone https://github.com/dongliang/codeg-aur.git
cd codeg-aur/codeg-bin
makepkg -si
```

### 方式 2：源码版 codeg-git（跟随上游 main 最新提交）

```bash
git clone https://github.com/dongliang/codeg-aur.git
cd codeg-aur/codeg-git
makepkg -si
```

首次编译约 10-30 分钟（Rust + 前端），之后增量编译很快。

### 方式 3：自定义 pacman 仓库（可选，自动更新）

在 `/etc/pacman.conf` 末尾添加：

```
[codeg-aur]
Server = https://github.com/dongliang/codeg-aur/releases/latest/download
SigLevel = Optional
```

然后：

```bash
sudo pacman -Syu
sudo pacman -S codeg-git
```

## 说明

- 两个包互斥（`conflicts`），同一台机器只能装其一
- `codeg-bin` 是 `codeg-git` 预编译产物，内容一致
- 许可证：Apache-2.0（随包附带 LICENSE）
- 若 AUR 恢复注册，本包亦会同步发布到 AUR
