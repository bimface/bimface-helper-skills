# 电子围墙效果

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能创建电子围墙效果
- 修改效果方向或持续时间后需调用 `wallEffect.update()` 才能生效

## 创建电子围墙效果

```javascript
// 构造电子围墙效果配置项
const wallEffectConfig = new Glodon.Bimface.Plugins.Animation.WallEffectConfig();

// 配置Viewer对象
wallEffectConfig.viewer = viewer3D;

// 配置方向：沿着路径切线方向
wallEffectConfig.direction = {
    type: "Tangent",  // 运动方式为沿着路径的切线方向
    reverse: false     // 运动方向默认为逆时针
};

// 配置持续时间（毫秒）
wallEffectConfig.duration = 3500;

// 配置围墙高度
wallEffectConfig.height = 80000;

// 配置是否拉伸
wallEffectConfig.stretch = true;

// 配置围墙路径坐标点
wallEffectConfig.path = [
    { x: 13023609.960575795, y: 25777457.255968206, z: 50.61713809092208 },
    { x: 13122365.995315686, y: 25839575.569788456, z: 50.61713809194653 },
    { x: 13315590.835127937, y: 25970551.36890273, z: 50.61713809410652 },
    { x: 13276959.063045746, y: 26028989.384943567, z: 50.61713809507025 },
    { x: 13155465.068261532, y: 26169861.95716057, z: 50.61713809737892 },
    // ... 更多坐标点
];

// 配置电子围墙颜色（RGBA）
wallEffectConfig.color = new Glodon.Web.Graphics.Color(50, 211, 166, 0.8);

// 构造电子围墙效果对象
const wallEffect = new Glodon.Bimface.Plugins.Animation.WallEffect(wallEffectConfig);
```

## 设置效果方向

```javascript
// 沿着路径切线方向（默认）
wallEffect.setDirection({
    type: "Tangent",
    reverse: false
});
wallEffect.setDuration(3500);
wallEffect.update();

// 沿着路径法线方向（向上）
wallEffect.setDirection({
    type: "Normal",
    reverse: false
});
wallEffect.setDuration(1000);
wallEffect.update();
```

方向类型说明：
- Tangent: 沿着路径切线方向
- Normal: 沿着路径法线方向（向上）
