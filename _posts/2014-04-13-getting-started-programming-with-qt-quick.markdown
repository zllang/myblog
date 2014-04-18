---
layout: post
title：Qt Quick 入门
---

在这篇文档中我们将使用QML创造一个简单的文件编辑器，在阅读完本文档后你应该可以开始使用QML与QT C＋＋协同开发自己的应用了。

###使用QML构造用户界面
我们将要编写的简单文本编辑器具有打开、保存和一些文本处理功能。本教程将分为两部分。第一部分主要涉及设计应用程序的布局和使用QML语言定义一些行为，第二部分主要使用QT C＋＋实现打开和保存功能。使用[QT 元对象系统]我们可以将C＋＋的函数当作QML对象属性的在QML中使用，利用QML和QT C＋＋我们可以有效的分离界面逻辑和应用程序内部逻辑。

![../../../images/text_editor_overview.png]()

完成后的源代码在examples/quick/tutorials/gettingStartedQml目录下。如果你想看程序最终的样子你可以跳过教程直接运行它[Text Editor]().

教程章节：

-  定义按钮和菜单
-  实现菜单栏
-  创建文本编辑器
-  
-  使用QT C＋＋扩展QML

关于QML的语法和功能，包含在[QML Reference]()中。

###定义按钮和菜单
####基本组件之按钮
我们从构建一个按钮来开始。按钮的基本功能是包括一个响应鼠标的区域和一个广西标签。当用户点击按钮时它能响应用户的行为。

在QML中，最基础的可视元素是[Rectangle]()。它具有一些QML 属性控制它的外观和位置。

`

`

在上面代码中首先`import QtQuick 2.0`语句让qmlscene工具帮我们导入我们稍后需要用到的QML类型。这一行必须在每个QML中都存在。注意后面包含了Qt模型的版本号。

这个简单的矩形有一个惟一标识"simpleButton",它被赋给id。矩形的其他属性也都按照属性名跟冒号，然后跟上具体的值。如代码所示，灰色被赋给了矩形的color属性。同样我们给矩形的width和height赋上了值。

Text类型是不可编辑的文本字段，我们将它命名为"buttonLabel“。我们将一个文本赋给了它的text属性，让它当作文本字段的内容。这个文本标签包含在Rectangle矩形内。我们将Text对象的anchors.centerIn属性赋成parent，它的父对象simpleButton，所以文本在矩形内以居中显示。anchors可以绑定到其他元素的anchors，这样可以使布局更简单。

我们保存代码为SimpleButton.qml。然后将它当作参数运行qmlscene，我们就可以看到一个灰色的包含着文本的矩形。

![../../../images/simpleButton.png]()

为了实现按钮的点击功能，我们使用QML的事件绑定机制。QML的事件绑定很类似于Qt的信号槽机制。信号触发，然后连接的槽被执行。
`
code 2
code 2
`
我们在simpleButton中包含了一个MouseArea对象。MouseArea对象描述了一个检测鼠标运动的区域。在我们的按钮中，我们设定MouseArea的anchor充满它的父对象，就是我们的simpleButton。QML使用基本anchor的对局让一个元素可以anchor到其他的对象来创建可适应的布局。

当鼠标在MouseArea指定的区域内移动时有很多信号被触发，其中一个叫做onClicked，当鼠标在区域内并可以接收点击事件，默认左键点击时它被调用。我们可以绑定onClicked事件。在我们的例子中，当鼠标在区域内点击时console.log()输入一些文本。consol.log()函数是一个很有用的在调试时用来输出文本的工具。

现在simpleButton.qml实现在屏幕显示一个按钮，当鼠标点击时显示一些文本。

`
code 3
code3 
`
完整功能的按钮代码在Button.qml中，在文章中的代码片段省略了一部分代码，因为它们要么是更早以前的部分，或者无关当前代码中讨论的功能。

用户自定义属性使用`property type name`这种语法定义。在以上代码中buttonColor属性定义为color类型并且赋值为"lightblue"，buttonColor属性被用在后面代码以条件的方式指定按钮的填充颜色。提示：属性赋值可以使用＝号，但建议使用冒号。自定义属性允许内部的元素访问外层矩形空间的值。如int，string，real和variant类型都被称作QML的基本类型。

绑定onEntered和onExited信号和颜色，当鼠标置于按钮之上时按钮边框颜色变为黄色，当鼠标移出时还原颜色。

使用`singal name｀可以定义一个信号，在Button.qml中定义了buttonClick信号。所有定义的信号都有自动定义的执行句柄，名字为信号名前加on。所以onButtonClick就是buttonClick的执行句柄，onButtonCLick赋于一个执行动作。在这个例子中，onClick句柄只是简单的调用onButtonClick,它将显示一些文本。onButtonClick使外界物体能够轻松访问按钮的鼠标区域。**For example, items may have more than one MouseArea declarations and a buttonClick signal can make the distinction between the several MouseArea signal handlers better.**

现在我们了解了如何在QML中实现处理鼠标的移动。我们在Rectangle中包含了一个文本区域，定义了一些属性，然后实现了响应鼠标的移动。在对象中创建QML对象这种思想将会在这个文本编辑程序创建过程中重复运用。

这个按钮现在没用任何用处，除非我们将它当作一个组件来执行一些操作。在下一小节，我们次构建一个菜单包含若干个按钮。

![../../../btnlabel.png]()

####创建菜单页
到现阶段，我们涉及了如何在单个QML文件中创建对象和赋于行为。在这一节中，我们将涉及如果导入QML类型和使重用已经创建的组件来创建新的组件。

菜单显示是一个列表组件，每个项都可以执行一种操作。在QML中我们需要几个步骤创建菜单。首先我们创建一个窗口包含一些执行不同操作的按钮。菜单的代码在FileQmenu.qml中。

```
import QtQuick 2.0  
import "folderName"
import "script.js" as Script

```
上面的语法展示了如果使用import关键字。包括重用不在相同目录的javascript文件和QML文件。现在Button.qml和FileMenu.qml在相同的目录，所以我们不需要导入而直接使用Bbutton.qml，我们可以直接使用Button{}这样的语法来创建一个Button对象，就如同我们使用过的Rectangle{}一样。
```
如FileMenu.qml:

Row{
    
}



```
如FileMenu.qml我们定义了三个Button对象，它们被包含在一个Row类型中，它将它的子对象排成一排。在上面的代码中我们使用的Button对象和我们在Button.qml中声明的一样的。新的属性同赋值可以在创建对象时重新指定，它将覆盖Button.qml中的值。id为exitButton的按钮在按下时将会关闭并退出程序。注意，在Button.qml中的onButtonClick仅仅在exitButton对象中不会执行。

声明


