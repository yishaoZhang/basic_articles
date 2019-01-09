## 组件列表列更新问题 ##
+ 受 :key 控制
  + 一个渲染列表, 当数组改变时, key值改变将更新
  + :key="$index" 存在问题
+ 要求必须更新, 可以 :key="Math.random()" 辩证认识

## sockjs-node开发环境一直请求 ##
+ 本地开发环境网络环境发生变化
  + 重新运行下 npm run serve
## computed Array ##
+ array 遍历, 修改每一个元素, 要求通过computed处理后的数据
  + 该方法不合理, array 元素改变都想要触发一次computed
  + 该方法,可能无法实现, 修改array元素级别的时间粒度, 比computed周期更低
  + 同时注意,computed监视深or浅???
  + 合理方法
    + 将array复制
    + 依次修改array元素
    + 将array指向改变(将array副本重新赋值给array)
    + 触发一次computed
  
## import静态字典 ##
+ 某些情况需要静态字典, import引用后, 无法在项用直接使用
  + ?? 为何? 组件周期 + 引用异步?
+ 使用computed可以使用
+ 使用require?
  + 为何组件需要用require来引用??
  + 大部分组件用import?

## element upload data传参 ##
+ :data="extraParams"
  + 需写在action 之后? 后端报参数异常