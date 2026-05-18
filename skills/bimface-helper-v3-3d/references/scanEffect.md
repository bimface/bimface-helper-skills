# 扫描效果

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能创建扫描效果
- 创建/清除扫描效果后需调用 `viewer3D.render()` 刷新场景

## 环状扫描效果

```javascript
// 构造环状扫描效果配置项
const ringScanEffectConfig = new Glodon.Bimface.Plugins.Animation.RingScanEffectConfig();

// 配置Viewer对象
ringScanEffectConfig.viewer = viewer3D;

// 设置扫描颜色（RGBA）
ringScanEffectConfig.color = new Glodon.Web.Graphics.Color(17, 218, 183, 1);

// 设置扫描持续时间（毫秒）
ringScanEffectConfig.duration = 2000;

// 设置扫描中心位置
ringScanEffectConfig.originPosition = {
    x: 15062866.216183793,
    y: 27023651.89954786,
    z: -999.5828804953665
};

// 设置扫描半径
ringScanEffectConfig.radius = 200000;

// 设置衰减力度
ringScanEffectConfig.progressive = 5;

// 构造环状扫描效果对象
const ringScanEffect = new Glodon.Bimface.Plugins.Animation.RingScanEffect(ringScanEffectConfig);

// 显示扫描效果
ringScanEffect.show();

// 渲染场景
viewer3D.render();
```

### 清除环状扫描效果

```javascript
// 清除环状扫描效果
ringScanEffect.destroy();
```

## 扇形扫描效果

```javascript
// 构造扇形扫描效果配置项
const fanScanEffectConfig = new Glodon.Bimface.Plugins.Animation.FanScanEffectConfig();

// 配置Viewer对象
fanScanEffectConfig.viewer = viewer3D;

// 设置背景颜色（RGBA）
fanScanEffectConfig.backgroundColor = new Glodon.Web.Graphics.Color(0, 0, 0, 0.05);

// 设置扫描颜色（RGBA）
fanScanEffectConfig.color = new Glodon.Web.Graphics.Color(17, 218, 183, 0.8);

// 设置扫描持续时间（毫秒）
fanScanEffectConfig.duration = 2000;

// 设置扇形角度（弧度）
fanScanEffectConfig.fanAngle = Math.PI;

// 设置扫描中心位置
fanScanEffectConfig.originPosition = {
    x: 14778969.35005347,
    y: 25677946.03299103,
    z: 2000
};

// 设置扫描半径
fanScanEffectConfig.radius = 200000;

// 构造扇形扫描效果对象
const fanScanEffect = new Glodon.Bimface.Plugins.Animation.FanScanEffect(fanScanEffectConfig);

// 显示扫描效果
fanScanEffect.show();

// 渲染场景
viewer3D.render();
```

### 清除扇形扫描效果

```javascript
// 清除扇形扫描效果
fanScanEffect.destroy();
```
