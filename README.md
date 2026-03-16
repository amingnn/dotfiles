### 使用一条命令设置一台新机器

```shell
# 常规环境（保留 chezmoi，后续继续管理与更新）  
sh -c "$(curl -fsLS get.chezmoi.io)" -- init --apply <GitHub用户名>

# 一次性环境（完成后清理 chezmoi 相关痕迹）
sh -c "$(curl -fsLS get.chezmoi.io)" -- init --one-shot <GitHub用户名>
```

### 拉取远程最新版本

```shell
chezmoi update
```


