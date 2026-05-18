# 线框颜色

## 使用约束与说明
- 必须在`ViewAdded`事件触发后、`model3D`可用时才能调用
- 设置/恢复线框颜色后需调用 `viewer3D.render()` 刷新场景
- `*ByObjectData` 系列方法的筛选条件格式需参照 [构件筛选](filter.md)

## 设置所有线框颜色

```javascript
const color = new Glodon.Web.Graphics.Color(128, 128, 128, 1);

// 设置所有构件线框颜色
model3D.setWireframeColor(color);
viewer3D.render();

// 恢复所有构件线框颜色为默认
model3D.restoreWireframeColor();
viewer3D.render();
```

## 按构件ID设置线框颜色

```javascript
const componentIds = ["267327", "268067", "271431", "272632", "276388"];
const color = new Glodon.Web.Graphics.Color(0, 0, 255, 1);

// 设置指定构件线框颜色
model3D.overrideComponentsFrameColorById(componentIds, color);
viewer3D.render();

// 恢复指定构件线框颜色
model3D.restoreComponentsFrameColorById(componentIds);
viewer3D.render();
```

## 按筛选条件设置线框颜色

```javascript
const filter = [
    { family: "window 3" },
    { family: "双扇推拉门5" },
    { family: "四扇推拉门 2" },
    { family: "固定" }
];
const color = new Glodon.Web.Graphics.Color(0, 255, 0, 1);

// 设置符合筛选条件的构件线框颜色
model3D.overrideComponentsFrameColorByObjectData(filter, color);
viewer3D.render();

// 恢复符合筛选条件的构件线框颜色
model3D.restoreComponentsFrameColorByObjectData(filter);
viewer3D.render();
```
