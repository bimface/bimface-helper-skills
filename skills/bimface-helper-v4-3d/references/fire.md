# 火焰效果（Fire）

> ⚠️ **注意**：本文档基于 v3 API 文档 `bimface-helper-v3-3d` 和 v4 通用迁移模式（命名空间 `Plugins→Plugin`、Config 类去除等）编写，具体 API 请以 `参考/接口文档/Fire-v4.pdf` 为准。

## 使用约束与说明

- **命名空间变更**：v4 中命名空间从 `Plugins.ParticleSystem` 变更为 `Plugin.Fire`
- **Config 类移除**：不再使用 `FireEffectConfig` 类，构造函数直接接收配置对象
- 必须在 `ViewAdded` 事件触发后才能创建火焰效果
- 修改火焰参数（大小、类型、浓度、颜色）后需调用 `update()` 方法才能生效

---

## 创建火焰效果

```javascript
viewer3D.addEventListener(
  Glodon.Bimface.Viewer.Viewer3DEvent.ViewAdded,
  () => {
    // 直接传入配置对象（无需 FireEffectConfig 类）
    const fireEffect = new Glodon.Bimface.Plugin.Fire.FireEffect({
      viewer: viewer3D,                                    // Viewer3D 实例
      position: { x: -2194.954, y: -7739.213, z: 10527.306 }  // 火焰插入点（世界坐标）
    });

    // 渲染场景
    viewer3D.render();
  }
);
```

## 调整火焰大小

```javascript
// 获取当前比例
const scale = fireEffect.getScale();

// 设置火焰粒子比例（0.5 表示 50% 大小）
fireEffect.setScale(0.5);
fireEffect.update();

// 设置火焰粒子比例（1 表示 100% 大小）
fireEffect.setScale(1);
fireEffect.update();
```

## 更改火焰类型

```javascript
// 获取当前类型
const type = fireEffect.getType();

// 设置为火焰效果
fireEffect.setType(Glodon.Bimface.Plugin.Fire.FireType.Fire);
fireEffect.update();

// 设置为烟雾效果
fireEffect.setType(Glodon.Bimface.Plugin.Fire.FireType.Smoke);
fireEffect.update();
```

## 调整烟雾浓度

```javascript
// 获取当前浓度
const concentration = fireEffect.getSmokeConcentration();

// 设置烟雾浓度（0-1 之间，值越大浓度越高）
fireEffect.setSmokeConcentration(0.2);
fireEffect.update();
```

## 调整烟雾颜色

```javascript
// v4 使用 Glodon.Bimface.Common.Graphics.Color
fireEffect.setColor({
  smokeColor: { r: 255, g: 255, b: 255 }
});
fireEffect.update();

// 获取当前颜色
const smokeColor = fireEffect.getColor();
```

## 销毁火焰效果

```javascript
// 完全销毁火焰效果
fireEffect.destroy();
```

## 控制火焰效果

```javascript
// 启动火焰效果
fireEffect.start();

// 停止火焰效果
fireEffect.stop();
```

---

## 完整示例

```javascript
let fireEffect;

viewer3D.addEventListener(
  Glodon.Bimface.Viewer.Viewer3DEvent.ViewAdded,
  () => {
    // 创建火焰效果
    fireEffect = new Glodon.Bimface.Plugin.Fire.FireEffect({
      viewer: viewer3D,
      position: { x: 5000, y: 10000, z: 8000 }
    });

    viewer3D.render();

    // 3 秒后切换为烟雾效果
    setTimeout(() => {
      fireEffect.setType(Glodon.Bimface.Plugin.Fire.FireType.Smoke);
      fireEffect.setSmokeConcentration(0.3);
      fireEffect.setColor({
        smokeColor: { r: 128, g: 128, b: 128 }
      });
      fireEffect.update();
    }, 3000);
  }
);

// 销毁火焰（在页面卸载或切换场景时）
// fireEffect.destroy();
```

---

## v3 → v4 对照

| 项目 | v3（旧版） | v4（新版） |
|------|-----------|-----------|
| 命名空间 | `Plugins.ParticleSystem` | `Plugin.Fire` |
| 类名 | `FireEffect` | `FireEffect`（待 PDF 确认） |
| Config 类 | `new FireEffectConfig()` 设置属性 | 不再需要，直接传对象 `{viewer, position}` |
| 构造方式 | `new FireEffect(fireConfig)` | `new FireEffect({viewer, position})` |
