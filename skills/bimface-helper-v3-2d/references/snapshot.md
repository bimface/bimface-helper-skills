# 截图

## 使用约束与说明
- 必须在图纸 `Loaded` 事件触发后才能调用截图方法
- 截图结果在回调函数中异步返回

## 生成图纸截图

```javascript
// 生成当前图纸视图的截图
const bgColor = new Glodon.Web.Graphics.Color("#FFFFFF", 1);
viewer2D.createSnapshotAsync(bgColor, function(base64String) {
    // base64String 为截图的BASE64编码字符串
    console.log("截图生成成功", base64String);

    // 示例：将截图显示在页面上
    const img = document.createElement('img');
    img.src = base64String;
    document.body.appendChild(img);
});
```
