# 发光效果

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能添加发光效果
- 发光效果需要先调用 `viewer3D.enableGlowEffect(true)` 开启全局开关
- 修改发光效果后需调用 `viewer3D.render()` 刷新场景
- 多模型场景下需通过 `viewer3D.getModel(modelId)` 获取目标模型后调用接口

## 整体发光

`type: "body"` 使构件整体呈现发光效果。

```javascript
// 开启发光全局开关
viewer3D.enableGlowEffect(true);

model3D.setGlowEffectById(
    ['24', '25', '26', '27', '28', '33', '34', '35', '36'],
    {
        type: 'body',
        color: new Glodon.Web.Graphics.Color(255, 229, 89, 1),
        intensity: 0.4,
        spread: 3
    }
);

viewer3D.render();
```

### 清除整体发光

```javascript
model3D.removeGlowEffectById(['24', '25', '26', '27', '28', '33', '34', '35', '36']);
viewer3D.render();
```

## 轮廓线发光

`type: "outline"` 仅使构件轮廓线发光。

```javascript
viewer3D.enableGlowEffect(true);

model3D.setGlowEffectById(
    ['58', '59', '60', '61'],
    {
        type: 'outline',
        color: new Glodon.Web.Graphics.Color(255, 255, 160, 1),
        intensity: 0.4,
        spread: 3
    }
);

viewer3D.render();
```

### 清除轮廓线发光

```javascript
model3D.removeGlowEffectById(['58', '59', '60', '61']);
viewer3D.render();
```

### GlowEffect 配置属性

| 属性 | 类型 | 说明 |
|------|------|------|
| `type` | String | `"body"` 整体发光，`"outline"` 轮廓线发光 |
| `color` | Glodon.Web.Graphics.Color | 发光颜色 |
| `intensity` | Number | 发光强度，取值范围0.3~1.0 |
| `spread` | Number | 扩散程度 |

## 辉光效果（BloomEffect）

适用于多模型场景中对指定模型的构件添加辉光。

```javascript
const bloomEffectConfig = new Glodon.Bimface.Plugins.Effect.BloomEffectConfig();
bloomEffectConfig.ids = [
    {
        modelId: '模型ID',
        objectIds: ['19', '23', '25', '34', '46', '32', '41', '52', '56']
    }
];
bloomEffectConfig.intensity = 0.3;
bloomEffectConfig.spread = 4;
bloomEffectConfig.viewer = viewer3D;

const bloomEffect = new Glodon.Bimface.Plugins.Effect.BloomEffect(bloomEffectConfig);
viewer3D.render();
```

### 清除辉光效果

```javascript
bloomEffect.clear();
viewer3D.render();
```

## 清除所有发光

```javascript
// 清除指定模型的所有发光效果
model3D.clearGlowEffect();

// 关闭发光全局开关
viewer3D.enableGlowEffect(false);
viewer3D.render();
```
