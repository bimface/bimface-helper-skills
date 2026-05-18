# 剖切盒

## 使用约束与说明
- 剖切盒的初始化必须在场景加载完成（触发ViewAdded事件）之后进行
- 一个场景内只能创建一个剖切盒实例
- 所有改变剖切盒视觉状态的操作后，必须调用viewer3D.render()刷新场景才能生效

## 创建剖切盒

```JavaScript
// 声明为模块级或全局变量，供后续显隐控制使用
let sectionBox;

function initSectionBox() {
  // 防御性设计：避免重复创建
  if (sectionBox) {
    return;
  }

  // 剖切盒配置类
  const sectionBoxConfig = new Glodon.Bimface.Plugins.Section.SectionBoxConfig();
  // 关联全局场景查看器
  sectionBoxConfig.viewer = viewer3D;
  
  // 初始化剖切盒对象
  sectionBox = new Glodon.Bimface.Plugins.Section.SectionBox(sectionBoxConfig);
  console.log("剖切盒初始化完成");
}
```

## 剖切盒的显隐与重置

```JavaScript
// 确保已初始化
if (!sectionBox) {
  initSectionBox();
}

// 显示剖切盒
sectionBox.showBox();
viewer3D.render();

// 隐藏剖切盒
sectionBox.hideBox();
viewer3D.render();

// 重置剖切盒
sectionBox.reset();
viewer3D.render();
```

## 剖切盒自适应到模型

```JavaScript
if (sectionBox) {
  // 剖切盒自适应到当前显示的实体模型
  sectionBox.fitToModel();
  viewer3D.render();
}
```

## 获取和设置剖切盒状态

```JavaScript
let savedSectionState;
// 1. 获取并保存当前剖切状态
if (sectionBox) {
  savedSectionState = sectionBox.getState();
  console.log("剖切状态已保存", savedSectionState);
}

// 2. 恢复此前保存的剖切状态
if (sectionBox && savedSectionState) {
  sectionBox.setState(savedSectionState);
  viewer3D.render(); // 状态恢复后必须刷新
}
```
