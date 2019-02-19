## 安装node ##
+ 编译好的文件
  + tar -xvf node-v6.10.0-linux-x64.tar.xz // node file name
    + cd node-v6.10.0-linux-x64.tar.xz/bin
    + ./node -v // 查看是否可用
  + 设置全局 // window 环境变量
    + ln -s [安装路径/解压路径+'/bin/node'] /usr/local/bin/node
    + ln -s [安装路径/解压路径+'/bin/npm'] /usr/local/bin/npm
  + 测试验证
    + node -v
    + npm -v

## npm global config ##
+ npm config get prefix // 查看配置
+ mkdir ~/.npm-global //创建
+ npm config set prefix '~/.npm-global' // 重新指向
+ export PATH=~/.npm-global/bin:$PATH // 设置全局路径
  + 此处拼写有误, ~是本账号相对路径? 改成绝对就行, 如下
  + export PATH=/root/.npm-global/bin:$PATH
+ source ~/.profile // 更新全局配制
  + 机子不同, 一般在 /etc/profile