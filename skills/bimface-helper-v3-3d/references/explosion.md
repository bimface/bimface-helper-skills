# 楼层爆炸

## 使用约束与说明
- 必须在`ViewAdded`事件触发后、`model3D`可用时才能调用
- `model3D.getFloors()` 是异步回调，楼层列表必须在回调中获取后才能用于爆炸，如果没有楼层信息也无法使用楼层爆炸功能
- 必须在viewer3DConfig或app3DConfig中设置 `enableExplosion = true` ，才能真正开启爆炸效果
- 爆炸/清除后需调用 `viewer3D.render()` 刷新场景

## 开启楼层爆炸

```javascript
// 定义楼层爆炸的方向，缺省值为 {x: 0, y: 0, z: 1}
const direction = { x: 1, y: 1, z: 1 };

// 获取楼层列表
let floorList = new Array();
model3D.getFloors(function (data) {
    if (!data) {
        console.log('No floor data.');
        return;
    }
    for (let i = 0; i < data.length; i++) {
        floorList.push(data[i].id);
    }
    // 设置楼层爆炸
    // 参数1: 爆炸系数（数值越大楼层之间分离越开）
    // 参数2: 楼层ID列表
    // 参数3: 爆炸方向
    model3D.setFloorExplosion(3, floorList, direction);
    viewer3D.render();
});
```

## 关闭楼层爆炸

```javascript
// 清除楼层爆炸效果
model3D.clearFloorExplosion();
viewer3D.render();
```
