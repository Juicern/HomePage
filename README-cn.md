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

## 学习

------

### 编程语言学习

------

#### C#

-------



##### Linq

-------

###### Any()

* 使用`Any()`可判断是否含有元素，如：`lstObject.Count > 0`与`lstObject.Any()`效果相同
* 在里面放置lambda表达式可以判断是否有满足此条件的元素，如：`lstObject.Any(x => x == key)`，可以判断`lstObject`中是否有等于`key`的元素。

------

#### JavaScript

------

#### Html

------

#### Css

------

#### Python

------

#### TypeScript

------

#### C++

------

#### Java

------

#### Sql

------

##### Join

下图分别展示了left join, right join, inner join, outer join相关的7种用法

![sql-join](./img/study/program%20language%20learning/sql/sql-join.png)

> 注意：left join与left outer join, right join 和 right outer join，没有任何差别 

##### top

在sql server中似乎没有limit这个用法，只能用top来取前n个数，如：

```MSSQL
select top 1 queston_id from survey_log
```

##### union

合并两个或多个select语句的结果集，结果集中的列名总是等于union中第一个select语句的列名

* union: 选取不同的值
* union all: 选取所有的值（可重复）

##### case 

下述为示例

```MSSQL
case sex
when '1' then '男'
when '2' then '女'
else ‘其他’ end
```



#### Mermaid

------

### 开发工具学习

------

#### Winform

------

##### 属性

* Anchor：根据form来进行对齐（注意是根据form，而不是根据父页面）

#### .Net

------

##### 一些错误

* "The underlying connection was closed: An unexpected error occurred on a send."

 解决方法：

将`HttpWebRequest`的`keep-alive`属性变为`true`，且修改安全协议类型：`ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12;`

#### Visual Studio Code

------

#### Visual Studio

------

##### 快捷键

* `ctrl`+`.` （打开修补程序）
* `ctrl`+`r`（重命名）
* `F12`（跳转到定义处）

#### WSL

------

#### Github

------

##### fork

在已经fork的项目中，若在自己本地已经有过更改，想要推送到被fork的项目中，则需先将本地的更改push，然后在github中打开项目，会提示与被fork的项目有不同，此时便会提示创建一个pull request，按照操作，便可将提交push到被fork的项目中，在repo的拥有者同意merge前，本地的push会一直传到这个request。若repo的拥有者已merge，则之后的更改，则需重复上述操作，重新创建一个pull request。

## LeetCode整理

------