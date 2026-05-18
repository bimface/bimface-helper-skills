# 飞线效果

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能创建飞线效果
- 飞线由曲线(SplineCurve)和曲线动画(CurveAnimation)两部分组成

## 创建飞线

```javascript
const extObjMng = new Glodon.Bimface.Plugins.ExternalObject.ExternalObjectManager(viewer3D);
const color = new Glodon.Web.Graphics.Color(255, 237, 141, 1.0);
const style = { lineType: 'Continuous', lineStyle: null };

const curve = new Glodon.Bimface.Plugins.Geometry.SplineCurve(
    [{ x: 100, y: 200, z: 300 }, { x: 400, y: 500, z: 600 }],
    color,
    3,
    style
);

// 设置曲线贴图
curve.setMap({
    src: 'https://static.bimface.com/attach/xxx.png',
    enableColorOverride: true
}, function () {
    extObjMng.loadObject({ name: 'flyCurve', object: curve });
});

// 拉伸曲线使其有厚度
curve.stretch();
```

## 飞线动画

```javascript
const curveAnimationConfig = new Glodon.Bimface.Plugins.Animation.CurveAnimationConfig();
curveAnimationConfig.viewer = viewer3D;
curveAnimationConfig.curves = [curve];
curveAnimationConfig.time = 1200;  // 单位是毫秒
curveAnimationConfig.loop = true;
curveAnimationConfig.type = 'flow';

const curveAnimation = new Glodon.Bimface.Plugins.Animation.CurveAnimation(curveAnimationConfig);
curveAnimation.play();
```

## 停止飞线动画

```javascript
curveAnimation.stop();
```
