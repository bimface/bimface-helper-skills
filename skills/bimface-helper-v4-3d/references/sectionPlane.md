# 剖切面（SectionPlaneTool）

## 使用约束与说明

- **命名空间变更**：v4 中命名空间从 `Plugins.Section`（复数）变更为 `Plugin.Section`（单数），类名从 `SectionPlane` 变更为 `SectionPlaneTool`
- **Config 类移除**：不再使用 `SectionPlaneConfig` 类，构造函数直接接收配置对象
- 必须在场景加载完成（触发 `ViewAdded` 事件）后才能初始化剖切面
- `setPlane` 替代旧版的 `setPositionByPlane` / `setPlane`，**不再提供 offset 参数**，如需偏移请直接在 origin 上调整
- `getStatus` / `setStatus` 替代旧版的 `getState` / `setState`

## 创建剖切面

```javascript
let sectionPlaneTool;

function initSectionPlane() {
  if (sectionPlaneTool) {
    return;
  }

  // 直接传入配置对象（无需 SectionPlaneConfig 类）
  sectionPlaneTool = new Glodon.Bimface.Plugin.Section.SectionPlaneTool({
    viewer: viewer3D,    // viewer3D 为 Viewer3D 实例
    enableHatch: true    // 是否开启剖切补面，默认为 true
  });

  console.log('剖切面初始化完成');
}
```

## 开关剖切面

```javascript
// 开启剖切面（旧版为 enable(true)）
sectionPlaneTool.switchOn();

// 关闭剖切面（旧版为 exit）
sectionPlaneTool.switchOff();
```

## 设置剖切面位置和朝向

`setPlane` 替代旧版的 `setPositionByPlane` 和 `setPlane`。v4 中不再提供 offset 参数 —— 如需偏移剖切面，请调整 origin 坐标（将法向量乘以偏移量加到 origin 上）。

```javascript
// 设置剖切面位置和朝向
sectionPlaneTool.setPlane({
  origin: { x: 0, y: 0, z: 0 },   // 剖切面中心点
  normal: { x: 0, y: 0, z: 1 }    // 剖切面法向量，剖切方向与向量方向一致
});

// 例如：沿 X 轴方向剖切，中心在 (50, 0, 0)
sectionPlaneTool.setPlane({
  origin: { x: 50, y: 0, z: 0 },
  normal: { x: 1, y: 0, z: 0 }
});

// 例如：向下倾斜 45° 剖切（在 XZ 平面）
// 如需偏移（旧版 offset），直接在 origin 上调整
const normalX = 0, normalY = -1, normalZ = 1;
const offsetDistance = 10;
sectionPlaneTool.setPlane({
  origin: {
    x: 0 + normalX * offsetDistance,
    y: 0 + normalY * offsetDistance,
    z: 0 + normalZ * offsetDistance
  },
  normal: { x: normalX, y: normalY, z: normalZ }
});
```

## 反转剖切方向

```javascript
// 反转剖切方向（旧版为 changeClipDirection）
sectionPlaneTool.reverseDirection();
```

## 剖切面的显隐与重置

```javascript
// 显示剖切面
sectionPlaneTool.showPlane();

// 隐藏剖切面
sectionPlaneTool.hidePlane();

// 重置剖切面状态为默认值
sectionPlaneTool.reset();
```

## 获取和设置剖切面状态

`getStatus` 是异步方法，返回 Promise。

```javascript
// 获取剖切面当前状态（返回 Promise）
const status = await sectionPlaneTool.getStatus();
console.log('剖切面状态:', status);
// status 对象结构：
// {
//   basePoint: { x, y, z },      // 基准点
//   center: { x, y, z },         // 中心位置
//   cubeSize: { x, y, z },       // 剖切面大小
//   enabled: true,               // 是否启用
//   isHatchByMaterial: false,    // 是否开启剖切材质补面
//   planeOffset: 0,              // 剖切面偏移量
//   position: { x, y, z },       // 剖切面位置
//   visible: true                // 是否显示
// }

// 设置剖切面状态
sectionPlaneTool.setStatus(status);
```

### 使用 Promise 方式

```javascript
// 获取剖切面当前状态
sectionPlaneTool.getStatus().then(status => {
  console.log('当前剖切面状态:', status);
});

// 保存状态后恢复
let savedStatus;
sectionPlaneTool.getStatus().then(status => {
  savedStatus = status;
  // ... 后续通过 setStatus(savedStatus) 恢复
});
```

## 事件监听

v4 新增了直接在工具实例上注册事件监听器的方式。

```javascript
// 监听剖切面变化事件
sectionPlaneTool.addEventListener(
  Glodon.Bimface.Plugin.Section.SectionPlaneEvent.PlaneChanged,
  () => {
    console.log('剖切面已变化');
  }
);

// 监听反转方向事件
sectionPlaneTool.addEventListener(
  Glodon.Bimface.Plugin.Section.SectionPlaneEvent.Reversed,
  () => {
    console.log('剖切面方向已反转');
  }
);

// 移除事件监听器（需传入同一个回调函数引用）
sectionPlaneTool.removeEventListener(
  Glodon.Bimface.Plugin.Section.SectionPlaneEvent.PlaneChanged,
  onPlaneChanged
);
```

## 完整使用示例

```javascript
let sectionPlaneTool;

// 在 ViewAdded 事件回调中初始化
function onViewAdded() {
  initSectionPlane();
}

function initSectionPlane() {
  if (sectionPlaneTool) {
    return;
  }

  sectionPlaneTool = new Glodon.Bimface.Plugin.Section.SectionPlaneTool({
    viewer: viewer3D,
    enableHatch: true
  });

  // 监听剖切面变化
  sectionPlaneTool.addEventListener(
    Glodon.Bimface.Plugin.Section.SectionPlaneEvent.PlaneChanged,
    () => {
      console.log('剖切面状态已更新');
    }
  );

  console.log('剖切面初始化完成');
}

function startSectionPlane() {
  if (!sectionPlaneTool) initSectionPlane();
  sectionPlaneTool.switchOn();
}

function stopSectionPlane() {
  if (sectionPlaneTool) sectionPlaneTool.switchOff();
}

// 设置 X 轴方向剖切
function sectionOnXAxis(cutPosition) {
  if (!sectionPlaneTool) return;
  sectionPlaneTool.setPlane({
    origin: { x: cutPosition, y: 0, z: 0 },
    normal: { x: 1, y: 0, z: 0 }
  });
}

// 设置 Y 轴方向剖切（水平剖切）
function sectionOnYAxis(cutPosition) {
  if (!sectionPlaneTool) return;
  sectionPlaneTool.setPlane({
    origin: { x: 0, y: cutPosition, z: 0 },
    normal: { x: 0, y: 1, z: 0 }
  });
}

// 设置 Z 轴方向剖切
function sectionOnZAxis(cutPosition) {
  if (!sectionPlaneTool) return;
  sectionPlaneTool.setPlane({
    origin: { x: 0, y: 0, z: cutPosition },
    normal: { x: 0, y: 0, z: 1 }
  });
}

function flipDirection() {
  if (sectionPlaneTool) sectionPlaneTool.reverseDirection();
}

async function logStatus() {
  if (!sectionPlaneTool) return;
  const status = await sectionPlaneTool.getStatus();
  console.log('剖切面状态:', status);
}
```

## v3 → v4 方法对照表

| 功能 | v3（旧版） | v4（新版） |
|------|-----------|-----------|
| 构造 | `new SectionPlane(new SectionPlaneConfig())` | `new SectionPlaneTool({viewer, enableHatch})` |
| 命名空间 | `Plugins.Section.SectionPlane` | `Plugin.Section.SectionPlaneTool` |
| 开启 | `enable(true)` | `switchOn()` |
| 关闭 | `exit()` | `switchOff()` |
| 设置面 | `setPlane(plane)` / `setPositionByPlane(origin, direction, offset)` | `setPlane({origin, normal})`（无 offset） |
| 反转方向 | `changeClipDirection()` | `reverseDirection()` |
| 获取状态 | `getState()`（同步） | `getStatus()`（返回 Promise） |
| 设置状态 | `setState(state)` | `setStatus(status)` |
| 事件监听 | `viewer.addEventListener(...)` | 工具实例 `addEventListener(...)` |
