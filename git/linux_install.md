## centOS redHat ##
+ 版本过低
  + yum remove git //卸载
+ 下载安装包
  + wget "URL"
+ 解压
  + tar -zxvf git.gz
+ 进入解压后文件目录
  + cd git
+ 配置git安装路径
  + 路径可以自定义, 但将git添加至bash环境变量需使用该路径
  + ./configure prefix=/usr/local/git/
+ 编译并且安装
  + make && make install
+ git指令添加至bash中
  + vim /etc/profile
  + 修改最后一行
    + export PATH=$PATH:/usr/local/git/bin
+ bash环境变量立即生效
  + source /etc/profile
