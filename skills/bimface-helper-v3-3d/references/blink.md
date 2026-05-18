# 构件强调效果

## 使用约束与说明
- 必须在`ViewAdded`事件触发后、`model3D`可用时才能调用
- 启用/关闭强调后需调用 `viewer3D.render()` 刷新场景

## 开启强调效果

```javascript
// 添加需要强调的构件，可以是构件ID、外部构件ID或房间ID
model3D.addBlinkComponentsById(["134052", "150980", "room1", "room2", extObjId]);

// 设置强调颜色（支持十六进制颜色和透明度）
model3D.setBlinkColor(new Glodon.Web.Graphics.Color("#32D3A6", 0.8));

// 启用强调效果
viewer3D.enableBlinkComponents(true);

// 设置闪烁间隔时间（单位：毫秒）
model3D.setBlinkIntervalTime(500);

// 渲染场景
viewer3D.render();
```

## 关闭强调效果

```javascript
// 清除所有强调构件
model3D.clearAllBlinkComponents();

// 渲染场景
viewer3D.render();
```
