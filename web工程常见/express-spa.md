## express布署vue spa ##
+ 通过ssh 远程登录, 关闭cli 进程关闭的问题?
  + 借用pm2
  + 文件更新pm2 不重启?
    + linux进程关闭
      + ps -ef | grep 'node'
      + kill 'processID'v
+ vue spa使用history模式时,刷新问题?
  + express使用 'connect-history-api-fallback'
    + npm install connect-history-api-fallback --save
    + var history = require('connect-history-api-fallback')
    + var app = express()
    + app.use(history())
      // 该行需写在 app.use(express.static(path.join(__dirname, 'public'))); 之前