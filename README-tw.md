# 歡迎來到Ricky-Chu的主頁

[English version](./README.md)

------

以下所寫，暫時只是大綱

## 前言

------

生活豐富多彩

## 專案

------

### 河海大學計算機類課程資料整理

------

repo地址：[河海大學計算機類課程資料整理](https://github.com/Ricky-Chu/lib-cs-hhu)

本專案由本人親自整理，彙集了各方資源，精心挑選，多次更改排版與內容，找到最完整和整潔的課程資料，供目前在讀的本科生下載學習，相信你點開不會後悔。

### 河海大學自動健康打卡

------

repo地址：[河海大學自動健康打卡](https://github.com/Ricky-Chu/HHUHealthCheckinGUI)

本專案是本人某日在github中閒逛時發現，fork之後也修改了幾處地方，使得其更適合目前本科生的健康打卡，目前會有卡頓現象。

### 家居人體感應

------

repo地址：[家居人體感應](https://github.com/Ricky-Chu/BodyAlarm)

本專案是本人大二下學期的安卓課程設計專案，實現了物體經過時報警，並提供豐富的設定介面，適合初學者學習。

### 河海大學校園碼生成工具

------

repo地址：[河海大學校園碼生成工具](https://github.com/Ricky-Chu/GetOutTheHell)

本專案建立於新冠肺炎元年，因學校需要出示出行碼才可出入，但申請又極其困難與無理，故建此專案，此專案為微信小程式，出行碼可透過設定來改變資訊，樣式與學校官方一模一樣。

### 高德地圖簡單呼叫

------

repo地址：[高德地圖簡單呼叫](https://github.com/Ricky-Chu/GaodeMap-APITest)

本專案是本人大四上學期認識實習課程的課程設計專案，介面精美而易用，完美符合任課老師所有需求。

### 釘釘介面簡單呼叫

------

repo地址：[釘釘介面簡單呼叫](https://github.com/Ricky-Chu/WebApiTest)

本專案為公司新增的呼叫釘釘介面的需求，故學習並掌握了釘釘介面的基本呼叫，能利用釘釘提供的介面進行簡便的企業管理，適合初學者練習與除錯。

## 理論學習

------

### [設計模式](./DesignPatterns-cn.md)

### 計算機網路

------



### 演算法與資料結構

-------



### 資料庫原理

-------



### 作業系統

------



## 程式語言學習

------

### C#

-------



#### Linq

-------

##### Any()

* 使用`Any()`可判斷是否含有元素，如：`lstObject.Count > 0`與`lstObject.Any()`效果相同
* 在裡面放置lambda表示式可以判斷是否有滿足此條件的元素，如：`lstObject.Any(x => x == key)`，可以判斷`lstObject`中是否有等於`key`的元素。

------

### JavaScript

------

### Html

------

### Css

------

### Python

------

bit_length: 獲取當前數字的用二進位制表示的長度

### TypeScript

------

### C++

------

### Java

------

### Sql

------

#### Join

下圖分別展示了left join, right join, inner join, outer join相關的7種用法

![sql-join](README-tw.assets/sql-join.png)

> 注意：left join與left outer join, right join 和 right outer join，沒有任何差別 

#### top

在sql server中似乎沒有limit這個用法，只能用top來取前n個數，如：

```MSSQL
select top 1 queston_id from survey_log
```

#### union

合併兩個或多個select語句的結果集，結果集中的列名總是等於union中第一個select語句的列名

* union: 選取不同的值
* union all: 選取所有的值（可重複）

#### case 

下述為示例

```mssql

```



```MSSQL
case sex
when '1' then '男'
when '2' then '女'
else ‘其他’ end
```

#### where

約束宣告，使用Where來約束來自資料庫的資料，where是在結果返回之前起作用的，且where中不能使用聚合函式

#### having

過濾宣告，是在查詢返回結果集以後對查詢結果進行的過濾操作，在having中可以使用聚合函式

### Mermaid

------

## 開發工具學習

------

### Winform

------

#### 屬性

* Anchor：根據form來進行對齊（注意是根據form，而不是根據父頁面）

#### 跨窗體操作控制元件

使用委託來實現跨窗體呼叫控制元件

效果描述：有兩個窗體，FORM1（一個名為“開啟form2”的button控制元件）和FORM2（一個名為“改變form1顏色“的button控制元件）。啟動時，FORM1中點選button控制元件“開啟form2””使FORM2顯示出來。點選FORM2中的“改變form1顏色”後，Form1中顏色改變。

1. 在Form2裡面

   首先宣告一個委託和一個委託例項

   * Form2類外：

     ```c#
     public delegate void ChangeFormColor(bool topmost);
     ```

   * Form2類裡：

     ```c#
     public event ChangeFormColor ChangeColor;
     ```

   Form2的按鈕事件中呼叫委託：

   ```c#
   private void button1_Click(object sender, EventArgs e)
   {
       ChangeColor(true);//執行委託例項
   }
   ```

2. 在Form2裡面：

   button控制元件"開啟form2"的click事件中有下列程式碼：

   ```c#
   Form2 f = new Form2();
   f.ChangeColor += new ChangeFormColor(f_ChangeColor);
   f.Show();
   ```

   f.ChangeColor += new ChangeFormColor(f_ChangeColor);
   這句最關鍵，你輸入到+=之後，按兩下Tab，他會自動給你生成回撥函式，如下：

   ```c#
   void f_ChangeColor(bool topmost)
   {
       this.BackColor = Color.LightBlue;
       this.Text = "改變成功";
   }
   ```

3. 完整程式碼

   Form1：

   ```c#
   using System;
   using System.Drawing;
   using System.Windows.Forms;
    
   namespace 跨窗體呼叫控制元件
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
               this.Text = "改變成功";
           }
       }
   }
   ```

   Form2:

   ```c#
   using System;
   using System.Windows.Forms;
    
   namespace 跨窗體呼叫控制元件
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
               ChangeColor(true);//執行委託例項
           }
       }
   }
   ```

   

### .Net

------

#### 一些錯誤

* "The underlying connection was closed: An unexpected error occurred on a send."

 解決方法：

將`HttpWebRequest`的`keep-alive`屬性變為`true`，且修改安全協議型別：`ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12;`

### Visual Studio Code

------

### Visual Studio

------

#### 快捷鍵

* `ctrl`+`.` （開啟修補程式）
* `ctrl`+`r`（重新命名）
* `F12`（跳轉到定義處）

#### 一些錯誤

##### 部分檔案打不開窗體設計器 變成類.cs

解決方法：在csproj檔案下修改對應的`<Compile>`，將檔案節點改為`<SubForm>Form</SubForm>`

```
<Compile Include="Form1.cs">
	<SubType>Form</SubType>
</Compile>
```





### WSL

------

### Github

------

#### fork

在已經fork的專案中，若在自己本地已經有過更改，想要推送到被fork的專案中，則需先將本地的更改push，然後在github中開啟專案，會提示與被fork的專案有不同，此時便會提示建立一個pull request，按照操作，便可將提交push到被fork的專案中，在repo的擁有者同意merge前，本地的push會一直傳到這個request。若repo的擁有者已merge，則之後的更改，則需重複上述操作，重新建立一個pull request。

## LeetCode整理

------