# 水面效果

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能创建水面效果
- 通过平面构件添加水面时需先隐藏原始构件，销毁水面后恢复显示

## 通过平面构件添加水面

```javascript
// 需要添加水面效果的平面构件
const WaterComponents = [
    { modelId: "10000777816696", objectIds: ["311979", "312471"] }
];

// 隐藏原始平面构件
viewer3D.hideComponentsById(WaterComponents);

// 构造水面效果配置项
const waterEffectConfig = new Glodon.Bimface.Plugins.Animation.WaterEffectConfig();

// 通过平面构件添加水面效果（ids、boundary选填一项，若都填写则默认ids有效）
waterEffectConfig.ids = WaterComponents;
waterEffectConfig.viewer = viewer3D;

// 构造水面效果类
const waterEffect = new Glodon.Bimface.Plugins.Animation.WaterEffect(waterEffectConfig);

// 设置水面颜色
const waterColor = new Glodon.Web.Graphics.Color('#23A9F2', 0.4);
waterEffect.setColor(waterColor);

// 设置水面缩放比例
waterEffect.setScale(2);

// 设置X方向速度
waterEffect.setXDirection(2);

// 设置Y方向速度
waterEffect.setYDirection(2);
```

## 通过构造点添加水面

```javascript
// 定义水面边界点
const pt1 = { x: -111660, y: 70555, z: 2500 };
const pt2 = { x: -111660, y: -66665, z: 2500 };
const pt3 = { x: 112330, y: -66665, z: 2500 };
const pt4 = { x: 112330, y: 70555, z: 2500 };

const waterBoundary = [pt1, pt2, pt3, pt4];

// 构造水面效果配置项
const waterEffectConfig = new Glodon.Bimface.Plugins.Animation.WaterEffectConfig();

// 通过构造点添加水面效果
waterEffectConfig.boundary = waterBoundary;
waterEffectConfig.viewer = viewer3D;

// 构造水面效果类
const waterEffect = new Glodon.Bimface.Plugins.Animation.WaterEffect(waterEffectConfig);

// 设置水面颜色
const waterColor = new Glodon.Web.Graphics.Color('#23A9F2', 0.4);
waterEffect.setColor(waterColor);

// 设置水面缩放比例
waterEffect.setScale(2);

// 设置X方向速度
waterEffect.setXDirection(2);

// 设置Y方向速度
waterEffect.setYDirection(2);
```

## 水面下降/上升

```javascript
// 水面下降
waterEffect.setOffset(-2000);

// 水面上升
waterEffect.setOffset(2000);
```

## 销毁水面效果

```javascript
// 通过构件添加的水面
if (water1) {
    water1.destroy();
    water1 = null;
    // 恢复显示原始平面构件
    viewer3D.showComponentsById(WaterComponents);
    viewer3D.render();
}

// 通过边界点添加的水面
if (water2) {
    water2.destroy();
    water2 = null;
}
```

## 材质流动效果

通过材质贴图+流速控制实现管道内水流效果。

```javascript
const materialConfig = new Glodon.Bimface.Plugins.Material.MaterialConfig();
materialConfig.viewer = viewer3D;
materialConfig.src = '贴图URL';
materialConfig.rotation = 90;
materialConfig.offset = [0, 0];
materialConfig.scale = [0.1524, 0.1524];

const material = new Glodon.Bimface.Plugins.Material.Material(materialConfig);
material.overrideComponentsMaterialById(['724388', '722117']);

// 水流动画
const flowConfig = new Glodon.Bimface.Plugins.Animation.FlowEffectConfig();
flowConfig.material = material;
flowConfig.speed = [0.1, 0];
flowConfig.viewer = viewer3D;

const flowEffect = new Glodon.Bimface.Plugins.Animation.FlowEffect(flowConfig);
flowEffect.play();
```

### 停止流动

```javascript
flowEffect.stop();
```

### 清除材质

```javascript
material.clearOverrideComponentsMaterial();
viewer3D.render();
```

## 喷水效果

```javascript
const sprayConfig = new Glodon.Bimface.Plugins.ParticleSystem.SprayWaterEffectConfig();
sprayConfig.viewer = viewer3D;
sprayConfig.color = new Glodon.Web.Graphics.Color(231, 254, 255, 1);
sprayConfig.originPosition = { x: -105, y: -8648, z: -9118 };
sprayConfig.originPitch = 0.15 * Math.PI;
sprayConfig.originYaw = 0.5 * Math.PI;
sprayConfig.originRadius = 50;
sprayConfig.originIntensity = 0.3;
sprayConfig.spread = 1.1;
sprayConfig.scale = 2;

const sprayWater = new Glodon.Bimface.Plugins.ParticleSystem.SprayWaterEffect(sprayConfig);
sprayWater.update();
viewer3D.render();
```

### 控制喷水

```javascript
sprayWater.stop();
sprayWater.play();
```
