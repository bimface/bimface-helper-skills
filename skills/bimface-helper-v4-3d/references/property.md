# 模型属性查询（Model Property）

## 使用约束与说明

- 所有属性查询方法返回 Promise，必须使用 `.then()` 或 `async/await` 处理，**不接受回调函数**
- 模型必须加载完成后才能调用这些方法
- 多模型场景下，需要通过 `viewer3D.getModel(modelId)` 获取指定模型实例

---

## getComponentProperty —— 获取构件属性

获取单个构件的详细属性信息：

```javascript
// v4: 返回 Promise，使用 .then() 获取结果
model.getComponentProperty("componentId").then((property) => {
  console.log("构件属性:", property);
  // property 是一个包含该构件所有属性字段的对象
});
```

### async/await 用法

```javascript
async function getComponentInfo(model, componentId) {
  const property = await model.getComponentProperty(componentId);
  return property;
}
```

---

## getFloors —— 获取楼层信息

获取模型的所有楼层列表：

```javascript
// v4: 返回 Promise，不接受回调
model.getFloors().then((floors) => {
  console.log("楼层列表:", floors);
  floors.forEach((floor) => {
    console.log(`楼层: ${floor.levelName}, 标高: ${floor.elevation}`);
  });
});
```

### async/await 用法

```javascript
async function listFloors(model) {
  const floors = await model.getFloors();
  return floors;
}
```

---

## 单模型模式

```javascript
const viewer3D = new Glodon.Bimface.Viewer.Viewer3D(viewer3DConfig);
viewer3D.addModel(viewToken);
const model = viewer3D.getModel();

viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ModelLoaded, () => {
  // 获取楼层信息
  model.getFloors().then((floors) => {
    console.log("楼层数量:", floors.length);
  });

  // 获取某个构件的属性
  model.getComponentProperty("component_123").then((property) => {
    console.log("构件属性:", property);
  });
});
```

---

## 多模型模式

```javascript
const viewer3D = new Glodon.Bimface.Viewer.Viewer3D(viewer3DConfig);

// 添加多个模型
viewer3D.addModel(viewToken1);
viewer3D.addModel(viewToken2);

viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ModelLoaded, () => {
  // 获取指定模型实例
  const model1 = viewer3D.getModel(modelId1);
  const model2 = viewer3D.getModel(modelId2);

  // 分别查询两个模型的信息
  model1.getFloors().then((floors) => {
    console.log("模型1 楼层:", floors);
  });

  model2.getComponentProperty("component_456").then((property) => {
    console.log("模型2 构件属性:", property);
  });
});
```

---

## 注意事项

- `getComponentProperty` 返回的对象结构与模型类型（Revit / IFC 等）有关，字段名称不固定
- 如需获取模型完整树结构，`getModelTree()` 方法可能存在但未在 v4 文档中确认，建议以实际 SDK 版本为准
- Promise 调用可能因构件 ID 不存在而 reject，建议添加 `.catch()` 处理异常
