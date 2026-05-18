# 火焰效果

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能创建火焰效果
- 修改火焰参数（大小、类型、浓度、颜色）后需调用 `fireEffect.update()` 才能生效

## 添加火焰效果

```javascript
// 火焰对象的插入点
const firePos = {
    x: -2194.954,
    y: -7739.213,
    z: 10527.306
};

// 构造火焰效果的配置项
const fireConfig = new Glodon.Bimface.Plugins.ParticleSystem.FireEffectConfig();

// 设置火焰对象的插入点
fireConfig.position = firePos;

// 设置火焰对象的viewer对象
fireConfig.viewer = viewer3D;

// 构造火焰对象
const fireEffect = new Glodon.Bimface.Plugins.ParticleSystem.FireEffect(fireConfig);

// 渲染场景
viewer3D.render();
```

## 销毁火焰效果

```javascript
// 销毁火焰效果
fireEffect.destroy();
```

## 调整火焰大小

```javascript
// 获取当前比例
const scale = fireEffect.getScale();

// 设置火焰粒子比例（0.5表示50%大小）
fireEffect.setScale(0.5);

// 设置火焰粒子比例（1表示100%大小）
fireEffect.setScale(1);

// 更新火焰参数配置
fireEffect.update();
```

## 更改火焰类型

```javascript
// 获取当前类型
const type = fireEffect.getType();

// 设置为火焰效果
fireEffect.setType(Glodon.Bimface.Plugins.ParticleSystem.FireType.Fire);

// 设置为烟雾效果
fireEffect.setType(Glodon.Bimface.Plugins.ParticleSystem.FireType.Smoke);

// 更新火焰参数配置
fireEffect.update();
```

## 调整烟雾浓度

```javascript
// 获取当前浓度
const concentration = fireEffect.getSmokeConcentration();

// 设置烟雾浓度（0-1之间，值越大浓度越高）
fireEffect.setSmokeConcentration(0.2);

// 更新火焰参数配置
fireEffect.update();
```

## 调整烟雾颜色

```javascript
// 设置烟雾颜色（RGB格式）
fireEffect.setColor({
    smokeColor: { r: 255, g: 255, b: 255 }
});

// 更新火焰参数配置
fireEffect.update();

// 获取当前颜色
const smokeColor = fireEffect.getColor();
```
