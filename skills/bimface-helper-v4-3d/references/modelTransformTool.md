# 合模工具（ModelTransformTool）

## 使用约束与说明

- 合模工具通过 Widget 模式使用：`app.initializeWidget("ModelTransformTool")` → `app.getWidget("ModelTransformTool")`
- 适用于多模型场景，用于对各个子模型进行独立的位置变换（平移、旋转、缩放）
- 变换数据是 16 元素数组，表示 4×4 变换矩阵（列主序 column-major）
- 必须在所有模型加载完成后使用
- `getModelTransformData()` 返回的是当前所有模型的变换数据，可用于保存和恢复

---

## 初始化

```javascript
// 初始化 Widget
app.initializeWidget("ModelTransformTool");
// 获取工具实例
const tool = app.getWidget("ModelTransformTool");
```

---

## 显隐控制

```javascript
// 显示合模工具
tool.show();

// 隐藏合模工具
tool.hide();
```

---

## 事件

### onHide —— 隐藏事件回调

```javascript
tool.onHide(() => {
  console.log("合模工具已隐藏");
});
```

---

## getModelTransformData —— 获取变换数据

```javascript
// 获取所有模型的当前变换数据
const transformData = tool.getModelTransformData();
console.log("变换数据:", transformData);
// 可将数据保存，用于后续恢复
```

---

## 变换矩阵说明

变换使用 16 元素数组，表示 4×4 矩阵（列主序排列）：

```
列主序 4×4 矩阵对应关系：
[ m0  m4  m8   m12 ]     [ 1  0  0  tx ]
[ m1  m5  m9   m13 ]  ≈  [ 0  1  0  ty ]
[ m2  m6  m10  m14 ]     [ 0  0  1  tz ]
[ m3  m7  m11  m15 ]     [ 0  0  0  1  ]
```

其中 `tx, ty, tz` 为平移分量。

```javascript
// 沿 X 轴平移 40，沿 Z 轴平移 10 的变换矩阵
const transform = [1, 0, 0, 0,  0, 1, 0, 0,  0, 0, 1, 0,  40, 0, 10, 1];
```

---

## setTransformation —— 设置模型变换

每个模型对象通过 `setTransformation()` 设置变换：

```javascript
// 设置模型的变换矩阵
model.setTransformation(transform);
```

---

## 完整示例

```javascript
const viewer3D = new Glodon.Bimface.Viewer.Viewer3D(viewer3DConfig);
const app = viewer3D.getApp();

// 加载多个模型
viewer3D.addModel(viewToken1);
viewer3D.addModel(viewToken2);

viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ModelLoaded, async () => {
  // 获取各个模型对象
  const models = viewer3D.getModelList();
  const model_1 = models[0];
  const model_2 = models[1];

  // 也可以通过 loadModel 异步加载并获取模型对象
  // const model_2 = await viewer3D.loadModel({ viewToken: viewToken2, modelId: "model2" });

  // 对模型 2 设置初始变换：沿 X 轴偏移 40，沿 Z 轴偏移 10
  model_2.setTransformation([1, 0, 0, 0,  0, 1, 0, 0,  0, 0, 1, 0,  40, 0, 10, 1]);

  // 初始化合模工具
  app.initializeWidget("ModelTransformTool");
  const tool = app.getWidget("ModelTransformTool");

  // 监听隐藏事件
  tool.onHide(() => {
    console.log("合模工具已隐藏");
  });

  // 显示合模工具
  tool.show();

  // 保存变换数据
  document.getElementById("btnSaveTransform").addEventListener("click", () => {
    const transformData = tool.getModelTransformData();
    console.log("当前变换数据:", transformData);
    // 可存储到服务端
    localStorage.setItem("modelTransformData", JSON.stringify(transformData));
  });

  // 手动设置更复杂的变换示例
  document.getElementById("btnMoveModel2").addEventListener("click", () => {
    // 沿 X 轴平移 80，Y 轴平移 20，Z 轴平移 -30
    model_2.setTransformation([1, 0, 0, 0,  0, 1, 0, 0,  0, 0, 1, 0,  80, 20, -30, 1]);
  });

  // 缩放示例：将模型沿各轴缩放 2 倍
  document.getElementById("btnScaleModel2").addEventListener("click", () => {
    model_2.setTransformation([2, 0, 0, 0,  0, 2, 0, 0,  0, 0, 2, 0,  0, 0, 0, 1]);
  });
});
```

---

## 注意事项

- 变换矩阵是列主序（column-major），注意与行主序的区别
- 合模工具适合用于多个模型之间的相对位置调整
- `model.setTransformation()` 设置的变换会与合模工具中的调整叠加
- 通过 `getModelTransformData()` 获取的数据可跨会话保存，实现"记住上次合模结果"的功能
- 变换矩阵的最后一行通常为 `[0, 0, 0, 1]`
