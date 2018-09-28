## 一般用法 ##
+ 双向指定
    + window对象(信息接收方window对象)
        + 父传子
            + iframe中 通过 window.frames
                + iframe中 contentWindow亦可以使用
            + 当前窗体 window.open(), 返回的窗体对象
                + 此必然会打开某个窗口
        + 子传父
            + iframe 中使用top or parent 关键字，找到父级窗口
            + 对于window情况，被打开窗口想主动发起，暂时没有解决办法
                + 但对于接收到一次父信息后，可以使用event.source
    + url(信息接收方window url)
        + 消息对象的url
+ 其它
    + 消息来源合法性
        + 一般在信息接受方要加合法性判断
            + if(event.origin !== "url") return;
    + 通信数据格式
        + 事件名称为'message'
            + 建议采用JSON格式
            + 建议添加eventName字段，以应对不同业务
