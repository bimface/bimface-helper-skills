# 构件选择（Component Selection）

## 使用约束与说明

- 所有选择操作必须在模型加载完成后调用
- 选择操作会同时影响构件的视觉高亮状态和选中状态
- 通过 objectData 操作时，必须先异步调用 `model.getMatchIds({objectData})` 获取构件 ID，再执行选择操作
- v4 中不再有 `xxxById` / `xxxByObjectData` 后缀方法，统一使用 `{ids, all}` 参数模式

---

## 方法对照表

| 功能 | v4 方法 | 原 v3 对应方法 |
|------|---------|----------------|
| 获取选中构件 | `model.getSelection()` | `getSelectedComponents()` |
| 添加到选中 | `model.addSelection({ids})` | `addSelectedComponentsById()` |
| 从选中移除 | `model.removeSelection({ids})` | `removeSelectedId()` |
| 清除全部选中 | `model.removeSelection({all: true})` | `clearSelectedComponents()` |

---

## getSelection —— 获取当前选中的构件

```javascript
// 返回当前选中的构件 ID 数组
const selectedIds = model.getSelection();
console.log("当前选中的构件:", selectedIds);
```

---

## addSelection —— 添加构件到选中集

```javascript
// 选中指定构件（已选中的会保持，新指定的会追加）
model.addSelection({ ids: ["componentId1", "componentId2"] });
```

### ByObjectData 模式

```javascript
const objectData = { levelName: "F02", categoryId: "-2001340" };

model.getMatchIds({ objectData }).then((componentIds) => {
  model.addSelection({ ids: componentIds });
});
```

---

## removeSelection —— 移除选中

```javascript
// 取消指定构件的选中状态
model.removeSelection({ ids: ["componentId1", "componentId2"] });

// 清除所有选中
model.removeSelection({ all: true });
```

### ByObjectData 模式

```javascript
model.getMatchIds({ objectData }).then((componentIds) => {
  model.removeSelection({ ids: componentIds });
});
```

---

## 完整示例

```javascript
const viewer3D = new Glodon.Bimface.Viewer.Viewer3D(viewer3DConfig);
viewer3D.addModel(viewToken);
const model = viewer3D.getModel();

viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ModelLoaded, () => {
  // 选中构件
  model.addSelection({ ids: ["component_100", "component_200"] });

  // 获取当前选中列表
  const selected = model.getSelection();
  console.log("已选中:", selected);

  // 取消选中某一个
  model.removeSelection({ ids: ["component_100"] });

  // 通过 objectData 匹配后选中
  const objectData = { levelName: "F01" };
  model.getMatchIds({ objectData }).then((componentIds) => {
    model.addSelection({ ids: componentIds });
  });

  // 清除所有选中
  setTimeout(() => {
    model.removeSelection({ all: true });
  }, 5000);
});
```

---

## 注意事项

- `getSelection()` 是同步方法，直接返回数组
- `addSelection` 是追加式操作，不会自动取消已选中的构件
- 如需替换选中集，先调用 `model.removeSelection({ all: true })` 再调用 `model.addSelection({ids})`
- 选中状态的视觉效果（高亮颜色等）可能受场景设置影响
