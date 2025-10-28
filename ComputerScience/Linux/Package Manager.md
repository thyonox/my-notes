









# homebrew

/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

echo >> /home/thyonox/.config/fish/config.fish
    echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> /home/thyonox/.config/fish/config.fish
    eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"

==> Next steps:
- Run these commands in your terminal to add Homebrew to your PATH:
    echo >> /home/thyonox/.config/fish/config.fish
    echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> /home/thyonox/.config/fish/config.fish
    eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
- Install Homebrew's dependencies if you have sudo access:
    sudo dnf group install development-tools
  For more information, see:
    https://docs.brew.sh/Homebrew-on-Linux
- We recommend that you install GCC:
    brew install gcc
- Run brew help to get started
- Further documentation:
    https://docs.brew.sh


## 常用命令
```bash
brew search <包名>        # 搜索软件
brew info <包名>
brew install <包名>       # 安装软件
brew install <包名>@版本   # 安装指定版本 (如 brew install python@3.11)

brew uninstall <包名>     # 卸载
brew autoremove           # 删除不再需要的依赖
	brew cleanup              # 清理旧版本和缓存

brew update               # 更新 Homebrew 自身 & 包信息
brew outdated             # 查看哪些软件过期
brew upgrade              # 升级所有软件
brew upgrade <包名>       # 升级指定软件

brew list                 # 列出已安装的软件
brew list --versions      # 列出软件及版本
brew info <包名>          # 查看软件详情（安装位置、依赖、版本等）
brew deps <包名>          # 查看依赖

brew services list                   # 查看服务
brew services start <包名>           # 启动服务
brew services stop <包名>            # 停止服务
brew services restart <包名>         # 重启服务

brew install --cask <包名>   # macOS 上安装 GUI 应用


```