# dotfiles

个人 dotfiles，使用 [GNU Stow](https://www.gnu.org/software/stow/) 管理。

## 目录结构

每个子目录是一个 stow 包，包内路径与 `$HOME` 一一对应：

```
dotfiles/
├── fastfetch/.config/fastfetch/
├── kitty/.config/kitty/
├── niri/.config/niri/
└── noctalia/.local/state/noctalia/
```

| 包 | 链接到 |
| --- | --- |
| `kitty` | `~/.config/kitty` |
| `niri` | `~/.config/niri` |
| `fastfetch` | `~/.config/fastfetch` |
| `noctalia` | `~/.local/state/noctalia` |

## 安装

Arch Linux：

```bash
sudo pacman -S stow
```

Debian/Ubuntu：

```bash
sudo apt install stow
```

## 使用

把仓库克隆到 `$HOME` 下（stow 要求包目录在目标目录内）：

```bash
git clone git@github.com:xiaoxiaoaqaq/dotfiles.git ~/dotfiles
cd ~/dotfiles
stow -v -t ~ kitty niri fastfetch noctalia
```

首次使用前，先备份/移除已存在的同名配置，否则 stow 会报冲突：

```bash
mv ~/.config/kitty ~/.config/kitty.bak
mv ~/.config/niri ~/.config/niri.bak
mv ~/.config/fastfetch ~/.config/fastfetch.bak
mv ~/.local/state/noctalia ~/.local/state/noctalia.bak
```

也可以先用 `--adopt` 把现有文件收进仓库（会覆盖仓库中的同名文件，谨慎）：

```bash
stow -v -t ~ --adopt kitty niri fastfetch
```

常用命令：

```bash
cd ~/dotfiles

# 预演，只看会做什么，不实际执行
stow -n -v -t ~ kitty niri fastfetch noctalia

# 重新链接
stow -R -v -t ~ kitty niri fastfetch noctalia

# 取消链接
stow -D -v -t ~ kitty niri fastfetch noctalia
```

> `noctalia` 包是运行时状态目录（设置、插件缓存等），变动较频繁。如果不想让它出现在 `git status` 里，可以只 stow 其他包，或执行 `stow -D -v -t ~ noctalia` 取消链接。

## 新增配置

1. 在仓库中新建包目录，按相对 `$HOME` 的路径放文件，例如 `~/.config/foo/bar.conf` 对应 `foo/.config/foo/bar.conf`。
2. 执行 `stow -v -t ~ foo` 建立链接。
