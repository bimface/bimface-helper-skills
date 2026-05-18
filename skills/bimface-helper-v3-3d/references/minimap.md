# 写实小地图

## 使用约束与说明
- 在 `WebApplication3DConfig` 中配置
- 切换后需重新打开小地图面板才能生效

```javascript
const webAppConfig = new Glodon.Bimface.Application.WebApplication3DConfig();
webAppConfig.domElement = dom4Show;

// 开启写实小地图
webAppConfig.enableRealisticMiniMap = true;

const app3D = new Glodon.Bimface.Application.WebApplication3D(webAppConfig);
```
