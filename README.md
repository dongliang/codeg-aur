# codeg-aur

[codeg](https://github.com/xintaofei/codeg)（多 Agent AI 编码工作台）的 Arch Linux 打包仓库。

**维护者**：dongliang <dongliang17@gmail.com>

## 关于（About）

**codeg**（Code Generation）是一个多 Agent AI 编码工作台：在一个桌面应用里统一运行并聚合 Claude Code、Codex、OpenCode、Gemini CLI 等 AI 编码代理的会话，支持主代理向子代理委派任务、多代理协作、MCP/Skills 管理、git worktree 并行开发等。由 [xintaofei/codeg](https://github.com/xintaofei/codeg) 开发，Apache-2.0 许可。

**codeg-aur** 是其 Arch Linux 打包仓库，提供两种安装方式：

| 包 | 类型 | 特点 |
|---|---|---|
| `codeg-bin` | 预编译 | 云端自动构建，即装即用，`pacman -Syu` 自动更新 |
| `codeg-git` | 源码构建 | 本地编译，跟随上游 main 最新提交，链接 Arch 官方 webkit2gtk |

> 背景：官方分发的 AppImage 存在 WebKit 打包缺陷（捆绑库的 libexec 路径错误、缺少辅助进程），在 Arch 系系统上启动即崩溃。本仓库的源码构建版链接系统 webkit2gtk-4.1，彻底绕开该问题。

## 安装方式

### 方式 1：自定义 pacman 仓库（推荐，自动更新）

在 `/etc/pacman.conf` 末尾添加：

```
[codeg-aur]
Server = https://dongliang.github.io/codeg-aur/$arch
SigLevel = PackageOptional
```

然后执行：

```bash
sudo pacman -Syu
sudo pacman -S codeg-bin
```

之后上游更新时，仓库会自动重新构建，`sudo pacman -Syu` 即可升级。

> 说明：本仓库为未签名仓库（`SigLevel = PackageOptional`），经 HTTPS 分发。所有包均由上游 Apache-2.0 源码构建，非官方仓库，使用风险自负。

### 方式 2：预编译包直接安装

```bash
sudo pacman -U https://github.com/dongliang/codeg-aur/releases/latest/download/codeg-bin-0.23.5.r2077.gaa0c4d6-1-x86_64.pkg.tar.zst
```

所有历史版本保存在 [Releases](https://github.com/dongliang/codeg-aur/releases) 页面。

### 方式 3：源码构建（跟随上游 main 最新提交）

```bash
git clone https://github.com/dongliang/codeg-aur.git
cd codeg-aur/codeg-git
makepkg -si
```

首次编译约 10-30 分钟（Rust + 前端），之后增量编译很快。

## 自动化说明

- 本仓库通过 [GitHub Actions](https://github.com/dongliang/codeg-aur/actions) 每 6 小时检查上游更新
- 检测到上游 main 分支有新提交时，自动在云端 Arch 容器中编译并发布：
  - 更新自定义仓库（gh-pages 分支，只保留最新版本）
  - 生成新 GitHub Release（所有历史版本保留）
  - 同步更新 `codeg-bin` PKGBUILD 版本号与校验和
- 手动触发：仓库 Actions 页 → build → Run workflow

## 包说明

- `codeg-bin` 与 `codeg-git` 互斥（`conflicts`），同一台机器只能装其一
- `codeg-bin` 是 `codeg-git` 的预编译产物，内容一致
- 许可证：Apache-2.0（随包附带 LICENSE）
- 若 AUR 恢复注册，`codeg-git` 亦会同步发布到 AUR
