# OMS03

> 点餐管理系统 03

## 题目背景

由于饭店经营得当，饭店进行了一次很大规模的扩建，招聘了更多的厨师，也能力做出更多种类的菜品，饭店老板雄心壮志，甚至打算把菜单出成一本比新华字典还要厚的书。那么此时，点餐管理系统就需要进行功能的更新了，具体的更新如下：

## 任务

### Test

在oms02中，大家编写了Menu类，由于这次饭店的扩建，一个菜单中的菜品种类的远远多于oms02，无论是打印菜单还是关键字查找都有可能得到好多结果，打印出来长长的一串，十分不利于顾客点餐。

为了解决这个问题，系统对打印的结果进行截断显示的处理，把输出的结果按照“页”单位存放，同时加入若干的指令进行当前页的控制（前进/后退/退出/返回首页）。

### 功能新增

在上次点餐系统的基础上（务必完成oms02），我们为指令终端加入了一些新的指令，方便按页读取菜单信息，基本格式如下：

```shell
选项 [参数1] [参数2] [参数3] [参数4]
```

| 选项 | [参数 1] | [参数 2] | [参数 3] | [参数 4] | 功能描述 |
| :---: | :---: | :---: | :---: | :---: | :---: |
| gd | -key | 关键字 | 第n页 | 每页m个记录 | 详见功能1描述（有内嵌指令，请仔细阅读） |
| pm | 第n页 | 每页m个记录 |  |  | 详见功能2描述（有内嵌指令，请仔细阅读） |

#### 功能1
新增的功能1沿用了oms02的`gd`指令，但是由于参数数量的区别，功能上与其并不冲突。新的功能1要实现**在关键字查找的基础上**进行分页的操作：
- 参数2：测试数据保证关键字不含有空白符号，查找时模糊大小写（大小写无关）
- 参数3：n（int）表示输出的菜品列表属于当前的第n页的内容
- 参数4：m（int）表示当前的分页操作是按照每页m个菜品划分的
- 不保证菜单中的菜品总种类数C可以被m整除，因此最后一页的菜品数lm ≤ m
- 页码的计算从1开始

内嵌指令操作1：当调用了按页关键字查找菜品功能时，接下来的输入均按照如下的指令处理：

| 内嵌指令 | 功能描述 |
| :---: |  :---: |
| n | 输出当前页的下一页的内容，如果当前页已经是最后一页，请输出`this is the end page` |
| l | 输出当前页的上一页的内容，如果当前页已经是第一页，请输出`this is the begin page` |
| f | 转到第一页（首页）并打印其内容|
| q | 退出内嵌指令环境，返回到上一层指令环境，同时打印退出提示`exit page-check mode`，该指令是**唯一**退出内嵌环境的指令，不可调用其他指令退出当前环境 |
| | 如果当前菜单的内容为空，则直接输出`menu is empty, exit page-check mode`，并退出内嵌环境 |
| | 如果调用了内嵌环境不存在的指令（包括参数不对应），输出`call inner method illegal` |

##### 打印格式

```
[-] gd -key C 2 3
[+] Page: 2
    1. DID:H000111,DISH:XiHongShiChaoJiDan,PRICE:5.0,TOTAL:30
    2. DID:C000000,DISH:IceCream,PRICE:4.0,TOTAL:100
    3. DID:O000000,DISH:Cola,PRICE:5.0,TOTAL:200
    n-next page,l-last page,f-first page,q-quit
[-] n
[+] this is the end page
[-] l
[+] Page: 1
    1. DID:H000000,DISH:C1,PRICE:1.0,TOTAL:130
    2. DID:H000001,DISH:C2,PRICE:2.0,TOTAL:100
    3. DID:H000002,DISH:C3,PRICE:3.0,TOTAL:100
    n-next page,l-last page,f-first page,q-quit
[-] l
[+] this is the begin page
[-] f
[+] Page: 1
    1. DID:H000000,DISH:C1,PRICE:1.0,TOTAL:130
    2. DID:H000001,DISH:C2,PRICE:2.0,TOTAL:100
    3. DID:H000002,DISH:C3,PRICE:3.0,TOTAL:100
    n-next page,l-last page,f-first page,q-quit
[-] gd -id H000000
[+] call inner method illegal
[-] q
[+] exit page-check mode
[-] QUIT
[+] ----- Good Bye! -----
```

#### 功能2

新增的功能2沿用了oms02的`pm`指令，但是由于参数数量的区别，功能上与其并不冲突。新的功能2要实现**打印菜单的基础上**进行分页的操作：
- 参数1：n（int）表示输出的菜品列表属于当前的第n页的内容
- 参数2：m（int）表示当前的分页操作是按照每页m个菜品划分的
- 不保证菜单中的菜品总种类数C可以被m整除，因此最后一页的菜品数lm ≤ m
- 页码的计算从1开始

内嵌指令操作2：当调用了按页打印菜品功能时，接下来的输入均按照如下的指令处理：

| 内嵌指令 | 功能描述 |
| :---: |  :---: |
| n | 输出当前页的下一页的内容，如果当前页已经是最后一页，请输出`this is the end page` |
| l | 输出当前页的上一页的内容，如果当前页已经是第一页，请输出`this is the begin page` |
| f | 转到第一页（首页）并打印其内容|
| q | 退出内嵌指令环境，返回到上一层指令环境，同时打印退出提示`exit page-check mode`，该指令是**唯一**退出内嵌环境的指令，不可调用其他指令退出当前环境 |
| | 如果当前菜单的内容为空，则直接输出`menu is empty, exit page-check mode`，并退出内嵌环境 |
| | 如果调用了内嵌环境不存在的指令（包括参数不对应），输出`call inner method illegal` |

##### 打印格式

```
[-] pm
[+] 1. DID:H000000,DISH:H1,PRICE:1.0,TOTAL:10
    2. DID:H000001,DISH:H2,PRICE:1.0,TOTAL:10
    3. DID:H000002,DISH:H3,PRICE:1.0,TOTAL:10
    4. DID:H000003,DISH:D1,PRICE:1.0,TOTAL:10
    5. DID:C000002,DISH:C1,PRICE:1.0,TOTAL:10
[-] pm 2 3
[+] Page: 2
    1. DID:H000003,DISH:D1,PRICE:1.0,TOTAL:10
    2. DID:C000002,DISH:C1,PRICE:1.0,TOTAL:10
    n-next page,l-last page,f-first page,q-quit
[-] q
[+] exit page-check mode
[-] QUIT
[+] ----- Good Bye! -----  
```

### 提示

虽然测试的数据量不会太⼤，但是对于菜单（菜品列表）等建议使⽤动态存储⽅式，以防爆栈

## 关于提交的注意事项

oms作业的提交有**严格**的格式要求，测试只关注`Test.java`文件，因此请确保你所提交的oms作业格式如下所示：

提交格式：zip压缩包

命名格式（严格）：作业次数-学号-姓名.zip

> 2-19373000-张三.zip 表示这是张三的oms02作业

压缩包结构：

```
作业次数-学号-姓名.zip
|
| src
    |
    | Test.java
    | ****.java
    | ...
```