## MAC系统如何生成ssh ##
+ 检查 SSH key 是否存在(避免重写)
    + ls -al ~/.ssh
+ 生成新的SSH
    + ssh-keygen -t rsa -C "your registed github account"
    + 两次回车确认
        + 保存路径
        + enter passphrase
+ 添加key 到 ssh
    + ssh-add ~/.ssh/id_rsa
+ 复制本地公钥，添加至github（多数本地gitlab不支持ssh）
    + 复制
        + vim ~/.ssh/id_rsa.pub //打开后手动复制
        + pbcopy < ~/.ssh/id_rsa.pub //copy by command
    + 登录github
        + setting --> SSH keys --> Add SSH key
+ 检测 SSH key
    + ssh "your github account"
        + 出现 You've successfully authenticated. 即可