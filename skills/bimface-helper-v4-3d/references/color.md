# 构件颜色操作（Component Color）

## 使用约束与说明

- 所有颜色操作必须在模型加载完成后调用
- 颜色类必须使用 `Glodon.Bimface.Common.Graphics.Color`，不可使用旧版 `Glodon.Web.Graphics.Color`
- 通过 objectData 操作时，必须先异步调用 `model.getMatchIds({objectData})` 获取构件 ID，再执行颜色操作
- 颜色覆写后可能需要调用 `viewer3D.render()` 以刷新画面

---

## Color 构造

### RGB + Alpha 模式

```javascript
// Glodon.Bimface.Common.Graphics.Color(red, green, blue, alpha)
// RGB 范围 0-255，alpha 范围 0-1

const redColor = new Glodon.Bimface.Common.Graphics.Color(255, 0, 0, 1);
const semiTransparentBlue = new Glodon.Bimface.Common.Graphics.Color(0, 0, 255, 0.5);
```

### Hex + Alpha 模式

```javascript
const pinkColor = new Glodon.Bimface.Common.Graphics.Color("#EE799F", 0.8);
const solidGreen = new Glodon.Bimface.Common.Graphics.Color("#00FF00", 1);
```

---

## overrideComponentColor —— 覆写构件颜色

```javascript
const color = new Glodon.Bimface.Common.Graphics.Color(255, 0, 0, 1);
model.overrideComponentColor({ ids: ["componentId1", "componentId2"] }, color);
```

### ByObjectData 模式

```javascript
const objectData = { categoryId: "-2001340" };
const color = new Glodon.Bimface.Common.Graphics.Color(0, 255, 0, 0.7);

model.getMatchIds({ objectData }).then((componentIds) => {
  model.overrideComponentColor({ ids: componentIds }, color);
});
```

---

## restoreComponentColor —— 恢复构件原始颜色

```javascript
// 恢复指定构件的颜色
model.restoreComponentColor({ ids: ["componentId1", "componentId2"] });
```

---

## clearOverrideComponentColor —— 清除所有颜色覆写

恢复所有被覆写颜色的构件：

```javascript
model.clearOverrideComponentColor();
```

> 如果 `clearOverrideComponentColor` 不可用，可使用 `model.restoreComponentColor({});` 恢复全部。

---

## 完整示例

```javascript
const viewer3D = new Glodon.Bimface.Viewer.Viewer3D(viewer3DConfig);
viewer3D.addModel(viewToken);
const model = viewer3D.getModel();

viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ModelLoaded, () => {
  // 按 ID 给构件上色
  const warningColor = new Glodon.Bimface.Common.Graphics.Color(255, 165, 0, 1);
  model.overrideComponentColor({ ids: ["component_123", "component_456"] }, warningColor);

  // 按 objectData 匹配后上色
  const objectData = { levelName: "F02", categoryId: "-2001340" };
  const highlightColor = new Glodon.Bimface.Common.Graphics.Color("#FF4500", 0.8);

  model.getMatchIds({ objectData }).then((componentIds) => {
    model.overrideComponentColor({ ids: componentIds }, highlightColor);
  });

  // 恢复单个构件颜色
  model.restoreComponentColor({ ids: ["component_123"] });

  // 清除所有颜色覆写
  model.clearOverrideComponentColor();
});
```
