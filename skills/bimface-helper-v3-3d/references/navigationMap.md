# 导航地图

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能创建导航地图
- 导航地图需要绑定一个DOM容器，确保该DOM元素在页面中已存在

## 创建导航地图对象

### 按高度生成导航地图

```javascript
// 配置存放导航地图的DOM容器
const domElement = document.querySelector(".mapDom");

// 创建导航地图配置
const navigationMapCfg = new Glodon.Bimface.Plugins.NavigationMap.NavigationMapConfig();
navigationMapCfg.viewer = viewer3D;
navigationMapCfg.domElement = domElement;

// 设置为按指定高度生成剖面导航地图的形式
navigationMapCfg.type = "SetProfile";

// 设置生成剖面的高度（必填）
navigationMapCfg.height = 5240;

// 构造导航地图
const navigationMap = new Glodon.Bimface.Plugins.NavigationMap.NavigationMap(navigationMapCfg);
```

### 按相机高度生成导航地图

```javascript
// 配置存放导航地图的DOM容器
const domElement = document.querySelector(".mapDom");

// 创建导航地图配置
const navigationMapCfg = new Glodon.Bimface.Plugins.NavigationMap.NavigationMapConfig();
navigationMapCfg.viewer = viewer3D;
navigationMapCfg.domElement = domElement;

// 设置为自动根据相机高度生成导航地图
navigationMapCfg.type = "AutoProfile";

// 生成导航地图
const navigationMap = new Glodon.Bimface.Plugins.NavigationMap.NavigationMap(navigationMapCfg);
```

### 外部图片导航地图

```javascript
// 配置存放导航地图的DOM容器
const domElement = document.querySelector(".mapDom");

// 创建导航地图配置
const navigationMapCfg = new Glodon.Bimface.Plugins.NavigationMap.NavigationMapConfig();
navigationMapCfg.viewer = viewer3D;
navigationMapCfg.domElement = domElement;

// 设置为导入关联导航地图形式，支持通过url传入图片
navigationMapCfg.type = "Relevance";

// 设置图片URL
navigationMapCfg.url = "https://example.com/map.png";

// 设置模型定位锚点（分别和导航地图point1、point2点对应）
navigationMapCfg.modelAnchors = {
    point1: {
        x: -10082.694908451429,
        y: 2085.900442177565,
        z: 5050.00000000000006,
    },
    point2: {
        x: 7061.564481749251,
        y: -5401.61180149294,
        z: 4499.991210937501,
    },
};

// 设置导航地图锚点（以左上角为原点的像素坐标）
navigationMapCfg.mapAnchors = {
    point1: { x: 30, y: 83 },
    point2: { x: 287, y: 196 },
};

// 生成导航地图
const navigationMap = new Glodon.Bimface.Plugins.NavigationMap.NavigationMap(navigationMapCfg);
```

## 更新外部图片导航地图

```javascript
// 更新地图绑定的图片
navigationMap.associateModel({
    url: "https://example.com/newMap.png",
    mapAnchors: {
        point1: { x: 466, y: 165 },
        point2: { x: 165, y: 568 },
    },
    modelAnchors: {
        point1: { x: -5856.350844991256, y: 6357.73046875, z: 5000 },
        point2: { x: 8418.8017578125, y: -4260.144554473609, z: 5000 },
    },
});
```

## 销毁导航地图

```javascript
// 销毁导航地图
navigationMap.destroy();

// 隐藏导航地图容器
document.querySelector(".mapDom").style.display = "none";
```
