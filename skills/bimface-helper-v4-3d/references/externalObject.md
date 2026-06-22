# 外部构件（ExternalObject）

> ⚠️ **注意**：本文档基于 v3 API 文档 `bimface-helper-v3-3d` 和 v4 通用迁移模式（命名空间 `Plugins→Plugin`、Config 类去除、Color 类路径变更、回调→Promise 等）编写，具体 API 请以 `参考/接口文档/ExternalObject-v4.pdf` 为准。

## 使用约束与说明

- **命名空间变更**：v4 中命名空间从 `Plugins.ExternalObject`（复数）变更为 `Plugin.ExternalObject`（单数）
- **Promise 化**：`loadObject` 在 v4 中应返回 Promise，不再使用回调函数
- 整个场景仅需要一个 `ExternalObjectManager` 对象
- 外部构件的操作（移动、缩放、旋转等）必须在 `loadObject` 完成后再执行
- 加载外部构件后需调用 `viewer3D.updateSceneBoundingBox()` 再 `viewer3D.render()`
- v4 中使用 `{x, y, z}` 普通对象代替 THREE.Vector3
- 对内部模型构件进行编辑时，需先通过 `convert()` 将其转为外部构件，原内部构件自动隐藏

---

## 创建外部构件管理器

```javascript
// v4 构造函数接收配置对象（而非直接传入 viewer3D）
const extObjMng = new Glodon.Bimface.Plugin.ExternalObject.ExternalObjectManager({
  viewer: viewer3D
});
```

---

## 加载外部构件

### 通过 URL 加载 FBX/OBJ

```javascript
const extObjMng = new Glodon.Bimface.Plugin.ExternalObject.ExternalObjectManager({
  viewer: viewer3D
});

// loadObject 返回 Promise（v4 模式）
extObjMng.loadObject({
  name: "robot",
  url: { objectUrl: "https://example.com/robot.fbx" }
}).then(() => {
  // 加载完成后获取 ID 并执行变换操作
  const objectId = extObjMng.getObjectIdByName("robot");
  extObjMng.translate(objectId, { x: 0, y: -12000, z: -450 });
  extObjMng.scale(objectId, { x: 10, y: 10, z: 10 });
  extObjMng.rotateX(objectId, Math.PI / 2);

  viewer3D.updateSceneBoundingBox();
  viewer3D.render();
});
```

### 通过 URL 加载 OBJ（含材质）

```javascript
extObjMng.loadObject({
  name: "closet",
  url: {
    objectUrl: "https://example.com/closet.obj",
    mtlUrl: "https://example.com/closet.mtl"
  }
}).then(() => {
  const objectId = extObjMng.getObjectIdByName("closet");
  extObjMng.translate(objectId, { x: -5200, y: -5651, z: -450 });
  extObjMng.rotateX(objectId, Math.PI / 2);

  viewer3D.updateSceneBoundingBox();
  viewer3D.render();
});
```

### 将内部构件转换为外部构件

```javascript
// 将模型内部构件转为外部构件，原内部构件自动隐藏
const convertedObject = extObjMng.convert("282543", true);
extObjMng.loadObject({
  name: "fan",
  object: convertedObject
});

// 跨模型转换（多模型场景）
const crossModelObj = extObjMng.convert(
  { modelId: "10000724170567", objectId: "11091" },
  true
);
extObjMng.loadObject({ name: "part", object: crossModelObj });
```

### 加载外部构件时设置 objectData 与关联关系

```javascript
// objectData 用于条件筛选，association 用于模型爆炸时保持相对位置
extObjMng.loadObject({
  name: "recPlane",
  object: rectanglePlane,
  objectData: [{ levelName: "F2" }],
  association: {
    modelId: "10000776931924",
    objectId: "284052"
  }
}).then(() => {
  viewer3D.updateSceneBoundingBox();
  viewer3D.render();
});
```

---

## 变换操作

所有变换操作在 `loadObject` Promise resolve 后执行。

```javascript
const extObjId = extObjMng.getObjectIdByName("vehicle");

// 绝对平移（v4 使用 {x, y, z} 对象）
extObjMng.translate(extObjId, { x: -7500, y: -15000, z: -450 });

// 轴向偏移（相对当前位置）
extObjMng.offsetY(extObjId, -1000);
extObjMng.offsetZ(extObjId, 250);
extObjMng.offset(extObjId, { x: -1150, y: -7950, z: 3450 });

// 绕世界坐标轴旋转
extObjMng.rotateZ(extObjId, Math.PI / 6);
extObjMng.rotateX(extObjId, Math.PI / 2);

// 缩放
extObjMng.scale(extObjId, { x: 2.0, y: 2.0, z: 2.0 });
```

### 绕任意指定轴旋转

```javascript
// 绕指定基点、方向轴旋转（适用机械臂关节等场景）
extObjMng.rotateOnBasePoint(
  extObjId,
  { x: -1072.803, y: 188.984, z: 960.013 },  // 旋转基点
  { x: 0, y: 1, z: 0 },                       // 旋转轴方向向量
  Math.PI / 20                                  // 旋转弧度
);
viewer3D.render();
```

### 局部坐标转世界坐标

```javascript
const worldPoint = extObjMng.toWorldPosition(extObjId, {
  localPosition: { x: -419.872, y: 5146.563, z: 1281.917 },
  localVector: { x: 1, y: 0, z: 0 }
});
// worldPoint.worldPosition — 世界坐标基点
// worldPoint.worldVector  — 世界坐标旋转轴方向
```

---

## 构件层级关系

```javascript
const relationships = {
  id: excavator_id,
  children: [
    {
      id: arm1_id,
      children: [
        { id: arm2_id, children: [{ id: bucket_id, children: null }] }
      ]
    }
  ]
};
extObjMng.setHierarchy(relationships);

// 解除所有层级关系
extObjMng.clearAllHierarchy();
```

---

## 克隆外部构件

```javascript
extObjMng.loadObject({
  name: "car",
  url: { objectUrl: "https://example.com/car.fbx" }
}).then(() => {
  const fbxId = extObjMng.getObjectIdByName("car");

  for (let i = 0; i < 5; i++) {
    extObjMng.clone(fbxId, `clonedcar${i}`);
    const clonedId = extObjMng.getObjectIdByName(`clonedcar${i}`);
    extObjMng.translate(clonedId, { x: -2200 - (i + 1) * 2600, y: -11651, z: -450 });
    extObjMng.scale(clonedId, { x: 10, y: 10, z: 10 });
    extObjMng.rotateX(clonedId, Math.PI / 2);
  }

  viewer3D.updateSceneBoundingBox();
  viewer3D.render();
});
```

---

## 着色与外观控制

```javascript
// 着色指定构件
const color = new Glodon.Bimface.Common.Graphics.Color(66, 180, 200, 0.5);
extObjMng.overrideColor({ ids: [extObjId] }, color);

// 恢复所有外部构件默认颜色
extObjMng.restoreColor({ all: true });

viewer3D.render();
```

---

## 移除外部构件

```javascript
// 按名称获取 ID 后移除
const objectId = extObjMng.getObjectIdByName("vehicle");
extObjMng.removeById(objectId);

// 移除所有外部构件
extObjMng.clear();

viewer3D.render();
```

---

## v3 → v4 对照

| 项目 | v3（旧版） | v4（新版） |
|------|-----------|-----------|
| 命名空间 | `Plugins.ExternalObject` | `Plugin.ExternalObject` |
| 构造函数 | `new ExternalObjectManager(viewer3D)` | `new ExternalObjectManager({viewer})` |
| loadObject | 回调函数 `(config, callback)` | 返回 Promise `.then()` |
| 坐标对象 | `new THREE.Vector3(x, y, z)` | `{x, y, z}` 普通对象 |
| Color 类路径 | `Glodon.Web.Graphics.Color` | `Glodon.Bimface.Common.Graphics.Color` |
| 移除构件 | `removeById(id)` | `removeById(id)` |
| 清空构件 | `clear()` | `clear()` |
