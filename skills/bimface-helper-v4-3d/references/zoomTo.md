# 视角定位（Zoom To）

## 使用约束与说明

- 相机对象通过 `viewer3D.getCamera()` 同步获取
- 视角定位方法返回 Promise，需通过 `.then()` 或 `await` 处理
- `zoomToBoundingBox` 和 `zoomToSelection` 是两个核心定位方法
- `setStandardView` 用于切换到预定义标准视角
- 相关依赖：获取包围盒需使用 `model.getBoundingBox()` 或 `model.getBoundingBoxByIds([ids])`（均为 Promise）
- 所有操作必须在模型加载完成后进行

---

## zoomToBoundingBox —— 缩放到包围盒

将相机视野缩放到指定的包围盒范围。

```javascript
camera.zoomToBoundingBox({
  boundingBox: {
    min: { x: -10, y: 0, z: -10 },
    max: { x: 10, y: 20, z: 10 }
  },
  margin: 0.1,         // 边距比例，默认 0.1（10%）
  duration: 1000,      // 动画时长（毫秒），默认 1000
  direction: { x: 0, y: 0, z: -1 }  // 相机方向，默认 {x:0, y:0, z:-1}
}).then(() => {
  console.log("已缩放到包围盒");
});
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| boundingBox | `{min, max}` | 是 | 目标包围盒 |
| margin | Number | 否 | 边距比例，>0 扩大视野，<0 缩小视野，默认 0.1 |
| duration | Number | 否 | 动画时长（毫秒），默认 1000 |
| direction | `{x, y, z}` | 否 | 相机朝向，默认 `{x:0, y:0, z:-1}` |

---

## zoomToSelection —— 缩放到选中构件

先通过 `model.addSelection()` 选中构件，再缩放到选中集。

```javascript
// margin > 0：视野外扩（更远）
// margin < 0：视野内缩（更近）
camera.zoomToSelection({
  margin: 0.1
}).then(() => {
  console.log("已缩放到选中构件");
});
```

| 参数 | 类型 | 说明 |
|------|------|------|
| margin | Number | 边距比例，>0 视野放大/拉远，<0 视野缩小/靠近 |

---

## setStandardView —— 标准视角

切换到预定义的 6 个标准视角 + Home 视角：

```javascript
// 可用的标准视角枚举值：
// Glodon.Bimface.Camera.ViewOption3D.Home      - 默认视角（前上 45°）
// Glodon.Bimface.Camera.ViewOption3D.Front     - 前视图
// Glodon.Bimface.Camera.ViewOption3D.Back      - 后视图
// Glodon.Bimface.Camera.ViewOption3D.Left      - 左视图
// Glodon.Bimface.Camera.ViewOption3D.Right     - 右视图
// Glodon.Bimface.Camera.ViewOption3D.Top       - 俯视图
// Glodon.Bimface.Camera.ViewOption3D.Bottom    - 仰视图

camera.setStandardView(Glodon.Bimface.Camera.ViewOption3D.Front).then(() => {
  console.log("已切换到前视图");
});
```

---

## 相关方法

### model.getBoundingBox —— 获取模型包围盒

```javascript
// 返回 Promise，resolve 后得到 {min: {x,y,z}, max: {x,y,z}}
const box = await model.getBoundingBox();
```

### model.getBoundingBoxByIds —— 获取指定构件包围盒

```javascript
// 返回 Promise
const box = await model.getBoundingBoxByIds(["componentId1", "componentId2"]);
```

---

## 完整示例

```javascript
const viewer3D = new Glodon.Bimface.Viewer.Viewer3D(viewer3DConfig);
viewer3D.addModel(viewToken);
const model = viewer3D.getModel();
const camera = viewer3D.getCamera();

viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ModelLoaded, () => {
  // === 1. 缩放到模型全景 ===
  document.getElementById("btnZoomToModel").addEventListener("click", async () => {
    const box = await model.getBoundingBox();
    camera.zoomToBoundingBox({
      boundingBox: box,
      margin: 0.1,
      duration: 1500
    }).then(() => {
      console.log("已缩放到模型全景");
    });
  });

  // === 2. 缩放到指定构件 ===
  document.getElementById("btnZoomToComponents").addEventListener("click", async () => {
    const componentIds = ["component_100", "component_200"];
    const box = await model.getBoundingBoxByIds(componentIds);
    camera.zoomToBoundingBox({
      boundingBox: box,
      margin: 0.05,       // 更紧凑的边距
      duration: 800
    }).then(() => {
      console.log("已缩放到指定构件");
    });
  });

  // === 3. 选中后缩放到选中集 ===
  document.getElementById("btnZoomToSelection").addEventListener("click", () => {
    // 先选中构件
    model.addSelection({ ids: ["component_100", "component_200", "component_300"] });
    // 缩放到选中集
    camera.zoomToSelection({
      margin: 0.15          // 略大的边距，让选中构件周围有呼吸空间
    }).then(() => {
      console.log("已缩放到选中构件");
    });
  });

  // 选中 + 紧凑缩放（更近视角）
  document.getElementById("btnZoomToSelectionClose").addEventListener("click", () => {
    model.addSelection({ ids: ["component_100"] });
    camera.zoomToSelection({
      margin: -0.1         // 负值：更靠近构件
    }).then(() => {
      console.log("已靠近选中构件");
    });
  });

  // === 4. 标准视角切换 ===
  const views = ["Home", "Front", "Back", "Left", "Right", "Top", "Bottom"];
  views.forEach(viewName => {
    document.getElementById(`btnView${viewName}`).addEventListener("click", () => {
      camera.setStandardView(
        Glodon.Bimface.Camera.ViewOption3D[viewName]
      ).then(() => {
        console.log(`已切换到 ${viewName} 视角`);
      });
    });
  });

  // === 5. 按条件查找并缩放 ===
  document.getElementById("btnFindAndZoom").addEventListener("click", async () => {
    // 查找 F01 楼层的所有构件
    const objectData = { levelName: "F01" };
    const componentIds = await model.getMatchIds({ objectData });

    if (componentIds.length === 0) {
      console.log("未找到匹配构件");
      return;
    }

    // 获取这些构件的包围盒并缩放
    const box = await model.getBoundingBoxByIds(componentIds);
    camera.zoomToBoundingBox({
      boundingBox: box,
      margin: 0.1,
      duration: 1200
    }).then(() => {
      console.log(`已缩放到 F01 楼层（${componentIds.length} 个构件）`);
    });
  });

  // === 6. 自定义方向缩放 ===
  document.getElementById("btnZoomFromTop").addEventListener("click", async () => {
    const box = await model.getBoundingBox();
    // 从顶部（正上方）看向模型
    camera.zoomToBoundingBox({
      boundingBox: box,
      margin: 0.1,
      duration: 1500,
      direction: { x: 0, y: 0, z: 1 }   // 从 Z 轴正方向（上方）看向原点
    }).then(() => {
      console.log("已从顶部查看模型");
    });
  });
});
```

---

## 注意事项

- `zoomToBoundingBox` 和 `zoomToSelection` 返回 Promise，动画完成后 resolve
- `setStandardView` 返回 Promise，视角切换完成后 resolve
- `margin` 为负数时使视野更靠近目标，正数时更远离（给目标留更多边距）
- `zoomToSelection` 需要先通过 `model.addSelection()` 选中目标构件
- `model.getBoundingBox()` 和 `model.getBoundingBoxByIds([ids])` 都返回 Promise
- `direction` 参数用于控制缩放后相机的朝向，常用于实现"从指定方向查看"的效果
