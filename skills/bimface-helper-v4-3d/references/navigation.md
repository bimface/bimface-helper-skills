# 导航模式

## 使用约束与说明

- 导航管理器通过 `viewer3D.getNavigationManager()` 获取
- 支持三种导航模式：第一人称、第三人称、环绕模式
- 各模式只能同时激活一种，切换时新设置会覆盖旧的导航状态
- 全局变量名统一使用：`viewer3D`、`app`、`model`、`camera`

---

## 获取导航管理器

```javascript
var navigationManager = viewer3D.getNavigationManager();
```

---

## 第一人称模式：setFirstPersonMode

以第一人称视角在场景中漫游：

```javascript
navigationManager.setFirstPersonMode({
    speedFactor: 1.0,      // 移动速度系数，默认 1.0
    hitDetection: true,    // 是否启用碰撞检测，默认 true
    gravity: true          // 是否启用重力，默认 true
});
```

---

## 第三人称模式：setThirdPersonMode

以第三人称视角在场景中漫游：

```javascript
navigationManager.setThirdPersonMode({
    movementMode: "Walk",             // "Walk"（行走）或 "Run"（奔跑）
    position: { x: 0, y: 0, z: 50 }, // 初始位置
    avatar: "OfficeMale",            // 可选，角色模型
    hitDetection: true,              // 是否启用碰撞检测
    gravity: true                    // 是否启用重力
});
```

### movementMode 取值

| 值 | 说明 |
|----|------|
| `"Walk"` | 行走模式，移动速度较慢 |
| `"Run"` | 奔跑模式，移动速度较快 |

### avatar 取值（可选）

| 值 | 说明 |
|----|------|
| `"OfficeMale"` | 办公室男性角色 |
| `"ConstructionWorker"` | 建筑工人角色 |

不传 `avatar` 参数时，第三人称模式不显示角色模型。

---

## 环绕模式：setOrbitMode

切换回默认的模型旋转观察模式：

```javascript
navigationManager.setOrbitMode();
```

环绕模式是默认模式，用于围绕模型旋转、平移和缩放观察。

---

## 完整示例

```javascript
var viewer3D, app, model, camera;
var navigationManager;

// 模型加载完成后获取导航管理器
function initNavigation() {
    navigationManager = viewer3D.getNavigationManager();

    // 默认使用环绕模式
    navigationManager.setOrbitMode();
}

// 切换到第一人称漫游
function switchToFirstPerson() {
    navigationManager.setFirstPersonMode({
        speedFactor: 1.5,
        hitDetection: true,
        gravity: true
    });
}

// 切换到第三人称漫游
function switchToThirdPerson() {
    navigationManager.setThirdPersonMode({
        movementMode: "Walk",
        position: camera.getStatus().position,  // 从当前相机位置开始
        avatar: "ConstructionWorker",
        hitDetection: true,
        gravity: true
    });
}

// 切换回环绕模式
function switchToOrbit() {
    navigationManager.setOrbitMode();
}
```
