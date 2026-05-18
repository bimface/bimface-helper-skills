# 模型内嵌入图纸

## 使用约束与说明
- 使用 `DrawingHelper` 在3D模型中嵌入Revit图纸（平面图、剖面图、立面图）
- 必须在`ViewAdded`事件触发后才能获取图纸列表和嵌入图纸
- `addDrawingsById` 是异步操作，结果在回调函数中处理

## 初始化

```javascript
const helperConfig = new Glodon.Bimface.Plugins.RevitHelpers.DrawingHelperConfig();
helperConfig.viewer = viewer3D;
const helper = new Glodon.Bimface.Plugins.RevitHelpers.DrawingHelper(helperConfig);
```

## 获取图纸列表

```javascript
const drawingList = helper.getDrawingList();
```

## 嵌入图纸

```javascript
// 按ID嵌入图纸，offset为偏移距离
helper.addDrawingsById(['图纸ID'], 1200, function (data) {
    helper.setDrawingsOpacityById(['图纸ID'], 0.5);
});
```

## 显示/隐藏与透明度控制

```javascript
helper.showDrawingsById(['图纸ID']);
helper.hideDrawingsById(['图纸ID']);
helper.setDrawingsOpacityById(['图纸ID'], 0.5);
```
