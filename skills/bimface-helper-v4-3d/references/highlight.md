# 构件高亮（Highlight）

## 使用约束与说明

- 高亮操作必须在模型加载完成后调用
- `highlight` 是视觉强调效果，不改变构件的实际颜色（区别于 `overrideComponentColor`）
- 通过 objectData 操作时，必须先异步调用 `model.getMatchIds({objectData})` 获取构件 ID
- `clearOverrides()` 会清除**所有**覆写效果，包括颜色、透明度、高亮
- 高亮是增量式的：多次 highlight 会叠加显示

---

## 高亮 vs 颜色覆写 的区别

| 特性 | highlight | overrideComponentColor |
|------|-----------|----------------------|
| 视觉效果 | 外发光/描边强调 | 替换构件材质颜色 |
| 清除方式 | `clearHighlight` | `restoreComponentColor` |
| 是否改变原色 | 否（叠加高亮层） | 是（直接替换颜色） |
| 全局清除 | `clearOverrides()` 也会清除 | `clearOverrideComponentColor()` 或 `clearOverrides()` |

---

## highlight —— 高亮构件

### 高亮指定构件

```javascript
model.highlight({ ids: ["componentId1", "componentId2"] });
```

### 高亮所有构件

```javascript
model.highlight({ all: true });
```

---

## clearHighlight —— 清除高亮

### 清除指定构件的高亮

```javascript
model.clearHighlight({ ids: ["componentId1", "componentId2"] });
```

### 清除所有高亮

```javascript
model.clearHighlight({ all: true });
```

---

## clearOverrides —— 清除所有覆写

一次性清除所有视觉效果覆写（包括高亮、颜色覆写、透明度覆写）：

```javascript
model.clearOverrides();
```

---

## ByObjectData 模式

```javascript
const objectData = { levelName: "F02", categoryId: "-2001340" };

model.getMatchIds({ objectData }).then((componentIds) => {
  // 高亮匹配到的构件
  model.highlight({ ids: componentIds });
});

// 清除时也需要先匹配再操作
model.getMatchIds({ objectData }).then((componentIds) => {
  model.clearHighlight({ ids: componentIds });
});
```

---

## 完整示例

```javascript
const viewer3D = new Glodon.Bimface.Viewer.Viewer3D(viewer3DConfig);
viewer3D.addModel(viewToken);
const model = viewer3D.getModel();

viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ModelLoaded, () => {
  // === 高亮操作 ===

  // 1. 高亮指定构件
  document.getElementById("btnHighlightIds").addEventListener("click", () => {
    model.highlight({ ids: ["component_100", "component_200", "component_300"] });
  });

  // 2. 高亮所有构件
  document.getElementById("btnHighlightAll").addEventListener("click", () => {
    model.highlight({ all: true });
  });

  // 3. 通过 ObjectData 匹配后高亮
  document.getElementById("btnHighlightByFloor").addEventListener("click", () => {
    const objectData = { levelName: "F01" };
    model.getMatchIds({ objectData }).then((componentIds) => {
      console.log(`匹配到 ${componentIds.length} 个构件`);
      model.highlight({ ids: componentIds });
    });
  });

  // === 清除高亮 ===

  // 4. 清除指定构件的高亮
  document.getElementById("btnClearHighlightIds").addEventListener("click", () => {
    model.clearHighlight({ ids: ["component_100"] });
  });

  // 5. 清除所有高亮
  document.getElementById("btnClearAllHighlights").addEventListener("click", () => {
    model.clearHighlight({ all: true });
  });

  // 6. 清除所有覆写（包括高亮、颜色、透明度）
  document.getElementById("btnClearAllOverrides").addEventListener("click", () => {
    model.clearOverrides();
  });

  // === 高亮 + 颜色覆写对比演示 ===

  // 先用颜色覆写标记一组构件
  document.getElementById("btnColorOverride").addEventListener("click", () => {
    const redColor = new Glodon.Bimface.Common.Graphics.Color(255, 0, 0, 0.5);
    model.overrideComponentColor({ ids: ["component_400", "component_500"] }, redColor);
  });

  // 再用高亮标记另一组构件（两组效果同时可见）
  document.getElementById("btnHighlightOther").addEventListener("click", () => {
    model.highlight({ ids: ["component_600", "component_700"] });
  });

  // 恢复颜色覆写的构件（高亮不受影响）
  document.getElementById("btnRestoreColor").addEventListener("click", () => {
    model.restoreComponentColor({ ids: ["component_400", "component_500"] });
  });

  // 一键清除所有效果
  document.getElementById("btnClearAll").addEventListener("click", () => {
    model.clearOverrides();
    // 等价于同时：
    // model.clearHighlight({ all: true });
    // model.clearOverrideComponentColor();
  });
});
```

---

## 注意事项

- `highlight` 和 `overrideComponentColor` 可以同时存在且互不影响
- `clearHighlight` 只清除高亮，不会恢复被 `overrideComponentColor` 覆写的颜色
- `restoreComponentColor` 只恢复颜色，不会清除高亮
- `clearOverrides()` 是一键清除所有效果的方法，适合"重置"场景
- 多次调用 `highlight({ ids: [...] })` 会累积高亮（不会自动取消之前的）
