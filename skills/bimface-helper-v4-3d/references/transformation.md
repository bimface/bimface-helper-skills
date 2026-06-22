## 使用约束与说明

- 使用 `viewer3D.getModel()` 获取默认模型对象，或 `viewer3D.getModel(modelId)` 获取指定模型
- 以下示例中 `model` 均指通过上述方式获取的模型对象
- v4 中获取/设置模型变换的方法更名为 `getTransformation()` / `setTransformation()`（v3 中的 `getModelTransformation` / `setModelTransformation` 已不再使用）
- 变换矩阵为 4x4 数组或矩阵对象，表示模型的平移、旋转、缩放在世界坐标系中的变换

### 获取当前变换

```javascript
const viewer3D = /* 已初始化的 viewer3D 实例 */;
const model = viewer3D.getModel();

const transformation = model.getTransformation();
console.log("当前变换矩阵:", transformation);
```

### 设置变换

```javascript
// 还原到初始位置（单位矩阵）
const identityMatrix = [
  1, 0, 0, 0,
  0, 1, 0, 0,
  0, 0, 1, 0,
  0, 0, 0, 1
];
model.setTransformation(identityMatrix);
```

### 修改并应用变换示例

```javascript
const viewer3D = /* 已初始化的 viewer3D 实例 */;
const model = viewer3D.getModel();

// 获取当前变换
const currentTrans = model.getTransformation();

// 沿 X 轴平移 10 个单位（修改矩阵第 13 个元素）
const newTrans = [...currentTrans];
newTrans[12] = currentTrans[12] + 10; // X 轴平移分量位于索引 12

model.setTransformation(newTrans);
```

### 坐标系说明

- 模型变换作用于**世界坐标系**，表示模型从自身局部坐标系到世界坐标系的变换
- 4x4 矩阵按列主序排列（column-major），索引 12、13、14 分别对应 X、Y、Z 轴平移分量
- 设置变换会覆盖先前的变换状态，如需增量变换需先获取当前矩阵、修改后再设置
