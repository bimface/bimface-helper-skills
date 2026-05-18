# 三维平面

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能创建平面
- `setMap` 是异步回调，贴图加载完成后的操作（添加为外部构件、缩放相机等）必须在回调中执行
- 平面需通过 `extObjMng.addObject` 添加为外部构件后才能在场景中可见

## 添加矩形平面

```javascript
// 定义矩形的两个对角点
const pt1 = {
    x: -19965,
    y: -31846,
    z: 5000
};

const pt2 = {
    x: 60444,
    y: 37484,
    z: 5000
};

// 构造矩形平面对象
const rectanglePlane = new Glodon.Bimface.Plugins.Geometry.Plane(pt1, pt2);

// 贴图URL
const imgSrc = 'https://example.com/plane.png';

// 设置平面的贴图
rectanglePlane.setMap(imgSrc, function() {
    // 贴图加载完成后的回调
    const extObjMng = new Glodon.Bimface.Plugins.ExternalObject.ExternalObjectManager(viewer3D);
    extObjMng.addObject("recPlane", rectanglePlane);
    
    // 构造包围盒并缩放相机
    const bBox = {
        min: pt1,
        max: pt2
    };
    viewer3D.zoomToBoundingBox(bBox);
    viewer3D.render();
}, 0.5);
```

### 显示/隐藏矩形平面

```javascript
// 隐藏矩形平面
const extObjId = extObjMng.getObjectIdByName("recPlane");
viewer3D.hideComponentsById([extObjId]);
viewer3D.render();

// 显示矩形平面
const extObjId = extObjMng.getObjectIdByName("recPlane");
viewer3D.showComponentsById([extObjId]);
viewer3D.render();
```

## 添加空间平面

```javascript
// 定义空间平面的顶点，注意点数不能少于3个，组成的多边形不能有自相交的情况出现
const pt3 = { x: -2700, y: 34464, z: -1677 };
const pt4 = { x: -2700, y: -30270, z: -1677 };
const pt5 = { x: -2700, y: -30270, z: 16150 };
const pt6 = { x: -2700, y: 34464, z: 16150 };

// 构造空间平面对象
const spatialPlane = new Glodon.Bimface.Plugins.Geometry.SpatialPlane([pt3, pt4, pt5, pt6]);

// 贴图URL
const imgSrc = 'https://example.com/section.png';

// 设置空间平面的贴图
spatialPlane.setMap(imgSrc, function() {
    // 贴图加载完成后的回调
    const extObjMng = new Glodon.Bimface.Plugins.ExternalObject.ExternalObjectManager(viewer3D);
    extObjMng.addObject("spaPlane", spatialPlane);
    
    // 构造包围盒并缩放相机
    const bBox = {
        min: pt3,
        max: pt5
    };
    viewer3D.zoomToBoundingBox(bBox);
    viewer3D.render();
}, 0.5);
```

### 显示/隐藏空间平面

```javascript
// 隐藏空间平面
const extObjId = extObjMng.getObjectIdByName("spaPlane");
viewer3D.hideComponentsById([extObjId]);
viewer3D.render();

// 显示空间平面
const extObjId = extObjMng.getObjectIdByName("spaPlane");
viewer3D.showComponentsById([extObjId]);
viewer3D.render();
```
