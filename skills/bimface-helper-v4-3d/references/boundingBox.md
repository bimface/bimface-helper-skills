## 使用约束与说明

- 使用 `viewer3D.getModel()` 获取默认模型对象，或 `viewer3D.getModel(modelId)` 获取指定模型
- 以下示例中 `model` 均指通过上述方式获取的模型对象
- 边界框结构为：`{min: {x, y, z}, max: {x, y, z}}`
- `getBoundingBoxByIds` 返回 Promise，需使用 `.then()` 或 `async/await` 处理
- 方法签名：`model.getBoundingBoxByIds([componentIds])`，参数为组件 ID 数组

### 单个组件边界框

```javascript
const viewer3D = /* 已初始化的 viewer3D 实例 */;
const model = viewer3D.getModel();

// 获取单个组件的包围盒
model.getBoundingBoxByIds(["123456"]).then(boundingBox => {
  console.log(boundingBox);
  // {min: {x: -10, y: 0, z: -5}, max: {x: 10, y: 20, z: 5}}
}).catch(error => {
  console.error("获取包围盒失败:", error);
});
```

### 多个组件边界框

```javascript
const viewer3D = /* 已初始化的 viewer3D 实例 */;
const model = viewer3D.getModel();

// 获取多个组件的合并包围盒
model.getBoundingBoxByIds(["123456", "789012"]).then(boundingBox => {
  console.log(boundingBox);
  // 返回包含所有指定组件的最小包围盒
});
```

### 合并多个包围盒（辅助函数）

当需要自行合并多个单独获取的包围盒时，可使用以下辅助函数：

```javascript
function mergeBoundingBoxes(boxes) {
  if (!boxes || boxes.length === 0) return null;

  const merged = {
    min: { x: Infinity, y: Infinity, z: Infinity },
    max: { x: -Infinity, y: -Infinity, z: -Infinity }
  };

  boxes.forEach(box => {
    merged.min.x = Math.min(merged.min.x, box.min.x);
    merged.min.y = Math.min(merged.min.y, box.min.y);
    merged.min.z = Math.min(merged.min.z, box.min.z);
    merged.max.x = Math.max(merged.max.x, box.max.x);
    merged.max.y = Math.max(merged.max.y, box.max.y);
    merged.max.z = Math.max(merged.max.z, box.max.z);
  });

  return merged;
}

// 使用示例：分别获取两个组件包围盒后合并
Promise.all([
  model.getBoundingBoxByIds(["123456"]),
  model.getBoundingBoxByIds(["789012"])
]).then(([box1, box2]) => {
  const merged = mergeBoundingBoxes([box1, box2]);
  console.log("合并后的包围盒:", merged);
});
```

### async/await 写法

```javascript
async function getComponentBoundingBox(viewer3D, componentIds) {
  const model = viewer3D.getModel();
  try {
    const boundingBox = await model.getBoundingBoxByIds(componentIds);
    return boundingBox;
  } catch (error) {
    console.error("获取包围盒失败:", error);
    return null;
  }
}

// 调用
const box = await getComponentBoundingBox(viewer3D, ["123456"]);
```
