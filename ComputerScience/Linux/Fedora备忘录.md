# 前置准备
1. 安装界面不要点击启用第三方软件源（会卡住好一会儿），等“软件”应用刷新完数据后，会自动弹出是否启用第三方软件源。
2. 更新系统与软件
	```bash
	sudo dnf upgrade --refresh
	```
3. 启用 RPM Fusion 仓库
	```bash
	sudo dnf install \
	https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm \
	https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
	```
4. 添加 Flatpak 仓库
	```bash
	sudo flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
	```
5. 安装 extensions manager & gnome tweaks
	- 命令：
		```bash
		flatpak install flathub com.mattjakeman.ExtensionManager
		sudo dnf install gnome-tweaks
		```
	- 备注：如何不能安装插件管理器，说明第三方软件源没有启用
6. 安装中文字体
	- 命令：
		```bash
		v sudo dnf install wqy-zenhei-fonts
		v sudo dnf install google-noto-fonts-common
		sudo dnf install adobe-source-han-sans-cn-fonts
		sudo dnf install google-noto-sans-cjk-fonts -y
		```
	- 备注：Noto Sans CJK，支持简繁中文，覆盖广泛。
# 软件安装
## Goolge Chrome
- 命令：
	```bash
	sudo dnf install google-chrome-stable
	```
- 注意：如果不能安装，需要启用 Chrome 仓库
	```bash
	sudo dnf install fedora-workstation-repositories
	sudo dnf config-manager setopt google-chrome.enabled=1
	```
## VScode
1. 安装密钥和 yum 存储库
	```bash
	sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
	echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\nautorefresh=1\ntype=rpm-md\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" | sudo tee /etc/yum.repos.d/vscode.repo > /dev/null
	```
2. 更新包缓存并安装
	```bash
	dnf check-update
	sudo dnf install code
	```
## Obsidian
- 命令：
	```bash
	flatpak install flathub md.obsidian.Obsidian
	```
- 备注：由 Obsidian 社区维护
## Cursor
## Idea & DataGrip
- 备注：
	- 官网下载 tar.gz 压缩包，配合破解脚本使用
	- 在起始界面可以选择安装桌面图标
## ApiFox
```bash
flatpak install flathub com.apifox.Apifox
```
## Telegram
```bash
flatpak install flathub org.telegram.desktop
```
## Vlc
```bash
sudo dnf install vlc
```
## WechatDevtools
- Appimage 安装
- 地址：https://github.com/msojocs/wechat-web-devtools-linux
## steam

# 环境安装
1. 安装 Nodejs
	```bash
	sudo dnf install -y nodejs
	```
2. 安装 Git
	- 全局配置
		```bash
		git config --global user.name "你的用户名"
		git config --global user.email "你的邮箱@example.com"
		```
	- 设置令牌或者 SSH
		```bash
		git config --global credential.helper store
		```
	- 配置代理
		```bash
		git config --global http.proxy http://proxy_address:port
		git config --global https.proxy https://proxy_address:port
		```
	- 注意：我使用 HTTPS 令牌，需要在 Github 申请令牌，对私有仓库操作时，第一次需要输入令牌，以后会保存到本地（~/.git-credentials）
3. 安装 Java
	- dnf 没有提供 21 以下版本，需要自己下载压缩包，一般解压到 usr/lib/jvm 里
	- 编程工具提供选择 JDK 的功能，不设置环境变量
	- Fedora 默认安装 openjdk-21（java-21-openjdk）,但只提供 JRE（通过 javac 命令验证），还需下载 JDK（java-21-openjdk-devel）
	- 可以选择使用 alternatives 管理多个 java 版本
4. 安装 Python
	- Fedora 默认安装 Pyhton，还需下载 pit
5. 安装 Maven
	- 命令：
		```bash
		sudo dnf install maven
		```
	- 备注：
		- 安装路径 usr/share/maven
		- 默认会在 /username/.m2 创建本地仓库，设置 maven 镜像
6. 安装 MySQL
	- 命令：
		```bash
		sudo dnf install community-mysql-server
		```
	- 配置：
		- 启动 mysql，设为开机自启
			```bash
			sudo systemctl start mysqld
			sudo systemctl enable mysqld
			sudo systemctl status mysqld
			```
		- 安全初始化
			```bash
			sudo mysql_secure_installation
			```
	- 备注：
		- 不要选择从 MySQL 官方仓库下，安装时会与 Mariadb 冲突，直接从 dnf 仓库里下载
7. 安装 Fish shell
	- 命令：
		```bash
		sudo dnf install fish
		```
	- 启动并设为默认 shell
		```bash
		fish
		chsh -s /usr/bin/fish
		```
	- 安装 fisher（插件管理）
		```bash
		curl -sL https://raw.githubusercontent.com/jorgebucaran/fisher/main/functions/fisher.fish | source && fisher install jorgebucaran/fisher
		```
	- 安装 starship（终端美化）
		```bash
		curl -sS https://starship.rs/install.sh | sh
		```
		- 启用：在 fish shell 配置文件添加 starship 的配置
			```bash
			nano ~/.config/fish/config.fish
			starship init fish | source
			source ~/.config/fish/config.fish
			```
		- 配置：在 ~/.config/starship.toml 中设置，主题可以设置设置官网的预设主题
	- 切换到fish shell后，为保证jetbrains脚本生效，需要在fish shell的配置文件中加上一些命令：
		```bash
		# 直接通过 bash 加载 JetBrains 环境变量
		if test -f "$HOME/.jetbrains.vmoptions.sh"
		    bash -c 'source ~/.jetbrains.vmoptions.sh && printenv' | grep '_VM_OPTIONS=' | while read -l line
		        set -l parts (string split '=' -m 1 "$line")
		        set -gx $parts[1] $parts[2]
		    end
		end
		```
# 插件安装
1. AppIndicator and KStatusNotifierItem Support
2. ArcMenu
3. Blur my shell
4. Clipboard History
5. Compiz alike magic lamp effct
6. Compiz windows effect
7. Coverflow Alt-Tab
8. Dash to dock
9. Desktop Cube
10. Forge
11. Media Controls
12. Open Bar
13. Quick settings tweaks
14. Search light
15. User Themes
16. Vitals
# 其他事项
1. 必须提前准备好代理工具
2. 字体：
	1. JetBrains mono
	2. JetBrains mono nerd
	3. comicShannsMono Nerd Ront Mono Regular
	4. Comic Mono
	5. Comic Mono Bold
3. Appimage 设置
	- 要创建应用图标，需要在 ~/.local/share/applications 目录下创建 .desktop 文件
	- .desktop 文件模板
		```bash
		[Desktop Entry]
		Name=
		Comment=
		Exec=/home/<你的用户名>/Applications//.AppImage
		Icon=/home/<你的用户名>/Applications//.png
		Terminal=false
		Type=Application
		Categories=Development;
		StartupWMClass=
		```
	- 提取 Appimage 内容
		```bash
		--appimage-extract
		```
	- WMClass
		```bash
		xprop WM_CLASS
		```
4. Tarpermonkey 不运行
	- 开启开发者模式
	- 在插件管理->详情->勾选"允许运行用户脚本"
5. Nvidia 驱动
	- 命令：
		```bash
		sudo dnf install akmod-nvidia
		sudo dnf install xorg-x11-drv-nvidia-cuda
		sudo dnf install nvidia-vaapi-driver
		```
	- 备注：
		- 以型号选用驱动
		- 确保内核版本与驱动版本一致
		- 安装完驱动后，“软件”应用后提示重启，进入软件，记住 Mock 密钥重启后选择等级 Mock，输入密钥。
		- 通过 lsmod | grep nvidia 查看驱动是否加载成功，会替换调默认的 Nvidia 驱动 Nouveau
6. 视频
	- 安装解码器以支持 h264 和 aac
		```bash
		sudo dnf install libavcodec-freeworld
		```
	- 默认安装了 ffmpeg
7. dnf list installed 输出为空，使用 rpm -qa 命令







