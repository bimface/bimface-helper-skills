# 天气效果

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能创建天气效果
- 开启/关闭天气效果后需调用 `viewer3D.render()` 刷新场景

## 雪景效果

```javascript
// 构造雪景效果配置项
const snowConfig = new Glodon.Bimface.Plugins.WeatherEffect.SnowConfig();

// 关联viewer
snowConfig.viewer = viewer3D;

// 设置天空的灰暗程度（0-1）
snowConfig.darkness = 0.3;

// 设置雪的密度：雪停:0; 小雪:1; 中雪:2; 大雪:3
snowConfig.density = 2;

// 设置积雪厚度（0-1）
snowConfig.thickness = 0.4;

// 构造雪景效果对象
const snow = new Glodon.Bimface.Plugins.WeatherEffect.Snow(snowConfig);

// 开启雪景效果
snow.enableEffect(true);

// 渲染场景
viewer3D.render();
```

### 关闭雪景效果

```javascript
// 关闭雪景效果
snow.enableEffect(false);

// 渲染场景
viewer3D.render();
```

## 雾天效果

```javascript
// 构造雾天效果配置项
const fogConfig = new Glodon.Bimface.Plugins.WeatherEffect.FogConfig();

// 关联viewer
fogConfig.viewer = viewer3D;

// 设置天空的灰暗程度（0-1）
fogConfig.darkness = 0.2;

// 设置光线衰减的指数（值越小则场景雾化速度越慢）
fogConfig.lightAttenuation = 3.0;

// 设置雾的颜色
fogConfig.fogColor = new Glodon.Web.Graphics.Color(255, 255, 255, 0.5);

// 设置最远可视范围（单位：mm）
fogConfig.visualDistance = 400000;

// 构造雾天效果对象
const fog = new Glodon.Bimface.Plugins.WeatherEffect.Fog(fogConfig);

// 开启雾天效果
fog.enableEffect(true);

// 渲染场景
viewer3D.render();
```

### 关闭雾天效果

```javascript
// 关闭雾天效果
fog.enableEffect(false);

// 渲染场景
viewer3D.render();
```

## 雨天效果

```javascript
// 构造雨天效果配置项
const rainConfig = new Glodon.Bimface.Plugins.WeatherEffect.RainConfig();

// 关联viewer
rainConfig.viewer = viewer3D;

// 设置天空的灰暗程度（0-1）
rainConfig.darkness = 0.2;

// 设置雨的密度：雨停:0; 小雨:1; 中雨:2; 大雨:3
rainConfig.density = 3;

// 构造雨天效果对象
const rain = new Glodon.Bimface.Plugins.WeatherEffect.Rain(rainConfig);

// 开启雨天效果
rain.enableEffect(true);

// 渲染场景
viewer3D.render();
```

### 关闭雨天效果

```javascript
// 关闭雨天效果
rain.enableEffect(false);

// 渲染场景
viewer3D.render();
```
