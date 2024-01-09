# apt 换源
```bash
# 备份原文件
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
# 编辑文件，加入新源
vim /etc/apt/sources.list
# 更新系统源
sudo apt update
```
# ZSH
## 安装 ZSH 本体与 Oh My ZSH
```bash
sudo apt install zsh
sh -c "$(wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"
```
## 插件
### zsh-autosuggestions
```shell
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```
### zsh-syntax-highlighting
```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```
### autojump
```bash
# .下载插件autojump到/.oh-my-zsh/custom目录中
git clone git@github.com:wting/autojump.git $ZSH_CUSTOM/plugins/autojump
# .到目录autojump中
cd $ZSH_CUSTOM/plugins/autojump
# 执行install.py
./install.py
# 在.zshrc中加入下行
echo "[[ -s ~/.autojump/etc/profile.d/autojump.sh ]] && . ~/.autojump/etc/profile.d/autojump.sh" >> ~/.zshrc
```
## .zshrc
### Alias
```bash
alias zshconfig="nvim ~/.zshrc"
alias apt="sudo apt"
alias ohmyzsh="nvim ~/.oh-my-zsh"
```
