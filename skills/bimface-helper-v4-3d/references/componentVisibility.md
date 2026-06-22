# 构件可见性控制（Component Visibility）

## 使用约束与说明

- 所有方法必须在模型加载完成后调用，否则可能抛出异常或无效
- 所有以 `{ids, all}` 为参数的方法，`ids` 和 `all` 可以同时使用；`all: true` 表示对所有构件生效，`ids` 则限定具体构件
- 通过 objectData 操作构件时，必须先调用 `model.getMatchIds({objectData})` 获取构件 ID 列表，再执行对应操作
-修改可见性后通常不需要手动调用 `viewer3D.render()`，但如果发现画面未更新，可显式调用
- 多模型场景下，需要通过 `viewer3D.getModel(modelId)` 获取指定模型实例

---

## hideComponents —— 隐藏构件

```javascript
// 按 ID 隐藏指定构件
model.hideComponents({ ids: ["componentId1", "componentId2"] });

// 隐藏所有构件
model.hideComponents({ all: true });
```

### ByObjectData 模式

```javascript
model.getMatchIds({ objectData }).then((componentIds) => {
  model.hideComponents({ ids: componentIds });
});
```

---

## showComponents —— 显示构件

```javascript
// 按 ID 显示已隐藏的构件
model.showComponents({ ids: ["componentId1", "componentId2"] });

// 显示所有构件
model.showComponents({ all: true });
```

### ByObjectData 模式

```javascript
model.getMatchIds({ objectData }).then((componentIds) => {
  model.showComponents({ ids: componentIds });
});
```

---

## transparentComponents —— 使构件半透明

```javascript
// 按 ID 将构件设为半透明
model.transparentComponents({ ids: ["componentId1", "componentId2"] });

// 将所有构件设为半透明
model.transparentComponents({ all: true });
```

### ByObjectData 模式

```javascript
model.getMatchIds({ objectData }).then((componentIds) => {
  model.transparentComponents({ ids: componentIds });
});
```

---

## opaqueComponents —— 移除构件半透明

将已半透明的构件恢复为不透明状态：

```javascript
// 按 ID 恢复为不透明
model.opaqueComponents({ ids: ["componentId1", "componentId2"] });

// 恢复所有构件为不透明
model.opaqueComponents({ all: true });
```

### ByObjectData 模式

```javascript
model.getMatchIds({ objectData }).then((componentIds) => {
  model.opaqueComponents({ ids: componentIds });
});
```

---

## setTranslucentColor —— 设置半透明颜色

设置构件在半透明状态下的显示颜色（原 `setTransparentedComponentColor`）：

```javascript
const color = new Glodon.Bimface.Common.Graphics.Color(255, 0, 0, 0.5);
model.setTranslucentColor(color);
```

- 该方法全局生效，不需要传 ids
- alpha 通道控制半透明程度，0 为完全透明，1 为完全不透明

---

## overrideComponentOpacity —— 覆写构件透明度

```javascript
// 将构件的透明度设置为 0.3（0=完全透明，1=完全不透明）
model.overrideComponentOpacity({ ids: ["componentId1", "componentId2"] }, 0.3);
```

### ByObjectData 模式

```javascript
model.getMatchIds({ objectData }).then((componentIds) => {
  model.overrideComponentOpacity({ ids: componentIds }, 0.3);
});
```

---

## restoreComponentOpacity —— 恢复构件透明度

恢复已覆写的构件透明度至原始值：

```javascript
// 按 ID 恢复
model.restoreComponentOpacity({ ids: ["componentId1", "componentId2"] });

// 恢复所有
model.restoreComponentOpacity({ all: true });
```

---

## 完整示例

```javascript
// 完整示例：在 ViewAdded 事件中操作构件可见性
viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ViewAdded, function () {
  model = viewer3D.getModel();

  // 按 ID 隐藏构件
  model.hideComponents({ ids: ["component_123", "component_456"] });

  // 恢复显示
  model.showComponents({ ids: ["component_123"] });

  // 按 objectData 过滤并隐藏
  const objectData = { categoryId: "-2001340", levelName: "F01" };
  model.getMatchIds({ objectData }).then((componentIds) => {
    model.transparentComponents({ ids: componentIds });
  });

  // 设置半透明颜色
  const translucentColor = new Glodon.Bimface.Common.Graphics.Color(0, 255, 0, 0.3);
  model.setTranslucentColor(translucentColor);

  // 覆写并恢复透明度
  model.overrideComponentOpacity({ ids: ["component_123"] }, 0.1);
  model.restoreComponentOpacity({ ids: ["component_123"] });
});
```
