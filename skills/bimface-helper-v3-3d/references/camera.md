# 相机状态
在BIMFACE中，可以获取和设置相机的状态，包括位置、目标点、视角等参数。

## 获取相机状态
```javascript
// 获取当前相机状态
const cameraState = viewer.getCamera().getStatus();

// 渲染场景
viewer.render();

// 打印相机状态
console.log(cameraState);
```

## 恢复相机状态
```javascript
// 恢复之前保存的相机状态
viewer.getCamera().setStatus(cameraState);

// 渲染场景
viewer.render();
```

## 缩放到包围盒
```javascript
// 定义包围盒坐标
const boundingbox = {
    "min": {
        "x": -9496.219279454494,
        "y": -4299.299971053061,
        "z": 999.9607157957682
    },
    "max": {
        "x": 8443.779875881222,
        "y": 6382.751044860747,
        "z": 9475.048556778615
    }
};
// 缩放到指定包围盒
viewer.getCamera().zoomToBoundingBox({
    boundingbox: boundingbox,  // 包围盒对象
    margin: 0.5  // 包围盒缩放比例，取值最小为-1，默认值为0.5，值越大包围盒外放越多
});
```

## 视口缩放

以固定步长放大/缩小当前视口。

```javascript
// 放大视口
viewer3D.zoomIn();

// 缩小视口
viewer3D.zoomOut();
```

## 设置相机交互行为
```javascript
// 设置禁用/启用平移
viewer.getCamera().enablePan(true);
// 设置禁用/启用俯仰
viewer.getCamera().enablePitch(true);
// 设置禁用/启用旋转
viewer.getCamera().enableRotate(true);
// 设置禁用/启用缩放
viewer.getCamera().enableZoom(true);
```

## 设置阻尼效果

控制模型拖动时的惯性效果。

```javascript
// 开启阻尼
viewer3D.enableDamping(true);

// 设置阻尼系数（值越大阻尼越强）
viewer3D.setDampingFactor(3);

// 关闭阻尼
viewer3D.enableDamping(false);
viewer3D.render();
```

## 设置相机的转场效果
```javascript
// 设置禁用/启用相机转场效果
viewer.getCamera().enableTransitionEffect(true);
```

## 限制相机移动范围
```javascript
// 限制相机移动范围
viewer.getCamera().lockAxis(Glodon.Bimface.Viewer.AxisOption.Z, [Math.PI / 12, Math.PI / 2]);
// 解除相机移动范围限制
viewer.getCamera().unlockAxis(Glodon.Bimface.Viewer.AxisOption.Z);
```
限制轴仅支持Z轴，其他轴不支持限制；范围取值为[0, Math.PI]，这里是弧度。

## 相机状态参数说明
相机状态包含以下参数：
| 参数 | 说明 |
|------|------|
| name | 视角名称 |
| position | 相机位置 {x, y, z} |
| target | 相机目标点 {x, y, z} |
| up | 相机上方方向 {x, y, z} |
| near | 近裁剪面距离 |
| far | 远裁剪面距离 |
| zoom | 缩放比例 |
| version | 版本号 |
| fov | 视角场角度 |
| aspect | 宽高比 |
| coordinateSystem | 坐标系类型 |
