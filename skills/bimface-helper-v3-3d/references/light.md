# 光照效果

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能操作光照
- 通过 `viewer3D.getLightManager()` 获取光源管理器
- 添加/修改光源后需调用 `lightManager.update()`，场景变更后调用 `viewer3D.render()`

## 聚光灯

### 添加聚光灯

```javascript
const lightManager = viewer3D.getLightManager();

const spotLightConfig = new Glodon.Bimface.Light.SpotLightConfig();
spotLightConfig.viewer = viewer3D;
spotLightConfig.position = { x: 27, y: -1614, z: 9753 };
spotLightConfig.target = { x: 27, y: -1614, z: 0 };
spotLightConfig.intensity = 3;
spotLightConfig.distance = 10000;
spotLightConfig.penumbra = 0.1;
spotLightConfig.angle = Math.PI / 6;
spotLightConfig.color = new Glodon.Web.Graphics.Color(255, 215, 0, 1);
spotLightConfig.shadow = true;

const spotLight = new Glodon.Bimface.Light.SpotLight(spotLightConfig);
lightManager.addLight(spotLight);
lightManager.update();
viewer3D.render();
```

### 控制聚光灯开关

```javascript
const spotLightId = spotLightConfig.id;

// 关闭聚光灯
lightManager.enableLightsById([spotLightId], false);
lightManager.update();

// 开启聚光灯
lightManager.enableLightsById([spotLightId], true);
lightManager.update();
```

### 控制所有灯光

```javascript
// 关闭场景内所有灯光
lightManager.enableAllLights(false);
lightManager.update();

// 开启场景内所有灯光
lightManager.enableAllLights(true);
lightManager.update();
```

## 方向光

```javascript
const lightManager = viewer3D.getLightManager();
const directionalLight = lightManager.getAllDirectionalLights()[0];

// 根据经纬度和时间设置太阳光方向
const latLon = { lat: 31.0, lon: 120.0 };
const date = new Date(2020, 7, 31, 7, 0, 0);
directionalLight.setDirectionByCondition(latLon, date);
directionalLight.enableShadow(true);
```

### 光照模拟（动态时间推进）

```javascript
// 每100ms推进0.1小时，模拟太阳运动
const timer = setInterval(function () {
    directionalLight.setDirectionByCondition(latLon, date);
    viewer3D.render();
    date.setTime(date.getTime() + 1000 * 0.1 * 3600);

    if (date.getHours() > 16) {
        clearInterval(timer);
        date.setHours(7);
    }
}, 100);
```

## 环境光照（IBL）

基于图像的光照(Image-Based Lighting)，提供多种预设环境光样式。

```javascript
const iblManagerConfig = new Glodon.Bimface.Plugins.IBL.IBLManagerConfig();
iblManagerConfig.viewer = viewer3D;
iblManagerConfig.style = Glodon.Bimface.Plugins.IBL.IBLStyle.SunsetGrass;

const iblManager = new Glodon.Bimface.Plugins.IBL.IBLManager(iblManagerConfig);
```

### 切换IBL样式

```javascript
iblManager.setStyle(Glodon.Bimface.Plugins.IBL.IBLStyle.CloudySky);
```

### IBLStyle 样式列表

| 样式值 | 说明 |
|--------|------|
| `CloudySky` | 蓝天白云 |
| `CityNightView` | 城市夜景 |
| `OpenField` | 空旷园区 |
| `LawnScene` | 草坪效果 |
| `SunsetGrass` | 日落草坪 |
