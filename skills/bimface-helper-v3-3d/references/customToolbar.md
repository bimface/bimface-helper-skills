# 自定义工具条

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能操作工具条
- 按钮点击事件处理函数必须使用具名函数（参照 [监听事件](events.md) 规范）

## 自定义工具条按钮
需要在创建webApplication3DConfig时，配置需要显示的按钮
```javascript
// 创建webApplication3DConfig
const app3DConfig = new Glodon.Bimface.Application.WebApplication3DConfig();
app3DConfig.domElement = dom4Show;
// 配置需要显示的按钮
// “Home”为主视角，“RectangleSelect”为框选放大，“Measure”为测量，“Section”为剖切，“Walk”为漫游，“Map”为地图，“Property”为构件详情，“Setting”为设置，“Information”为基本信息，“FullScreen”为全屏
app3DConfig.Buttons = ['MainToolbar', 'CustomToolbarButton'];
```

## 获取工具条对象

```javascript
// 获取主工具条对象
const toolbar = app3D.getToolbar('MainToolbar');
```

## 创建自定义按钮

```javascript
// 创建按钮配置
const btnConfig = new Glodon.Bimface.UI.Button.ButtonConfig();

// 创建按钮对象
const btn = new Glodon.Bimface.UI.Button.Button(btnConfig);
```

## 设置按钮样式

```javascript
// 设置按钮的HTML内容，工具条样式均可以通过HTML+CSS进行样式自定义修改
btn.setHtml('<button style="width: 50px; height: 50px; color: white; font-size: 18px; background: rgba(0, 0, 0, 0); opacity: 0.6; border: none;">着色</button>');
```

## 设置按钮点击事件

```javascript
// 添加点击事件监听
const onBtnClick = function () {
    // 点击按钮后执行的操作
    const color = new Glodon.Web.Graphics.Color("#EE799F", 0.8);
    viewer3D.getModel().overrideComponentsColorById(["307240", "267327"], color);
    viewer3D.render();
};
btn.addEventListener('Click', onBtnClick);
```

## 插入按钮到工具条

```javascript
// 在工具条指定位置插入按钮（位置从0开始）
toolbar.insertControl(2, btn);
```
