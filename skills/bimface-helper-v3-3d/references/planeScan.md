# 平面扫描效果

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能创建平面扫描效果
- 可通过 `show()` / `hide()` 控制显示与隐藏
- 可配合 `Material` 插件为扫描平面添加自定义材质贴图

## 创建平面扫描效果

```javascript
// 构造平面扫描效果配置项
let planeScanEffectConfig = new Glodon.Bimface.Plugins.Animation.PlaneScanEffectConfig();

// 配置Viewer对象
planeScanEffectConfig.viewer = viewer3D;

// 设置扫描方向
planeScanEffectConfig.direction = { x: 0.6, y: 0.8, z: 0 };

// 设置扫描持续时间（毫秒）
planeScanEffectConfig.duration = 2000;

// 设置扫描边界（封闭多边形坐标数组）
planeScanEffectConfig.boundary = [
  { x: 13023609.960575795, y: 25777457.255968206, z: 50.61713809092208 },
  { x: 13122365.995315686, y: 25839575.569788456, z: 50.61713809194653 },
  { x: 13315590.835127937, y: 25970551.36890273, z: 50.61713809410652 },
  { x: 13276959.063045746, y: 26028989.384943567, z: 50.61713809507025 },
  { x: 13155465.068261532, y: 26169861.95716057, z: 50.61713809737892 },
  // ... 更多坐标点，首尾应闭合
];

// 设置扫描颜色（RGBA）
planeScanEffectConfig.color = new Glodon.Web.Graphics.Color(50, 211, 166, 1.0);

// 设置材质对象（可选）
planeScanEffectConfig.material = material;

// 设置材质与颜色的混合参数（0-1之间，值越大材质越明显）
planeScanEffectConfig.blendingRatio = 0.3;

// 构造平面扫描效果对象
let planeScanEffect = new Glodon.Bimface.Plugins.Animation.PlaneScanEffect(planeScanEffectConfig);
```

## 显示/隐藏平面扫描效果

```javascript
// 显示平面扫描效果
planeScanEffect.show();

// 隐藏平面扫描效果
planeScanEffect.hide();
```

## Material材质插件

材质插件用于为扫描平面等效果添加自定义纹理贴图。

```javascript
// 构造材质配置项
let materialConfig = new Glodon.Bimface.Plugins.Material.MaterialConfig();

// 关联Viewer对象
materialConfig.viewer = viewer3D;

// 设置材质贴图URL
materialConfig.src = "https://static.bimface.com/attach/3e8cedfed7a04c8e9cb115ce192e209f_big.png";

// 构造材质对象
let material = new Glodon.Bimface.Plugins.Material.Material(materialConfig);
```
