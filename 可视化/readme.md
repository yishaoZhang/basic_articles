## dodge is not support linear attribute ##
+ color 字段需设置为type类型
```
chart.scale('time', {
        type: 'cat'
      })
```

## 柱状图使用自定义颜色 ##
```
chart.interval().position('time*value').color('name', ['#67c23a', '#f56c6c']).opacity(1).adjust([{
        type: 'dodge',
        marginRatio: 1 / 32
      }])
```