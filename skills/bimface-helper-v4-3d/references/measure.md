# 模型测量（MeasureTool）

## 使用约束与说明

- **命名空间变更**：v4 中命名空间从 `Plugins`（复数）变更为 `Plugin`（单数），即 `Glodon.Bimface.Plugin.Measure.MeasureTool`
- **Config 类移除**：不再使用 `MeasureConfig` 类，构造函数直接接收配置对象
- 必须在 `ViewAdded` 事件触发后才能初始化测量对象
- 必须先设置测量类型，再开启测量

## 初始化测量对象

```javascript
// 直接传入配置对象（无需 MeasureConfig 类）
const measureTool = new Glodon.Bimface.Plugin.Measure.MeasureTool({
  viewer: viewer3D  // viewer3D 为 Viewer3D 实例
});
```

## 测量类型设置与开关

```javascript
// 设置测量类型
// 距离测量
measureTool.setMeasureType(Glodon.Bimface.Plugin.Measure.MeasureTypeOption.Distance);

// 最小距离测量
measureTool.setMeasureType(Glodon.Bimface.Plugin.Measure.MeasureTypeOption.MinimumDistance);

// 角度测量
measureTool.setMeasureType(Glodon.Bimface.Plugin.Measure.MeasureTypeOption.Angle);

// 标高测量
measureTool.setMeasureType(Glodon.Bimface.Plugin.Measure.MeasureTypeOption.Elevation);

// 开启测量（旧版为 enable）
measureTool.switchOn();

// 关闭测量（旧版为 exit）
measureTool.switchOff();
```

## 设置测量结果格式

`setResultFormat` 替代了旧版的 `setUnits` 和 `setPrecision` 两个方法，将单位和精度合并在一个接口中设置。

```javascript
measureTool.setResultFormat({
  distance: {
    unit: Glodon.Bimface.Common.Unit.LengthUnit.Meter,
    precision: 2
  },
  elevation: {
    unit: Glodon.Bimface.Common.Unit.LengthUnit.Meter,
    precision: 2
  },
  area: {
    unit: Glodon.Bimface.Common.Unit.AreaUnit.SquareMeter,
    precision: 2
  },
  angle: {
    precision: 2
  }
});
```

> **注意**：单位枚举在 v4 中为 `Glodon.Bimface.Common.Unit.LengthUnit` / `Glodon.Bimface.Common.Unit.AreaUnit`（单数 `Unit`），而非旧版的 `Glodon.Bimface.Common.Units.LengthUnits`（复数 `Units`）。

## 隐藏/显示测量数据

```javascript
// 隐藏所有测量数据（旧版为 hideAllItems）
measureTool.hideItems({ all: true });

// 显示所有测量数据（旧版为 showAllItems）
measureTool.showItems({ all: true });
```

## 清除测量数据

```javascript
// 清除所有测量数据（旧版为 clear）
measureTool.clearResults();
```

## 完整使用示例

```javascript
// 假设 viewer3D 已通过 ViewAdded 事件获取
let measureTool;

function initMeasure() {
  if (measureTool) {
    return;
  }

  // 初始化测量工具
  measureTool = new Glodon.Bimface.Plugin.Measure.MeasureTool({
    viewer: viewer3D
  });

  // 设置测量结果格式
  measureTool.setResultFormat({
    distance: {
      unit: Glodon.Bimface.Common.Unit.LengthUnit.Meter,
      precision: 2
    },
    elevation: {
      unit: Glodon.Bimface.Common.Unit.LengthUnit.Meter,
      precision: 2
    },
    area: {
      unit: Glodon.Bimface.Common.Unit.AreaUnit.SquareMeter,
      precision: 2
    },
    angle: {
      precision: 2
    }
  });

  console.log('测量工具初始化完成');
}

function startDistanceMeasure() {
  if (!measureTool) {
    initMeasure();
  }
  // 设置为距离测量并开启
  measureTool.setMeasureType(Glodon.Bimface.Plugin.Measure.MeasureTypeOption.Distance);
  measureTool.switchOn();
}

function stopMeasure() {
  if (measureTool) {
    measureTool.switchOff();
  }
}

function clearAllMeasurements() {
  if (measureTool) {
    measureTool.clearResults();
  }
}

function toggleMeasureVisibility(visible) {
  if (measureTool) {
    if (visible) {
      measureTool.showItems({ all: true });
    } else {
      measureTool.hideItems({ all: true });
    }
  }
}
```

## v3 → v4 方法对照表

| 功能 | v3（旧版） | v4（新版） |
|------|-----------|-----------|
| 构造 | `new Measure(new MeasureConfig())` | `new MeasureTool({viewer})` |
| 命名空间 | `Plugins.Measure` | `Plugin.Measure` |
| 开启 | `enable()` | `switchOn()` |
| 关闭 | `exit()` | `switchOff()` |
| 清除 | `clear()` | `clearResults()` |
| 隐藏全部 | `hideAllItems()` | `hideItems({all: true})` |
| 显示全部 | `showAllItems()` | `showItems({all: true})` |
| 设置单位 | `setUnits({...})` | `setResultFormat({...})`（合并） |
| 设置精度 | `setPrecision({...})` | `setResultFormat({...})`（合并） |
