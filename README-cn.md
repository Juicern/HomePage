# 欢迎来到Ricky-Chu的主页

[English version](./README.md)

------

以下所写，暂时只是大纲

## 前言

------

生活丰富多彩

## 项目

------

### 河海大学计算机类课程资料整理

------

repo地址：[河海大学计算机类课程资料整理](https://github.com/Ricky-Chu/lib-cs-hhu)

本项目由本人亲自整理，汇集了各方资源，精心挑选，多次更改排版与内容，找到最完整和整洁的课程资料，供目前在读的本科生下载学习，相信你点开不会后悔。

### 河海大学自动健康打卡

------

repo地址：[河海大学自动健康打卡](https://github.com/Ricky-Chu/HHUHealthCheckinGUI)

本项目是本人某日在github中闲逛时发现，fork之后也修改了几处地方，使得其更适合目前本科生的健康打卡，目前会有卡顿现象。

### 家居人体感应

------

repo地址：[家居人体感应](https://github.com/Ricky-Chu/BodyAlarm)

本项目是本人大二下学期的安卓课程设计项目，实现了物体经过时报警，并提供丰富的设置界面，适合初学者学习。

### 河海大学校园码生成工具

------

repo地址：[河海大学校园码生成工具](https://github.com/Ricky-Chu/GetOutTheHell)

本项目建立于新冠肺炎元年，因学校需要出示出行码才可出入，但申请又极其困难与无理，故建此项目，此项目为微信小程序，出行码可通过设置来改变信息，样式与学校官方一模一样。

### 高德地图简单调用

------

repo地址：[高德地图简单调用](https://github.com/Ricky-Chu/GaodeMap-APITest)

本项目是本人大四上学期认识实习课程的课程设计项目，界面精美而易用，完美符合任课老师所有需求。

### 钉钉界面简单调用

------

repo地址：[钉钉界面简单调用](https://github.com/Ricky-Chu/WebApiTest)

本项目为公司新增的调用钉钉接口的需求，故学习并掌握了钉钉接口的基本调用，能利用钉钉提供的接口进行简便的企业管理，适合初学者练习与调试。

## 理论学习

------

### [设计模式](./DesignPatterns-cn.md)

### 计算机网络

------



### 算法与数据结构

-------



### 数据库原理

-------



### 操作系统

------



## 编程语言学习

------

### C#

-------



#### Linq

-------

##### Any()

* 使用`Any()`可判断是否含有元素，如：`lstObject.Count > 0`与`lstObject.Any()`效果相同
* 在里面放置lambda表达式可以判断是否有满足此条件的元素，如：`lstObject.Any(x => x == key)`，可以判断`lstObject`中是否有等于`key`的元素。

------

### GoLang

------

当标识符以一个大写字符出头，代表public；当以一个小写字符出头，代表protected

`defer`:延迟执行，按defer的顺序倒序执行

```go
package main

import "fmt"

func hello() {
	fmt.Print("hello ")
}

func world() {
	fmt.Println("world")
}

func main() {
	defer world()
	hello()
}
```

string在go中不是一个对象，传入的时候按照形参传入。





### JavaScript

------

### Html

------

### Css

------

### Python

------

bit_length: 获取当前数字的用二进制表示的长度

### TypeScript

------

### C++

------

### Java

------

### Sql

------

### Mermaid

------

## 开发工具学习

------

### Winform

------

#### 属性

* Anchor：根据form来进行对齐（注意是根据form，而不是根据父页面）

#### 跨窗体操作控件

使用委托来实现跨窗体调用控件

效果描述：有两个窗体，FORM1（一个名为“打开form2”的button控件）和FORM2（一个名为“改变form1颜色“的button控件）。启动时，FORM1中点击button控件“打开form2””使FORM2显示出来。点击FORM2中的“改变form1颜色”后，Form1中颜色改变。

1. 在Form2里面

   首先声明一个委托和一个委托实例

   * Form2类外：

     ```c#
     public delegate void ChangeFormColor(bool topmost);
     ```

   * Form2类里：

     ```c#
     public event ChangeFormColor ChangeColor;
     ```

   Form2的按钮事件中调用委托：

   ```c#
   private void button1_Click(object sender, EventArgs e)
   {
       ChangeColor(true);//执行委托实例
   }
   ```

2. 在Form2里面：

   button控件"打开form2"的click事件中有下列代码：

   ```c#
   Form2 f = new Form2();
   f.ChangeColor += new ChangeFormColor(f_ChangeColor);
   f.Show();
   ```

   f.ChangeColor += new ChangeFormColor(f_ChangeColor);
   这句最关键，你输入到+=之后，按两下Tab，他会自动给你生成回调函数，如下：

   ```c#
   void f_ChangeColor(bool topmost)
   {
       this.BackColor = Color.LightBlue;
       this.Text = "改变成功";
   }
   ```

3. 完整代码

   Form1：

   ```c#
   using System;
   using System.Drawing;
   using System.Windows.Forms;
    
   namespace 跨窗体调用控件
   {
       public partial class Form1 : Form
       {
           public Form1()
           {
               InitializeComponent();
           }
           private void button1_Click(object sender, EventArgs e)
           {
               Form2 f = new Form2();
               f.ChangeColor += new ChangeFormColor(f_ChangeColor);
               f.Show();
           }
           void f_ChangeColor(bool topmost)
           {
               this.BackColor = Color.LightBlue;
               this.Text = "改变成功";
           }
       }
   }
   ```

   Form2:

   ```c#
   using System;
   using System.Windows.Forms;
    
   namespace 跨窗体调用控件
   {
       public delegate void ChangeFormColor(bool topmost);
       public partial class Form2 : Form
       {
           public Form2()
           {
               InitializeComponent();
           }
           public event ChangeFormColor ChangeColor;
           private void button1_Click(object sender, EventArgs e)
           {
               ChangeColor(true);//执行委托实例
           }
       }
   }
   ```

   

### .Net

------

#### 一些错误

* "The underlying connection was closed: An unexpected error occurred on a send."

 解决方法：

将`HttpWebRequest`的`keep-alive`属性变为`true`，且修改安全协议类型：`ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12;`

### Visual Studio Code

------

### Visual Studio

------

#### 快捷键

* `ctrl`+`.` （打开修补程序）
* `ctrl`+`r`（重命名）
* `F12`（跳转到定义处）

#### 一些错误

##### 部分文件打不开窗体设计器 变成类.cs

解决方法：在csproj文件下修改对应的`<Compile>`，将文件节点改为`<SubForm>Form</SubForm>`

```
<Compile Include="Form1.cs">
	<SubType>Form</SubType>
</Compile>
```

##### 自定义类没有加入工具箱

解决方法：在Visual Studio中，选择工具，选项，Windows窗体设计器，常规，自动填充工具箱设为True

在工具箱处右键，点击`选择项`，将自定义控件的dll加入





### WSL

------

### Github

------

#### fork

在已经fork的项目中，若在自己本地已经有过更改，想要推送到被fork的项目中，则需先将本地的更改push，然后在github中打开项目，会提示与被fork的项目有不同，此时便会提示创建一个pull request，按照操作，便可将提交push到被fork的项目中，在repo的拥有者同意merge前，本地的push会一直传到这个request。若repo的拥有者已merge，则之后的更改，则需重复上述操作，重新创建一个pull request。

## LeetCode整理

------