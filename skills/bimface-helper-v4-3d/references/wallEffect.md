# 电子围墙效果（WallEffect）

> ⚠️ **注意**：本文档基于 v3 API 文档 `bimface-helper-v3-3d` 和 v4 通用迁移模式（命名空间 `Plugins→Plugin`、Config 类去除、Color 类路径变更等）编写，具体 API 请以 `参考/接口文档/WallEffect-v4.pdf` 和 `参考/接口文档/Wall-v4.pdf` 为准。

## 使用约束与说明

- **命名空间变更**：v4 中命名空间从 `Plugins.Animation` 变更为 `Plugin.WallEffect`
- **Config 类移除**：不再使用 `WallEffectConfig` 类，构造函数直接接收配置对象
- **Color 类变更**：v4 使用 `Glodon.Bimface.Common.Graphics.Color`，不再使用 `Glodon.Web.Graphics.Color`
- 必须在 `ViewAdded` 事件触发后才能创建电子围墙效果
- 修改效果参数（方向、持续时间等）后需调用 `update()` 方法才能生效

---

## 创建电子围墙效果

```javascript
viewer3D.addEventListener(
  Glodon.Bimface.Viewer.Viewer3DEvent.ViewAdded,
  () => {
    // 直接传入配置对象（无需 WallEffectConfig 类）
    const wallEffect = new Glodon.Bimface.Plugin.WallEffect.WallEffect({
      viewer: viewer3D,           // Viewer3D 实例
      height: 80000,              // 围墙高度
      stretch: true,              // 是否拉伸
      duration: 3500,             // 动画持续时间（毫秒）
      direction: {
        type: "Tangent",          // 运动方式：沿着路径切线方向（Tanget/Normal）
        reverse: false             // 运动方向：false 为逆时针
      },
      path: [                     // 围墙路径坐标点数组
        { x: 13023609.96, y: 25777457.26, z: 50.62 },
        { x: 13122366.00, y: 25839575.57, z: 50.62 },
        { x: 13315590.84, y: 25970551.37, z: 50.62 },
        { x: 13276959.06, y: 26028989.38, z: 50.62 },
        { x: 13155465.07, y: 26169861.96, z: 50.62 }
      ],
      // v4 使用 Glodon.Bimface.Common.Graphics.Color
      color: new Glodon.Bimface.Common.Graphics.Color(50, 211, 166, 0.8)
    });

    viewer3D.render();
  }
);
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

**方向类型说明：**
- `"Tangent"` — 沿着路径切线方向
- `"Normal"` — 沿着路径法线方向（向上）

## 更新围墙颜色

```javascript
// 动态修改围墙颜色
wallEffect.setColor(
  new Glodon.Bimface.Common.Graphics.Color(255, 100, 50, 0.9)
);
wallEffect.update();
```

## 控制围墙效果

```javascript
// 启动电子围墙动画
wallEffect.start();

// 停止电子围墙动画
wallEffect.stop();
```

---

## 完整示例

```javascript
let wallEffect;

viewer3D.addEventListener(
  Glodon.Bimface.Viewer.Viewer3DEvent.ViewAdded,
  () => {
    // 定义围墙路径坐标（从模型场景中获取）
    const boundaryPath = [
      { x: 13023609.96, y: 25777457.26, z: 50.62 },
      { x: 13122366.00, y: 25839575.57, z: 50.62 },
      { x: 13315590.84, y: 25970551.37, z: 50.62 },
      { x: 13276959.06, y: 26028989.38, z: 50.62 },
      { x: 13155465.07, y: 26169861.96, z: 50.62 }
    ];

    wallEffect = new Glodon.Bimface.Plugin.WallEffect.WallEffect({
      viewer: viewer3D,
      path: boundaryPath,
      height: 80000,
      stretch: true,
      duration: 3500,
      color: new Glodon.Bimface.Common.Graphics.Color(50, 211, 166, 0.8),
      direction: {
        type: "Tangent",
        reverse: false
      }
    });

    viewer3D.render();

    // 3 秒后切换为法线方向
    setTimeout(() => {
      wallEffect.setDirection({
        type: "Normal",
        reverse: false
      });
      wallEffect.setDuration(1000);
      wallEffect.update();
    }, 3000);
  }
);
```

---

## v3 → v4 对照

| 项目 | v3（旧版） | v4（新版） |
|------|-----------|-----------|
| 命名空间 | `Plugins.Animation` | `Plugin.WallEffect` |
| Config 类 | `new WallEffectConfig()` 设置属性 | 不再需要，直接传对象 |
| 构造方式 | `new WallEffect(wallEffectConfig)` | `new WallEffect({viewer, path, height, ...})` |
| Color 类路径 | `Glodon.Web.Graphics.Color` | `Glodon.Bimface.Common.Graphics.Color` |
