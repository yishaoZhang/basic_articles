## centOS nginx一般安装过程 ##
+ 通过 yum list installed | grep [name] 检查系统安装
    + autoconf libtool
    + automake make
    + gcc gcc-c++
    + 以上若未安装，则进行相关安装
+ 安装PCRE库 （正则 与 rewrite有关）
    + 通过 ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/ 寻找最新PCRE源码包
    + 命令顺序如下：
        + cd /usr/local/src
        + wget [source url] // 下载
        + tar -zxvf [pcre file name] // 解压文件 
        + cd [pcre 解压后文件夹] 
        + ./configure 
        + make 
        + make install 
+ 安装zlib库
    + http://zlib.net 选择合适包
    + 一般安装过程如下：
        + cd /usr/local/src
        + wget [source url] // download
        + tar -zxvf [zlib file name]
        + cd [zlib 解压后 目录]
        + ./configure
        + make
        + make install
+ 安装ssl
    + cd /usr/local/src
    + wget https://www.openssl.org/source/openssl-1.0.1t.tar.gz
    + tar -zxvf [download file name]

+ 安装nginx
    + cd /usr/local/src
    + wget [nginx download url]
    + tar -zxvf [nginx file name]
    + cd [nginx file name]
    + ./configure --sbin-path=/usr/local/nginx/nginx \
    + --conf-path=/usr/local/nginx/nginx.conf \
    + --pid-path=/usr/local/nginx/nginx.pid \
    + --with-http_ssl_module \
    + --with-pcre=[pcrc install location:/usr/local/src[pcrefileName]] \
    + --with-zlib=[zlib install location:/usr/local/src[zlibfileName]] \
    + --with-openssl=[openssl install location:/usr/local/src[opensslfileName]]
    + make
    + make install

+ nginx 相关操作
    + 启动
        + 查看安装过程中 --sbin-path
            + 例如：--sbin-path=/usr/local/nginx/nginx
        + 进入 /usr/local/nginx
        + ./nginx 即启动
    + 退出
        + 安装时： --sbin-path=/usr/local/nginx/nginx
        + 当前 pwd: /usr/local/nginx
        + ./nginx -s stop
        + 推荐 ./nginx -s quit
    + 重启
        + 安装时： --sbin-path=/usr/local/nginx/nginx
        + 当前 pwd: /usr/local/nginx
+ 访问
    + 一切正常，直接访问 ip 即可出现 welcome to nginx字样
    + 注意防火墙及80端口
        + 打开80端口
            + firewall-cmd --zone=public --add-port=80/tcp --permanent // success
            + firewall-cmd --reload // success