## 常见 ##
+ 在iframe中通过url传参，如果出现json格式的参数，注意JSON格式的双引号不好改变，src取值应单引号

## 传递参数的格式问题 ##
+ 简单请求
  + 请求体header content-type如下:
    + application/x-www-form-urlencoded
    + multipart/form-data
    + text/plain
+ 一般前端会通过 Qs 将参数序列化为 form-data
  + key: value value为基本类型
    + 如果要传对象: 一种思路为 将对象变化为JSON字符
    + 增加后端难度
+ 在实际开发过程中 后端有可能提出以JSON的形式传参
  + 请求体header content-type, 设置为application/json
  + 由简单请求变为复杂请求
  + 增加一次探嗅option请求
  + 一般C端项目是不允许的

## 未解决问题 ##
+ 模块原理
    + qt
    + ulr
    + ...
+ 前后端交互格式
    + form-data
    + json
    + ...
+ 常见取参函数写法
    + 注意格式
        + com?key=value&key=value
        + com?key/:value/key/value