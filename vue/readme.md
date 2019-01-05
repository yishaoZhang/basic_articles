## 组件列表列更新问题 ##
+ 受 :key 控制
  + 一个渲染列表, 当数组改变时, key值改变将更新
  + :key="$index" 存在问题
+ 要求必须更新, 可以 :key="Math.random()" 辩证认识

## sockjs-node开发环境一直请求 ##
+ 本地开发环境网络环境发生变化
  + 重新运行下 npm run serve