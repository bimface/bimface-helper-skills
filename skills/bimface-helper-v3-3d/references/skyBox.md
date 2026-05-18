# 天空盒与背景色

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能设置背景色和天空盒
- 设置背景色/天空盒后需调用 `viewer3D.render()` 刷新场景
- 天空盒管理器和环境光照(IBL)管理器不可同时生效，后设置的会覆盖前者

## 单色背景

```javascript
const color = new Glodon.Web.Graphics.Color(214, 224, 235, 1);
viewer3D.setBackgroundColor(color);
viewer3D.render();
```

## 渐变背景

```javascript
const topColor = new Glodon.Web.Graphics.Color('#BFEFFF', 0.8);
const bottomColor = new Glodon.Web.Graphics.Color('#949494', 0.8);
viewer3D.setBackgroundColor(topColor, bottomColor);
viewer3D.render();
```

## 天空盒效果

```javascript
const skyBoxManagerConfig = new Glodon.Bimface.Plugins.SkyBox.SkyBoxManagerConfig();
skyBoxManagerConfig.viewer = viewer3D;
skyBoxManagerConfig.style = Glodon.Bimface.Plugins.SkyBox.SkyBoxStyle.CloudySky;

// 自定义天空盒的六面贴图（仅 Customized 样式需要）
skyBoxManagerConfig.customizedImage = {
    front: 'https://static.bimface.com/attach/xxx_posz.jpg',
    back: 'https://static.bimface.com/attach/xxx_negz.jpg',
    left: 'https://static.bimface.com/attach/xxx_negx.jpg',
    right: 'https://static.bimface.com/attach/xxx_posx.jpg',
    top: 'https://static.bimface.com/attach/xxx_posy.jpg',
    bottom: 'https://static.bimface.com/attach/xxx_negy.jpg'
};

const skyBoxManager = new Glodon.Bimface.Plugins.SkyBox.SkyBoxManager(skyBoxManagerConfig);
viewer3D.render();
```

## 切换天空盒样式

```javascript
skyBoxManager.setStyle(Glodon.Bimface.Plugins.SkyBox.SkyBoxStyle.BlueSky);
```

### SkyBoxStyle 样式列表

| 样式值 | 说明 |
|--------|------|
| `BlueSky` | 蓝天效果 |
| `CloudySky` | 多云效果 |
| `CloudPuresky` | 有云晴天 |
| `DarkNight` | 暗夜效果 |
| `Galaxy` | 星空效果 |
| `SunsetPuresky` | 落日蓝天 |
| `MistyMorning` | 迷雾清晨 |
| `Customized` | 自定义效果（需配合customizedImage） |

## 关闭天空盒

```javascript
skyBoxManager.enableSkyBox(false);
viewer3D.render();
```

## 背景图

通过设置全透明背景色，让底层DOM的背景图透出。

```javascript
// 在 WebApplication3DConfig 中设置alpha=0
webAppConfig.backgroundColor = [{
    color: new Glodon.Web.Graphics.Color(25, 28, 33, 0)
}];

// 通过CSS设置DOM背景图
const viewEl = document.querySelector('.bf-view');
viewEl.style.background = "url('背景图URL')";
viewEl.style.backgroundSize = 'cover';
```
