---
layout: post
title: QML编码约定
---
本文主要包含QML我们文档和示例程序中的编码约定，并且也建议你这么做。

###QML对象定义
纵观我们的文档和示例程序，QML对象属性都是按如下顺序结构定义。

- id
- property 定义
- signale 定义
- javascript 函数
- 对象属性赋值
- 子对象创建
- states 状态列表
- transitions 转换动画

为了增强可读情我们将不同的部分以空行分隔。如下假设一个QML照片对象看起来是这样：

```
	Rectangle {
	    id: photo                                               // 第一行id，让我们很容易找到对象，orz

	    property bool thumbnail: false                          // property定义
	    property alias image: photoImage.source	

	    signal clicked                                          // signal 	

	    function doSomething(x)                                 // javascript 函数
	    {
	        return x + photoImage.width
	    }	

	    color: "gray"                                           // 对象属性
	    x: 20; y: 20; height: 150                               // 相关的属性分组
	    width: {                                                // 复杂赋值
	        if (photoImage.width > 200) {
	            photoImage.width;
	        } else {
	            200;
	        }
	    }	

	    Rectangle {                                             // 子对象 
	        id: border
	        anchors.centerIn: parent; color: "white"	

	        Image { id: photoImage; anchors.centerIn: parent }
	    }	

	    states: State {                                         // 状态表
	        name: "selected"
	        PropertyChanges { target: border; color: "red" }
	    }	

	    transitions: Transition {                               // 转换表
	        from: ""; to: "selected"
	        ColorAnimation { target: border; duration: 200 }
	    }
	}
```

### 属性分组
如果操作一组属性，建议使用分组中括号代替点以提高可读性。

如下示例：

```
	Rectangle {
	    anchors.left: parent.left; anchors.top: parent.top; anchors.right: parent.right; anchors.leftMargin: 20
	}	

	Text {
	    text: "hello"
	    font.bold: true; font.italic: true; font.pixelSize: 20; font.capitalization: Font.AllUppercase
	}
```

应该写成如下:

```
	Rectangle {
	    anchors { left: parent.left; top: parent.top; right: parent.right; leftMargin: 20 }
	}	

	Text {
	    text: "hello"
	    font { bold: true; italic: true; pixelSize: 20; capitalization: Font.AllUppercase }
	}
```
###列表

如果列表中只包含一个元素，我们通常省略掉中括号。

如下示例，一个组件非常有可能只有一个状态，

```
	states: [
	    State {
	        name: "open"
	        PropertyChanges { target: container; width: 200 }
	    }
	]
```

我们一般写成这样.

```
	states: State {
	        name: "open"
	        PropertyChanges { target: container; width: 200 }
	    }
```

### javascript代码

如果脚本代码只是单个表达式，我们建议写在行内，如下：

```
    Rectangle { color: "blue"; width: parent.width / 3 }
```

如果脚本包含几行，我们通常使用一个块包含它，如下：

```
	Rectangle {
	    color: "blue"
	    width: {
	        var w = parent.width / 3
	        console.debug(w)
	        return w
	    }
	}
```

如果脚本多于几行，或者让它能够被不同对象使用，我们建议创建一个函数然后调用它，如下：

```
	function calculateWidth(object)
	{
	    var w = object.width / 3
	    // ...
	    // more javascript code
	    // ...
	    console.debug(w)
	    return w
	}	

	Rectangle { color: "blue"; width: calculateWidth(parent) }
```
  
  对于长脚本，我们将它定义成函数，然后放在它自己的javascript文件中，然后像以下这样导入:
  
```
  	import "myscript.js" as Script	

	Rectangle { color: "blue"; width: Script.calculateWidth(parent) }
```
  
