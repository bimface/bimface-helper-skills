# 外部构件

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能初始化外部构件管理器
- `loadObject` 是异步回调，外部构件的操作（移动、缩放、旋转等）必须在加载完成的回调中执行；仅传入 `object`（程序构造的几何体）时同步可用
- 整个场景仅需要一个 ExternalObjectManager 对象
- 对内部模型构件进行编辑（移动、旋转等）时，需先通过 `convert()` 将其转为外部构件，原内部构件自动隐藏

## 创建管理器

```javascript
const extObjMng = new Glodon.Bimface.Plugins.ExternalObject.ExternalObjectManager(viewer3D);
```

## 加载外部构件

### 通过URL加载FBX/3DS

```javascript
const extObjMng = new Glodon.Bimface.Plugins.ExternalObject.ExternalObjectManager(viewer3D);

extObjMng.loadObject({ name: 'robot', url: { objectUrl: fbxUrl } }, function () {
    const objectId = extObjMng.getObjectIdByName("robot");
    extObjMng.translate(objectId, new THREE.Vector3(0, -12000, -450));
    extObjMng.scale(objectId, new THREE.Vector3(10, 10, 10));
    extObjMng.rotateX(objectId, Math.PI / 2);
    viewer3D.render();
});
```

### 通过URL加载OBJ（含材质）

```javascript
extObjMng.loadObject({
    name: 'closet',
    url: {
        objectUrl: objUrl,
        mtlUrl: mtlUrl,
    }
}, function () {
    const objectId = extObjMng.getObjectIdByName("closet");
    extObjMng.translate(objectId, { x: -5200, y: -5651, z: -450 });
    extObjMng.rotateX(objectId, Math.PI / 2);
    viewer3D.render();
});
```

### 将内部构件转换为外部构件

```javascript
// 将模型内部构件转为外部构件，原内部构件自动隐藏
extObjMng.loadObject({ name: 'fan', object: extObjMng.convert('282543', true) });

// 跨模型转换（多模型场景）
extObjMng.loadObject({ name: 'part', object: extObjMng.convert({ modelId: '10000724170567', objectId: '11091' }, true) });

const extObjId = extObjMng.getObjectIdByName('fan');
viewer3D.render();
```

### 加载外部构件时设置筛选条件与关联关系

`objectData` 用于条件筛选（如 `hideComponentsByObjectData`），`association` 用于模型爆炸时保持相对位置。

```javascript
extObjMng.loadObject({
    name: 'recPlane',
    object: rectanglePlane,
    objectData: [{ levelName: 'F2' }],
    association: {
        modelId: '10000776931924',
        objectId: '284052'
    }
}, function (successData) {
    const extObjId = extObjMng.getObjectIdByName("recPlane");
    viewer3D.render();
});
```

## 几何体对象

可通过 ExternalObjectManager 将程序构造的几何体加载为外部构件。

### 样条曲线 SplineCurve

```javascript
const points = [{ x: -94896, y: 59909, z: 10000 }, { x: -67288, y: -1781, z: 1500 }];

const splineCurve = new Glodon.Bimface.Plugins.Geometry.SplineCurve(points);
splineCurve.setType("polyline");     // "spline" | "polyline"
splineCurve.setWidth(1);
splineCurve.setColor(new Glodon.Web.Graphics.Color(255, 255, 0, 0.6));
splineCurve.setStyle({
    lineType: "Dashed",              // "Continuous" | "Dashed"
    lineStyle: {
        dashLength: 2000,            // 虚线短划线长度
        gapLength: 200               // 虚线间隙长度
    }
});

extObjMng.loadObject({ name: 'curve', object: splineCurve });
viewer3D.render();
```

#### 曲线贴地模式 clampMode

```javascript
splineCurve.clampMode({ mode: 'Space' });   // 空间曲线（不贴地）
splineCurve.clampMode({ mode: 'Ground' });  // 仅贴地形
splineCurve.clampMode({ mode: 'Model' });   // 仅贴模型
splineCurve.clampMode({ mode: 'Both' });    // 贴地形和模型
viewer3D.render();
```

#### 获取曲线上指定参数位置的点

```javascript
const startPos = splineCurve.getPointByParameter(0);  // 参数范围 [0, 1]
```

### 矩形平面 Plane

```javascript
const pt1 = { x: 0, y: 0, z: 0 };
const pt2 = { x: 2500, y: 800, z: 0 };
const rectanglePlane = new Glodon.Bimface.Plugins.Geometry.Plane(pt1, pt2);
rectanglePlane.setBorderColor(new Glodon.Web.Graphics.Color('#FFFFFF', 1.0));
rectanglePlane.setColor(new Glodon.Web.Graphics.Color('#32D3A6', 0.4));

extObjMng.loadObject({ name: 'recPlane', object: rectanglePlane });
```

### 带状面 Band

沿曲线生成的带状面，可设置两侧宽度和贴图。

```javascript
const band = new Glodon.Bimface.Plugins.Geometry.Band({
    width: [800, 800],                                    // [左侧宽度, 右侧宽度]
    curve: splineCurve,
    border: {
        enable: true,
        color: new Glodon.Web.Graphics.Color(0, 0, 0, 0.5)
    },
    color: new Glodon.Web.Graphics.Color(66, 64, 64, 1)
});

extObjMng.loadObject({ name: 'band', object: band });

// 设置带状面贴图（材质加载完成后在回调中执行）
const materialConfig = new Glodon.Bimface.Plugins.Material.MaterialConfig();
materialConfig.viewer = viewer3D;
materialConfig.src = "https://static.bimface.com/attach/..._道路贴图.png";
materialConfig.scale = [1.0, 25.0];                      // [U方向缩放, V方向缩放]
materialConfig.callback = function () {
    band.setMaterial(material);
    band.enableBorder(false);
    band.update();
    viewer3D.render();
};
const material = new Glodon.Bimface.Plugins.Material.Material(materialConfig);
```

### 圆管 Pipe

支持圆弧和直线两种轨道类型。

```javascript
// 直线型圆管
const pipeLine = new Glodon.Bimface.Plugins.Geometry.Pipe({
    color: new Glodon.Web.Graphics.Color(180, 180, 180, 1.0),
    crossSection: {
        radius: 50,       // 断面圆半径
        segments: 10      // 断面离散段数
    },
    rail: {
        line: {
            startPoint: { x: 0, y: 0, z: 0 },
            endPoint: { x: 1000, y: 0, z: 0 }
        },
        segments: 1       // 直线段数固定为1
    }
});
extObjMng.loadObject({ name: 'pipe', object: pipeLine });

// 圆弧型圆管
const pipeArc = new Glodon.Bimface.Plugins.Geometry.Pipe({
    color: new Glodon.Web.Graphics.Color(180, 180, 180, 1.0),
    crossSection: { radius: 50, segments: 10 },
    rail: {
        arc: {
            startPoint: { x: 0, y: 0, z: 0 },
            throughPoint: { x: 500, y: 500, z: 0 },
            endPoint: { x: 1000, y: 0, z: 0 }
        },
        segments: 10
    }
});
extObjMng.loadObject({ name: 'pipeArc', object: pipeArc });
```

### 通过 viewer3D 快捷添加

```javascript
viewer3D.addExternalObject("pipe1", pipeLine);
// 移除
new Glodon.Bimface.Plugins.ExternalObject.ExternalObjectManager(viewer3D).removeById("pipe1");
```

## 变换操作

所有变换操作在 `loadObject` 回调中执行。

```javascript
const extObjId = extObjMng.getObjectIdByName("vehicle");

// 绝对平移
extObjMng.translate(extObjId, { x: -7500, y: -15000, z: -450 });

// 轴向偏移（相对当前位置）
extObjMng.offsetY(extObjId, -1000);  // 沿Y轴移动 -1000mm
extObjMng.offsetZ(extObjId, 250);    // 沿Z轴移动 250mm
extObjMng.offset(extObjId, { x: -1150, y: -7950, z: 3450 });

// 绕世界坐标轴旋转
extObjMng.rotateZ(extObjId, Math.PI / 6);    // 绕Z轴逆时针旋转30°
extObjMng.rotateX(extObjId, Math.PI / 2);

// 缩放
extObjMng.scale(extObjId, { x: 2.0, y: 2.0, z: 2.0 });
```

### 绕任意指定轴旋转 rotateOnBasePoint

适用于机械臂关节旋转等场景。

```javascript
// 绕指定基点、指定方向轴旋转指定弧度
extObjMng.rotateOnBasePoint(
    extObjId,
    { x: -1072.803, y: 188.984, z: 960.013 },   // 旋转基点（世界坐标）
    { x: 0, y: 1, z: 0 },                        // 旋转轴方向向量
    Math.PI / 20                                  // 旋转弧度
);
viewer3D.render();
```

### 局部坐标与世界坐标转换 toWorldPosition

计算构件局部坐标中某点在世界坐标系中的位置与方向。

```javascript
const localPoint = {
    localPosition: { x: -419.872, y: 5146.563, z: 1281.917 },
    localVector: { x: 1, y: 0, z: 0 }
};
const worldPoint = extObjMng.toWorldPosition(armId, localPoint);
// worldPoint.worldPosition — 世界坐标下的基点
// worldPoint.worldVector  — 世界坐标下的旋转轴方向
extObjMng.rotateOnBasePoint(armId, worldPoint.worldPosition, worldPoint.worldVector, radian);
```

## 构件层级关系

设置父子层级后，父构件变换时子构件跟随变换。

```javascript
const relationships = {
    "id": excavator_id,
    "children": [
        {
            "id": arm1_id,
            "children": [
                { "id": arm2_id, "children": [{ "id": bucket_id, "children": null }] }
            ]
        }
    ]
};
extObjMng.setHierarchy(relationships);

// 解除所有层级关系
extObjMng.clearAllHierarchy();
```

### 获取与设置变换矩阵

```javascript
// 获取构件当前变换矩阵
const trans = extObjMng.getTransformation(excavator_id);

// 将指定构件的变换重置为相同矩阵（对齐姿态）
extObjMng.setTransformation(arm1_id, trans);
extObjMng.setTransformation(arm2_id, trans);
extObjMng.setTransformation(bucket_id, trans);
```

## 克隆外部构件

```javascript
for (let i = 0; i < 5; ++i) {
    extObjMng.clone(fbxId, `clonedcar${i}`);
    const clonedId = extObjMng.getObjectIdByName(`clonedcar${i}`);
    extObjMng.translate(clonedId, { x: -2200 - (i + 1) * 2600, y: -11651, z: -450 });
    extObjMng.scale(clonedId, new THREE.Vector3(10, 10, 10));
    extObjMng.rotateX(clonedId, Math.PI / 2);
    viewer3D.render();
}
```

## 动画

### FBX骨骼动画

```javascript
// 播放所有外部构件的FBX动画
extObjMng.getAllObjectIds().map(function (id) { extObjMng.play(id); });

// 暂停
extObjMng.getAllObjectIds().map(function (id) { extObjMng.pause(id); });
```

### 基于 requestAnimationFrame 的自定义动画

```javascript
let animationId;
function animate() {
    animationId = requestAnimationFrame(animate);
    extObjMng.rotateOnBasePoint(extObjId, basePoint, axis, Math.PI / 20);
    viewer3D.render();
}
animate();

// 停止动画
cancelAnimationFrame(animationId);
```

## 路径动画

外部构件沿指定样条曲线运动，支持循环播放和姿态跟随。

```javascript
const pathAnimationConfig = new Glodon.Bimface.Plugins.Animation.PathAnimationConfig();
pathAnimationConfig.viewer = viewer3D;
pathAnimationConfig.path = splineCurve;                          // 运动路径 SplineCurve
pathAnimationConfig.time = 8000;                                 // 动画总时长（毫秒）
pathAnimationConfig.loop = true;                                 // 是否循环
pathAnimationConfig.objectNames = [extObjMng.getObjectIdByName("vehicle")];  // 运动构件
pathAnimationConfig.isPitchEnabled = true;                       // 是否跟随俯仰
pathAnimationConfig.isYawEnabled = true;                         // 是否跟随偏航
pathAnimationConfig.originYaw = 0.5 * Math.PI;                   // 初始偏航角

const pathAnimation = new Glodon.Bimface.Plugins.Animation.PathAnimation(pathAnimationConfig);
pathAnimation.play();
// pathAnimation.pause();
// pathAnimation.stop();
```

### 相机跟随路径动画

```javascript
camera3D.setCameraAnimation({
    pathAnimation: pathAnimation,
    distance: 10000,
    angle: -Math.PI * 0.1
});
viewer3D.hideViewHouse();
app3D.getToolbar('MainToolbar').hide();

// 解除绑定
camera3D.clearCameraAnimation();
viewer3D.showViewHouse();
app3D.getToolbar('MainToolbar').show();
```

### 路径动画中让标签跟随构件运动

监听 `ExternalObjectTransformed` 事件，在回调中更新标签位置。

```javascript
const leadLabel = new Glodon.Bimface.Plugins.Drawable.LeadLabel(labelConfig);
drawableContainer.addItem(leadLabel);

function onCarTransformed(id, position) {
    drawableContainer.getItemById("labelItem").setWorldPosition(position);
}

extObjMng.addEventListener(
    Glodon.Bimface.Viewer.Viewer3DEvent.ExternalObjectTransformed,
    onCarTransformed
);

// 取消标签跟随
extObjMng.removeEventListener(
    Glodon.Bimface.Viewer.Viewer3DEvent.ExternalObjectTransformed,
    onCarTransformed
);
```

### 更新场景包围盒

添加外部构件后调用，确保构件在相机显示范围内。

```javascript
viewer3D.updateSceneBoundingBox();
viewer3D.render();
```

## 外观控制

### 着色与恢复

```javascript
// 着色指定构件
const color = new Glodon.Web.Graphics.Color(66, 180, 200, 0.5);
extObjMng.overrideColor({ ids: [extObjId] }, color);

// 恢复所有外部构件默认颜色
extObjMng.restoreColor({ all: true });
```

### 为外部构件设置 Canvas 材质

```javascript
const canvas = document.createElement("canvas");
// ... 在 canvas 上绘制内容 ...
const materialConfig = new Glodon.Bimface.Plugins.Material.MaterialConfig();
materialConfig.viewer = viewer3D;
materialConfig.canvas = canvas;
const material = new Glodon.Bimface.Plugins.Material.Material(materialConfig);
material.overrideComponentsMaterialById([extObjId]);
viewer3D.render();
```

## 移除外部构件

```javascript
const extObjMng = new Glodon.Bimface.Plugins.ExternalObject.ExternalObjectManager(viewer3D);

// 按名称获取ID后移除
const objectId = extObjMng.getObjectIdByName("vehicle");
extObjMng.removeById(objectId);

// 移除所有外部构件
extObjMng.clear();

viewer3D.render();
```
