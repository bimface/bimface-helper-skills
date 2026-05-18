# 自动旋转
在BIMFACE中，可以使用自动旋转功能让模型场景自动旋转，便于全方位展示模型。

## 使用约束与说明
- 自动旋转功能仅在模型加载完成后生效，需要确认是否已经触发了ViewAdded事件
- 在切换不同的旋转模式（如从“中心旋转”切换到“绕点旋转”）前，或者开启新的旋转前，建议先调用viewer3D.stopAutoRotate() 清除历史状态

## 开启中心旋转
```javascript
// 开启自动旋转（绕场景中心）
// 参数：旋转速度（数值越大越快）
viewer3D.startAutoRotate(5);
```

## 关闭中心旋转
```javascript
// 停止自动旋转
viewer3D.stopAutoRotate();
```

## 开启绕点旋转
```javascript
// 设置旋转中心点
const rotatePoint = {
    x: -9470,
    y: 1308,
    z: 500
};

// 开启自动旋转（绕指定点）
// 参数1：旋转速度（数值越大越快）
// 参数2：旋转中心点坐标对象 {x, y, z}，不指定时默认为场景中心
viewer3D.startAutoRotate(5, rotatePoint);
```

## 关闭绕点旋转
```javascript
// 停止自动旋转
viewer3D.stopAutoRotate();
```

## 完整示例
```javascript
// 强烈建议在封装旋转交互时，采用先停止、后开启的模式，以避免底层状态冲突
function safeStartRotate(type, targetPoint = null) {
    // 1. 必须操作：无论当前是什么状态，先强制清除历史旋转
    viewer3D.stopAutoRotate(); 

    // 2. 根据传入的类型开启新旋转
    if (type === 'center') {
        viewer3D.startAutoRotate(5);
    } else if (type === 'point' && targetPoint) {
        viewer3D.startAutoRotate(5, targetPoint);
    }
}
```