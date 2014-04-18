---
layout: post
title: QtQuick中的布局和定位
tags: qtquick 定位 布局
---

在QML中有几种方法可以定义元素，以下仅仅是一个简介，请参考[QtQuick重要概念-定位]()了解更多。

### 手动定位
元素可以直接指定x,y坐标将它定位在屏幕上，这样它们将按照 [可视坐标系统规则]()定位在相对于它们的父元素的左上角。

使用组合绑定替代使用常量属性来进行相对定位也很容易通过指定合适的绑定坐标来完成。

```
	import QtQuick 2.0	

	Item {
	    width: 100; height: 100	

	    Rectangle {
	        // Manually positioned at 20,20
	        x: 20
	        y: 20
	        width: 80
	        height: 80
	        color: "red"
	    }
	}
```
![layout_and_positing](../../../images/qml-uses-layouts-direct.png)

### 锚点
Item类型提供了使用anchors锚绑定其他Item类型的能力。每个元素都有6条锚线：左右上下，垂直居中，水平居中。垂直的三条锚线可以锚定其他元素的三条垂直锚线，水平的也是如此。

详细介绍请看[定位与锚点]文档中的[锚属性]

```
	import QtQuick 2.0	

	Item {
	    width: 200; height: 200	

	    Rectangle {
	        // Anchored to 20px off the top right corner of the parent
	        anchors.right: parent.right
	        anchors.top: parent.top
	        anchors.margins: 20 // Sets all margins at once	

	        width: 80
	        height: 80
	        color: "orange"
	    }	

	    Rectangle {
	        // Anchored to 20px off the top center corner of the parent.
	        // Notice the different group property syntax for 'anchors' compared to
	        // the previous Rectangle. Both are valid.
	        anchors { horizontalCenter: parent.horizontalCenter; top: parent.top; topMargin: 20 }	

	        width: 80
	        height: 80
	        color: "green"
	    }
```

![layout_anchors.png](../../../images/qml-uses-layouts-anchors.png)

### 定位与布局
对多数场景需要定位一组类型到规则模式，QtQuick提供了一些定位类型。元素置于定位器后将被 自动定位到合适位置。比如Row将元素定位于水平相邻排成一行。

要了解更详细，请参考[元素定位]和[定位器类型]

```
	import QtQuick 2.0	

	Item {
	    width: 300; height: 100	

	    Row { // The "Row" type lays out its child items in a horizontal line
	        spacing: 20 // Places 20px of space between items	

	        Rectangle { width: 80; height: 80; color: "red" }
	        Rectangle { width: 80; height: 80; color: "green" }
	        Rectangle { width: 80; height: 80; color: "blue" }
	    }
	}
```

![layout_positioner.png](../../../images/qml-uses-layouts-positioners.png)

如果定位和大小需要改变，你可以在QtQuick中使用布局来适应。

