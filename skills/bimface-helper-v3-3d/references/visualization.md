# 显示效果配置

## 使用约束与说明
- 显示效果配置在 `WebApplication3DConfig` 中设置
- 部分效果（如线框模式、SSAO）也可在运行时通过viewer3D接口动态切换
- 配置变更后需调用 `viewer3D.render()`

## WebApplication3DConfig 效果配置

```javascript
const webAppConfig = new Glodon.Bimface.Application.WebApplication3DConfig();
webAppConfig.domElement = dom4Show;

// 曝光度
webAppConfig.exposure = 0.4;

// 环境光照背景
webAppConfig.enableIBLBackground = true;

// 轮廓线
webAppConfig.enableBorderLine = false;

// 鼠标悬停
webAppConfig.enableHover = true;

// 渲染额外选项（通过visualization配置）
webAppConfig.visualization = {
    mode: Glodon.Bimface.Viewer.VisualizaionMode.Render,
    option: {
        enableWireframe: true,
        enableCSMShadow: false,
        exposure: 0,
        enableSSAO: true
    }
};
```

## 显示模式切换

| 模式值 | 说明 |
|--------|------|
| `VisualizaionMode.Render` | 渲染模式（有阴影，帧率中） |
| `VisualizaionMode.Performance` | 性能模式（无阴影，帧率高） |
| `VisualizaionMode.White` | 白模模式（白色，有阴影） |

## 接触阴影

```javascript
// 开启接触阴影
viewer3D.enableContactShadow(true);

// 关闭接触阴影
viewer3D.enableContactShadow(false);

// 查询状态
const isEnabled = viewer3D.isContactShadowEnabled();
```

## 镜头光晕

镜头光晕效果需要先设置天空盒。

```javascript
// 开启光晕
viewer3D.enableFlare(true);

// 关闭光晕
viewer3D.enableFlare(false);
viewer3D.render();
```
