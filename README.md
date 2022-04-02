`ButtonEx` 组件，一个对 `UnityEngine.UI.Button` Unity的扩展。实现了除`onClick`外的常用按钮交互事件，定制了Inspector

   ![ButtonEx](./Doc/ButtonExPreview.png)

**项目结构：**

- *Assets*
  - ButtonEx
    - ButtonEx.cs	//核心
    - Editor
      - ButtonExEditor.cs	//核心
    - Demo    //演示用的

**项目地址：**[ButtonEx - SouthBegonia](https://github.com/SouthBegonia/ButtonEx)



# 设计目的

## 1.实现按钮的其他交互逻辑

`UnityEngine.UI.Button`只实现了`onCLick`点击事件，而实际项目中，按下、按起、长按、双击等交互逻辑也会用到。解决办法：

-  `ButtonEx`继承`UnityEngine.UI.Button`或 `UnityEngine.UI.Selectable`后，实现 `IPointerDownHandler`等几个接口和长按逻辑即可

## 2.实现按钮的业务逻辑

通常点击按钮后，可能要播放按钮的点击音效、动效。为避免程序在各业务代码内编写重复代码，也方便公开相关参数（如播放音效的参数，动效参数）给非程序人员调控，因此相关业务逻辑完全可以放在`ButtonEx`内。解决办法(源码内就不具体编写了)：

- 可以在`ButtonEx.Awake()`时开启`onClick`的监听：`onClick.AddListener(PlaySound())`，但要注意是否会被其他代码`RemoveAllListeners()`
- 也可以在`m_OnClick.Invoke()`执行前自主调用`PlaySound()`

## 3.ButtonEx的引入，不能影响原有Button

若不是开坑初期就引入`ButtonEx`，就避免不了此问题：原有代码内就有大量对`Button`的使用，若新加的`ButtonEx`不是在`Button`上实现的，就得全部替换代码，还得告知其他程序人员该用`ButtonEx`或是`Button`，很大的工作量。解决办法：

- `ButtonEx`选择继承自`UnityEngine.UI.Button`，而不是`UnityEngine.UI.Selectable`，这么做的话还得编写`ButtonEx`的Inspector脚本~~(狗蛋：这就是代价)~~

## 4.定制`ButtonEx`的Inspector页面

若是不定制Inspector页面，虽然也可以根据`Attributes`属性做些美化~~(也不是不能用)~~，但考虑到未来可能有更多扩展，也为了方便非程序人员的直观使用，因此可以特殊定制其Inspector页面。解决办法：

- 因为`ButtonEx`是继承`UnityEngine,UI.Button`的，则其扩展脚本`ButtonExEditor`就继承自`SelectableEditor`进行定制



# 现有功能

- 常用按钮交互事件
  - 按下（`onDown`）、按起（`onUp`）、进入（`onEnter`）、移出（`onExit`）、长按（`onLongClick`）、双击（`onDoubleClick`）
- Editor模式下`Button`无损转换为`ButtonEx`（开关在组件右上角三个点里）



# 参考文章

- [ButtonEx - mob-sakai](https://github.com/mob-sakai/ButtonEx)
- [Unity按钮拓展 - 纯纯小萌新](https://blog.csdn.net/weixin_45549268/article/details/112919617)
