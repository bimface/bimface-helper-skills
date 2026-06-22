# 嵌入图纸（Embed Drawing）

## 使用约束与说明

- 使用 `viewer3D.getModel()` 获取默认模型对象，或 `viewer3D.getModel(modelId)` 获取指定模型
- 以下示例中 `model` 均指通过上述方式获取的模型对象
- `getDrawingList` 返回 Promise，需使用 `.then()` 或 `async/await` 处理
- v4 中方法名为 `getDrawingList`，参数为一个包含 `fileId` 和 `id`（组件 ID）的对象

### 获取模型组件关联的图纸列表

```javascript
const viewer3D = /* 已初始化的 viewer3D 实例 */;
const model = viewer3D.getModel();

const fileId = "file_001";
const componentId = "123456";

model.getDrawingList({fileId, id: componentId}).then(drawingList => {
  console.log("关联图纸列表:", drawingList);
  // drawingList 为数组，每项包含图纸的相关信息（名称、ID 等）
}).catch(error => {
  console.error("获取图纸列表失败:", error);
});
```

### 遍历图纸列表

```javascript
model.getDrawingList({fileId, id: componentId}).then(drawingList => {
  if (drawingList && drawingList.length > 0) {
    drawingList.forEach((drawing, index) => {
      console.log(`图纸 ${index + 1}:`, drawing);
    });
  } else {
    console.log("该组件没有关联图纸");
  }
});
```

### async/await 写法

```javascript
async function getComponentDrawings(viewer3D, fileId, componentId) {
  const model = viewer3D.getModel();

  try {
    const drawingList = await model.getDrawingList({fileId, id: componentId});
    console.log("组件关联图纸:", drawingList);
    return drawingList;
  } catch (error) {
    console.error("获取图纸列表失败:", error);
    return [];
  }
}

// 调用
const drawings = await getComponentDrawings(viewer3D, "file_001", "123456");
```
