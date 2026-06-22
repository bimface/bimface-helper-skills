## 使用约束与说明

- 使用 `viewer3D.getModel()` 获取默认模型对象，或 `viewer3D.getModel(modelId)` 获取指定模型
- 以下示例中 `model` 均指通过上述方式获取的模型对象
- 所有轴网显示/隐藏方法均返回 Promise，需使用 `.then()` 或 `async/await` 处理
- v4 中 `updateAxisGridsVisualization` 统一管理轴网的显示层级和颜色设置，替代了 v3 的多个分散方法

### 显示轴网

```javascript
const viewer3D = /* 已初始化的 viewer3D 实例 */;
const model = viewer3D.getModel();

// 按标高显示轴网
model.showAxisGrids({fileId: "file_001", elevation: 0}).then(() => {
  console.log("标高 0 处轴网已显示");
});

// 按楼层显示轴网
model.showAxisGrids({fileId: "file_001", floorId: "floor_01"}).then(() => {
  console.log("指定楼层轴网已显示");
});

// 显示所有轴网
model.showAxisGrids({all: true}).then(() => {
  console.log("所有轴网已显示");
});
```

### 隐藏轴网

```javascript
// 按标高隐藏轴网
model.hideAxisGrids({fileId: "file_001", elevation: 0}).then(() => {
  console.log("标高 0 处轴网已隐藏");
});

// 按楼层隐藏轴网
model.hideAxisGrids({fileId: "file_001", floorId: "floor_01"}).then(() => {
  console.log("指定楼层轴网已隐藏");
});

// 隐藏所有轴网
model.hideAxisGrids({all: true}).then(() => {
  console.log("所有轴网已隐藏");
});
```

### 更新轴网可视化样式

`updateAxisGridsVisualization` 用于设置轴网的显示层级和颜色：

```javascript
// 颜色定义
const color = new Glodon.Bimface.Common.Graphics.Color(255, 0, 0, 1);

// 设置显示层级（置顶、气泡始终可见）
model.updateAxisGridsVisualization({
  display: {
    onTop: true,
    bubblesAlwaysVisible: true
  }
});

// 设置轴网颜色（气泡颜色和线条颜色）
model.updateAxisGridsVisualization({
  colorOption: {
    bubble: color,  // 气泡颜色
    line: color      // 线条颜色
  }
});

// 同时设置显示与颜色
model.updateAxisGridsVisualization({
  display: {
    onTop: true,
    bubblesAlwaysVisible: true
  },
  colorOption: {
    bubble: new Glodon.Bimface.Common.Graphics.Color(255, 0, 0, 1),
    line: new Glodon.Bimface.Common.Graphics.Color(0, 0, 255, 1)
  }
});
```

### 综合示例（async/await）

```javascript
async function setupAxisGrids(viewer3D, fileId) {
  const model = viewer3D.getModel();

  try {
    // 显示指定标高的轴网
    await model.showAxisGrids({fileId, elevation: 0});

    // 设置轴网样式
    model.updateAxisGridsVisualization({
      display: { onTop: true, bubblesAlwaysVisible: true },
      colorOption: {
        bubble: new Glodon.Bimface.Common.Graphics.Color(255, 0, 0, 1),
        line: new Glodon.Bimface.Common.Graphics.Color(0, 150, 0, 1)
      }
    });

    console.log("轴网初始化完成");
  } catch (error) {
    console.error("轴网操作失败:", error);
  }
}
```
