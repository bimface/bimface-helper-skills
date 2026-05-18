# 立体锚点效果
在BIMFACE中，可以使用立体锚点效果在模型场景中创建棱锥形状的悬浮锚点，常用于标记重要位置或设备。

## 构造立体锚点效果

### 创建锚点管理器
```javascript
// 构造并配置三维锚点管理器配置项
const anchorMngConfig = new Glodon.Bimface.Plugins.Anchor.AnchorManagerConfig();

// 关联viewer
anchorMngConfig.viewer = viewer3D;

// 构造三维锚点管理器
const anchorMng = new Glodon.Bimface.Plugins.Anchor.AnchorManager(anchorMngConfig);
```

### 创建棱锥锚点
```javascript
// 构造棱锥锚点的配置项
const prismPointConfig = new Glodon.Bimface.Plugins.Anchor.PrismPointConfig();

// 设置棱锥锚点的悬浮位置
prismPointConfig.position = {
    x: 13907767.112090306,
    y: 26204006.755251624,
    z: 208873.42092020495
};

// 设置棱锥锚点悬浮动画循环一次的时间（毫秒）
prismPointConfig.duration = 1500;

// 设置棱锥锚点的大小
prismPointConfig.size = 35000;

// 构造棱锥锚点对象
const prismPoint = new Glodon.Bimface.Plugins.Anchor.PrismPoint(prismPointConfig);

// 将锚点添加到锚点管理器
anchorMng.addItem(prismPoint);
```

## 完整示例
```javascript
// 构造并配置三维锚点管理器配置项
const anchorMngConfig = new Glodon.Bimface.Plugins.Anchor.AnchorManagerConfig();
anchorMngConfig.viewer = viewer3D;

// 构造三维锚点管理器
const anchorMng = new Glodon.Bimface.Plugins.Anchor.AnchorManager(anchorMngConfig);

// 构造棱锥锚点的配置项
const prismPointConfig = new Glodon.Bimface.Plugins.Anchor.PrismPointConfig();

// 设置棱锥锚点的悬浮位置
prismPointConfig.position = {
    x: 13907767.112090306,
    y: 26204006.755251624,
    z: 208873.42092020495
};

// 设置棱锥锚点悬浮动画循环一次的时间（毫秒）
prismPointConfig.duration = 1500;

// 设置棱锥锚点的大小
prismPointConfig.size = 35000;

// 构造棱锥锚点对象
const prismPoint = new Glodon.Bimface.Plugins.Anchor.PrismPoint(prismPointConfig);

// 将锚点添加到锚点管理器
anchorMng.addItem(prismPoint);
```

## 配置参数说明

### AnchorManagerConfig
| 参数 | 说明 |
|------|------|
| viewer | 视图对象 |

### PrismPointConfig
| 参数 | 说明 |
|------|------|
| position | 锚点位置 {x, y, z} |
| duration | 悬浮动画循环时间（毫秒） |
| size | 锚点大小 |

## 常用方法说明
| 方法 | 说明 |
|------|------|
| new AnchorManager | 创建锚点管理器 |
| anchorMng.addItem | 添加锚点到管理器 |
| new PrismPoint | 创建棱锥锚点 |
