# 构件隔离（Component Isolation）

## 使用约束与说明

- 隔离操作必须在模型加载完成后调用
- 隔离是将选定构件单独高亮显示，其余构件半透明或隐藏
- 隔离后可能需要调用 `viewer3D.render()` 刷新画面
- 通过 objectData 操作时，必须先异步调用 `model.getMatchIds({objectData})` 获取构件 ID

---

## IsolateOption —— 隔离选项枚举

```javascript
// 仅有一个隔离选项：将其他构件设为半透明
Glodon.Bimface.Viewer.IsolateOption.MakeOthersTranslucent
```

---

## isolateComponents —— 隔离构件

```javascript
const state = Glodon.Bimface.Viewer.IsolateOption.MakeOthersTranslucent;

// 按 ID 隔离构件
model.isolateComponents({ ids: ["componentId1", "componentId2"] }, state);
```

### ByObjectData 模式

```javascript
const state = Glodon.Bimface.Viewer.IsolateOption.MakeOthersTranslucent;

model.getMatchIds({ objectData }).then((componentIds) => {
  model.isolateComponents({ ids: componentIds }, state);
});
```

---

## clearIsolation —— 取消隔离

```javascript
model.clearIsolation();
```

取消隔离后，所有构件恢复正常的显示状态。

---

## 完整示例

```javascript
const viewer3D = new Glodon.Bimface.Viewer.Viewer3D(viewer3DConfig);
viewer3D.addModel(viewToken);
const model = viewer3D.getModel();

viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ModelLoaded, () => {
  const isolationState = Glodon.Bimface.Viewer.IsolateOption.MakeOthersTranslucent;

  // 隔离指定构件
  model.isolateComponents({ ids: ["component_123", "component_456"] }, isolationState);

  // 刷新画面以确保隔离效果生效
  viewer3D.render();

  // 3 秒后取消隔离
  setTimeout(() => {
    model.clearIsolation();
  }, 3000);

  // 按 objectData 隔离 —— 隔离 F01 楼层的所有墙
  const objectData = { levelName: "F01", categoryId: "-2001340" };
  model.getMatchIds({ objectData }).then((componentIds) => {
    model.isolateComponents({ ids: componentIds }, isolationState);
  });
});
```
