---
layout: post
title: QML文件简介
tag:  qml qt
---

##目录
- QML文件结构、

> - QML语言基本语法 
   
- 使用QML文件定义类型
- 资源加载和网络透明
- 作用域和命名解析

QML文件是一种满足QML语法的纯文本文件，一个文件字义了一个QML对象类型，QML文件通常从本地或远程加载一个qml文件,并在代码中构建。在QML文件中使用Component或者在C++中使用QQmlComponent可以定义一个实例对象的类型。另外，如果对象类型以一个特别的类型名显式的暴露给QML类型系统，那么在其他文件中可以直接使用这个类型。

在文档中定义可重复使用的QML对象类型的能力是一个重要的推动者，以允许客户端写模块化的，具有很强的可读性和可维护的代码。

##QML文件结构
一个QML文件包含两部分，导入部分和声明部分。导入部分的导入语句定义了哪些QML类型和javascript资源可以在本文件中使用。对象声明部分定义了当本文件所定义对象被实例化时所需要创建的对象树。

一个简单的示例如下:

        import QtQuick 2.0

        Rectangle {
             width: 300
             height: 200
             color: "blue"
        }
参考[QML文件结构](qthelp://org.qt-project.qtqml.521/qtqml/qtqml-documents-structure.html)了解更多。

*未翻译链接指向qt5.2.1帮助文档 assistant。*
####QML语言语法
文件中对象声明部分必须使用合适的QML语法指定一个有效的对象层次结构。对象声明中可能包含自定义的对象属性。对象方法属性被指定为javascript函数，对象属性被指定为[属性绑定表达式](qthelp://org.qt-project.qtqml.521/qtqml/qtqml-syntax-propertybinding.html)。

请查看[QML语法](qthelp://org.qt-project.qtqml.521/qtqml/qtqml-syntax-basics.html)了解关于合法语法，参考[整合QML和javascript](qthelp://org.qt-project.qtqml.521/qtqml/qtqml-javascript-topic.html)了更多关于QML语法的知识。
##使用QML文件定义类型

如前一节中简要的描述，一个文件就隐含定义一个QML对象类型。QML的核心原则是定义，然后重用对象类型的能力。它提高了QML代码的可维护性，增强了对象层次声明的可读性，并促进了UI定义和逻辑实现之间的分离。

如下代码 ，客户端开发者使用一个文件定义了一个Button类型。

        // Button.qml
        import QtQuick 2.0        

        Rectangle {
            width: 100; height: 100
            color: "red"        

            MouseArea {
                anchors.fill: parent
                onClicked: console.log("Button clicked!")
            }
        }

Button类型可以按如下方式在应用程序中使用：

         // application.qml
        import QtQuick 2.0        

        Column {
            Button { width: 50; height: 50 }
            Button { x: 50; width: 100; height: 50; color: "blue" }
            Button { width: 50; height: 50; radius: 8 }
        }
![example1.png](../../../images/qmlfilestruct01.png)

请参考文档[定义对象类型](qthelp://org.qt-project.qtqml.521/qtqml/qtqml-documents-definetypes.html)了解更多内容。

##资源加载和网络透明
要注意QML的网络透明很重要，应用程序可以像使用本地文件一样导入一个远程路径中的文件。实际上任何url属性可以被指定为本地或远程URL，QML引擎将负责所有的网络通信。

请参考[网络透明](qthelp://org.qt-project.qtqml.521/qtqml/qtqml-documents-networktransparency.html)文档了解更多内容。
##作用域和命名解析
文件中的表达式通常涉及到对象或对象的属性，而且可以定义多个对象，不同对象可能具有相同名称的属性，一些预定义符号解析的主义必须通过QML定义。
请参考[作用域和命名解析](qthelp://org.qt-project.qtqml.521/qtqml/qtqml-documents-scope.html)了解更多。
