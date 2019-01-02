## 目标 ##
> vscode 提示错误, ctrl+s自动格式化
+ vscode 下载eslint
+ 基本的配制:
  + 注意注释部分先后顺序
```
{ 
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    {
      "language": "vue", // 注意项, 监听文件格式
      "autoFix": true
    }
  ],
  "editor.formatOnType": true,
  "eslint.autoFixOnSave": true, // 保存自动格式化
  "editor.detectIndentation": false,
  "editor.tabSize": 2,
  "breadcrumbs.enabled": true,
  "eslint.alwaysShowStatus": true,
}
```

## vscode自动保存问题 ##
+ 除vscode可以通过JSON配置自动保存
  + 某些第三方插件亦可造成自动保存,如 html + css + js等