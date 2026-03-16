---
类型: 软件
名称: chezmoi
分类: 配置管理
平台: [mac, windows, linux]
官网: https://www.chezmoi.io/
下载: https://www.chezmoi.io/install/
文档: https://www.chezmoi.io/user-guide/command-overview/
状态: 使用中
更新日期: 2026-03-16
---

## 简述

安全地在多台机器上管理 dotfiles。

## 安装/下载

- [[Homebrew]]（mac, linux，window）：`brew install chezmoi`
- 更多下载详见：<https://www.chezmoi.io/install/>

## 初始化（快速安装 / 自定义安装）

我这套 chezmoi 配置在首次执行 `chezmoi init` / `chezmoi apply` 时，会先询问安装方式：

- 快速安装：直接使用默认值（优先读取当前机器的 `git config --global user.name/user.email`，代理使用默认值）。
- 自定义安装：会逐项询问并写入配置（git 姓名、邮箱、代理）。

相关配置模板：`~/.local/share/chezmoi/.chezmoi.yaml.tmpl`  
官方模板/提示函数参考：<https://www.chezmoi.io/docs/reference/templates/>

## 快速上手
### 开始在当前机器使用

用于在当前机器从零开始建立 dotfiles 仓库；已有仓库时参考「[[#在多台机器使用]]」。
```shell
# 初始化本地源仓库
chezmoi init

# 纳入第一个文件
chezmoi add ~/.bashrc

# 编辑源状态
chezmoi edit ~/.bashrc

# 查看差异
chezmoi diff

# 应用变更（打印具体改动）
chezmoi apply -v

# 提示：所有命令都接受 `-v`（打印详细）和 `-n`（试运行，不实际执行）
```

提交到远端仓库（示例：GitHub）
```shell
# 进入源目录
chezmoi cd

# 提交到 git
git add .
git commit -m "Initial commit"

# 推送到远端
git remote add origin git@github.com:<GitHub用户名>/dotfiles.git
git branch -M main
git push -u origin main
```
### 在多台机器使用

已安装 chezmoi 时，从 dotfiles 仓库拉取并应用：
```shell
# 初始化（从仓库拉取）
chezmoi init https://github.com/<GitHub用户名>/dotfiles.git

# 查看差异
chezmoi diff

# 应用到主目录（打印具体改动）
chezmoi apply -v

# 后续同步（拉取并应用）
chezmoi update -v
```
### 使用一条命令设置一台新机器

在未安装 chezmoi 的情况下，直接安装并拉取 dotfiles：
```shell
# 常规环境（保留 chezmoi，后续继续管理与更新）  
sh -c "$(curl -fsLS get.chezmoi.io)" -- init --apply <GitHub用户名>

# 一次性环境（完成后清理 chezmoi 相关痕迹）
sh -c "$(curl -fsLS get.chezmoi.io)" -- init --one-shot <GitHub用户名>
```
### 更多
- 完整命令：`chezmoi help`
- 快速入门：<https://www.chezmoi.io/quick-start/>
- 详细文档：<https://www.chezmoi.io/docs/>

## 遇到的问题或有用的？

#### 日常维护



`chezmoi add <file>`: 新增需要管理的文件
`chezmoi edit <file>` 编辑需要管理的文件
`chezmoi apply -v` 应用并输出详细
`chezmoi diff` 查看差异

#### 关于工作模式的问题

概念：
- 源目录：`~/.local/share/chezmoi`（源状态 + git 仓库）
- 配置目录：`~/.config/chezmoi`（配置与敏感信息的管理入口）
- 真实目录：被纳入管理的文件（例如 `~/.zshrc`）

流程：
![[Pasted image 20260316163611.png]]

注意
- `edit` 改的是“源目录”，`apply` 才会写入真实目录。
- 如果直接编辑真实目录文件（例如 `~/.zshrc`），需要重新纳入：`chezmoi add ~/.zshrc` 或 `chezmoi re-add`

#### 添加配置文件的流程

以下均基于源目录的相对路径

oh my zsh需要配置`.chezmoiexternal.toml`使用，详见 [包含来自其他位置的文件](https://www.chezmoi.io/user-guide/include-files-from-elsewhere/#use-git-submodules-in-your-source-directory)

uv 和 brew 在迁移配置前下载使用`run_once_xxx.sh`脚本安装,详见[使用脚本进行操作](https://www.chezmoi.io/user-guide/use-scripts-to-perform-actions/)

brew的软件使用声明式`.chezmoidata/packages.yaml`安装，详见 [声明式安装](https://www.chezmoi.io/user-guide/advanced/install-packages-declaratively/)



