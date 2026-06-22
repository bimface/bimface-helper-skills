# 剖切盒（SectionBoxTool）

## 使用约束与说明

- **命名空间变更**：v4 中命名空间从 `Plugins.Section`（复数）变更为 `Plugin.Section`（单数），类名从 `SectionBox` 变更为 `SectionBoxTool`
- **Config 类移除**：不再使用 `SectionBoxConfig` 类，构造函数直接接收配置对象
- 必须在场景加载完成（触发 `ViewAdded` 事件）后才能初始化剖切盒
- 一个场景内只能创建一个剖切盒实例
- `getStatus` / `setStatus` 替代旧版的 `getState` / `setState`

## 创建剖切盒

```javascript
let sectionBoxTool;

function initSectionBox() {
  if (sectionBoxTool) {
    return;
  }

  // 直接传入配置对象（无需 SectionBoxConfig 类）
  sectionBoxTool = new Glodon.Bimface.Plugin.Section.SectionBoxTool({
    viewer: viewer3D,    // viewer3D 为 Viewer3D 实例
    enableHatch: true    // 是否开启剖切补面，默认为 true
  });

  console.log('剖切盒初始化完成');
}
```

## 开关剖切盒

```javascript
// 开启剖切盒（旧版为 enable(true)）
sectionBoxTool.switchOn();

// 关闭剖切盒（旧版为 exit）
sectionBoxTool.switchOff();
```

## 剖切盒的显隐与重置

```javascript
// 显示剖切盒
sectionBoxTool.showBox();

// 隐藏剖切盒
sectionBoxTool.hideBox();

// 重置剖切盒状态为默认值
sectionBoxTool.reset();
```

## 适配到指定包围盒

`fitToBoundingBox` 替代旧版的 `fitToModel`，支持将剖切盒调整到任意包围盒范围，使用更加灵活。

```javascript
// 将剖切盒调整到指定包围盒范围内
const boundingBox = {
  min: { x: -50, y: -10, z: -30 },
  max: { x: 80, y: 40, z: 60 }
};
sectionBoxTool.fitToBoundingBox(boundingBox);
```

> 若需适配到整个模型，可先从模型对象获取其包围盒后再传入。

## 反转剖切方向

```javascript
// 反转剖切方向（旧版为 changeClipDirection）
sectionBoxTool.reverseDirection();
```

## 获取和设置剖切盒状态

`getStatus` 是异步方法，返回 Promise。`getStatus` / `setStatus` 替代旧版的 `getState` / `setState`。

```javascript
// 获取剖切盒当前状态（返回 Promise）
const status = await sectionBoxTool.getStatus();
console.log('剖切盒状态:', status);
// status 对象结构：
// {
//   boundingBox: { min: {x, y, z}, max: {x, y, z} },
//   center: { x, y, z },
//   cubeSize: { x, y, z },
//   isHatchByMaterial: false,
//   isHatchEnabled: true,
//   isReversed: false,
//   visible: true
// }

// 设置剖切盒状态
sectionBoxTool.setStatus(status);
```

### 使用 Promise 方式

```javascript
// 获取剖切盒当前状态
sectionBoxTool.getStatus().then(status => {
  console.log('当前剖切盒状态:', status);
});

// 保存状态后恢复
let savedStatus;
sectionBoxTool.getStatus().then(status => {
  savedStatus = status;
  // ... 后续通过 setStatus(savedStatus) 恢复
});
```

## 事件监听

v4 新增了直接在工具实例上注册事件监听器的方式，而非通过 viewer 统一监听。

```javascript
// 监听剖切盒变化事件
sectionBoxTool.addEventListener(
  Glodon.Bimface.Plugin.Section.SectionBoxEvent.BoxChanged,
  () => {
    console.log('剖切盒已变化');
  }
);

// 监听反转方向事件
sectionBoxTool.addEventListener(
  Glodon.Bimface.Plugin.Section.SectionBoxEvent.Reversed,
  () => {
    console.log('剖切盒方向已反转');
  }
);

// 移除事件监听器（需传入同一个回调函数引用）
sectionBoxTool.removeEventListener(
  Glodon.Bimface.Plugin.Section.SectionBoxEvent.BoxChanged,
  onBoxChanged
);
```

## 完整使用示例

```javascript
let sectionBoxTool;

// 在 ViewAdded 事件回调中初始化
function onViewAdded() {
  initSectionBox();
}

function initSectionBox() {
  if (sectionBoxTool) {
    return;
  }

  sectionBoxTool = new Glodon.Bimface.Plugin.Section.SectionBoxTool({
    viewer: viewer3D,
    enableHatch: true
  });

  // 监听剖切盒变化
  sectionBoxTool.addEventListener(
    Glodon.Bimface.Plugin.Section.SectionBoxEvent.BoxChanged,
    () => {
      console.log('剖切盒状态已更新');
    }
  );

  console.log('剖切盒初始化完成');
}

function startSection() {
  if (!sectionBoxTool) initSectionBox();
  sectionBoxTool.switchOn();
}

function stopSection() {
  if (sectionBoxTool) sectionBoxTool.switchOff();
}

async function fitToModel() {
  if (!sectionBoxTool) return;
  // 假设 model 为已加载的模型对象，可通过 model.getBoundingBox() 获取包围盒
  const modelBoundingBox = await model.getBoundingBox();
  sectionBoxTool.fitToBoundingBox(modelBoundingBox);
}

function flipDirection() {
  if (sectionBoxTool) sectionBoxTool.reverseDirection();
}
```

## v3 → v4 方法对照表

| 功能 | v3（旧版） | v4（新版） |
|------|-----------|-----------|
| 构造 | `new SectionBox(new SectionBoxConfig())` | `new SectionBoxTool({viewer, enableHatch})` |
| 命名空间 | `Plugins.Section.SectionBox` | `Plugin.Section.SectionBoxTool` |
| 开启 | `enable(true)` | `switchOn()` |
| 关闭 | `exit()` | `switchOff()` |
| 适配模型 | `fitToModel()` | `fitToBoundingBox(boundingBox)` |
| 反转方向 | `changeClipDirection(isReverse)` | `reverseDirection()` |
| 获取状态 | `getState()`（同步） | `getStatus()`（返回 Promise） |
| 设置状态 | `setState(state)` | `setStatus(status)` |
| 事件监听 | `viewer.addEventListener(...)` | 工具实例 `addEventListener(...)` |
