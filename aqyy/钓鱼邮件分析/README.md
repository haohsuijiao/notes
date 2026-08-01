# 一、分析步骤

| 检查项                       | 说明                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| 发件人地址（From）           | 是否与声称的机构/公司域名一致？注意字母替换                  |
| 邮件头（Header）中的真实路径 | 查看 Received、Return-Path、Reply-To，看是否来自可信服务器。SPF/DKIM/DMARC 验证结果也可参考。 |
| 链接（URL）                  | 鼠标悬停或复制链接查看真实域名，是否与官方域名完全一致？钓鱼常用相似域名或随机域名。 |
| 邮件内容和语气               | 是否制造紧迫感（“账户将被禁用”“立即验证”）？是否要求点击链接输入密码/验证码？ |
| 附件                         | 是否带有陌生附件（尤其是 .exe、.js、.docm 等）？             |
| 收件人/抄送人                | 是否大量陌生收件人或非正常群发？                             |
| 发件时间                     | 是否异常（如凌晨批量发送）？                                 |

# 二、具体分析案例

```bahs
Received: from unknown (unknown [127.0.0.1])
	by m01.gov110.cn (Postfix) with SMTP id B3B92944A64
	for <M01archiving@m01.gov110.com>; Thu, 30 Jul 2026 18:31:48 +0800 (CST)
X-RM-TagInfo: emlType=0                                       
X-RM-SPAM:                                                                                        
X-RM-SPAM-FLAG:00000000
Received:from gr-PC (unknown[10.9.209.121])
	by rmsmtp-host003-12003 (RichMail) with SMTP id 2ee36a6b280e59c-cb690;
	Thu, 30 Jul 2026 18:31:43 +0800 (CST)
X-RM-TRANSID:2ee36a6b280e59c-cb690
Date: Thu, 30 Jul 2026 18:31:42 +0800
From: "guorui@airchina.com" <guorui@airchina.com>
To:m01@airchina.com
Subject: 1
X-Priority: 3
X-GUID: 3C5FBDF4-798F-4E70-9236-45295778281F
X-Has-Attach: no
X-Mailer: Foxmail 7.2.25.375[cn]
Mime-Version: 1.0
Message-ID: <202607301831424963363@airchina.com>
Content-Type: multipart/alternative;
	boundary="----=_001_NextPart322140777308_=----"

This is a multi-part message in MIME format.

------=_001_NextPart322140777308_=----
Content-Type: text/plain;
	charset="UTF-8"
Content-Transfer-Encoding: base64

...
...
guorui@airchina.com
 
发件人： duxinyi2015
发送时间： 2026-07-30 17:14
收件人： guorui
主题： 转发：欢迎使用 duxinyi2015

------=_001_NextPart322140777308_=----
Content-Type: text/html;
	charset="UTF-8"
Content-Transfer-Encoding: quoted-printable

...
...
------=_001_NextPart322140777308_=------
```

## 1，发件地址与收件地址可疑

```bahs
显示发件人：guorui@airchina.com
实际收件人：m01@airchina.com
邮件主题：进一个数字1，非常可疑，正常内部邮件不会这么发；
	邮件正文中又出现了另一个发件人 duxinyi2015@airchina.com，并声称“账户出现异常”“请验证账户为本人在使用，未验证将被禁用”，这是典型的制造恐慌+诱导点击的话术。
```

## 2，链接域名与国航无关

邮件中唯一的按钮链接是：

```bahs
https://plxarag.cn?top=duxinyi2015@airchina.com

域名是 plxarag.cn，不是 airchina.com 或国航官方子域名。
该域名注册信息隐蔽，极可能是临时注册的钓鱼域名。
参数中携带了你的邮箱，说明该链接是定向钓鱼，点击后可能会跳转到一个仿冒的国航登录页面，骗取你的账号密码。
```

## 3，邮件头揭示更多风险

```bahs
该邮件经过了 m01.gov110.cn 和 rmsmtp-host003-12003 中继，并非直接从国航官方邮件服务器发出。

使用 Foxmail 7.2.25.375 发送，而发件人声称是国航内部系统自动发送，两者矛盾。

邮件时间戳 2026-07-30 18:31:42，但转发的原始邮件声称是 2026-07-30 00:24，时间逻辑混乱。
```

## 4，内容缺少个性化信息

```bahs
正规企业邮件通常包含你的姓名、工号或业务相关信息，而此邮件只有通用模板，且HTML代码中样式、颜色、按钮均是通用钓鱼模板特征。
```

