# 网站的安全性架构 #

## 1. 网站应用攻击与防御 ##

### 1.1 XSS攻击
> 跨站脚本攻击(Cross Site Script) 篡改网页，恶意注入脚本

* 反射性攻击
	> 诱导用户点击还有攻击脚本的链接

<img src="resource/Security-1_RefectXSS.png" alt="反射型XSS攻击" style="width:650"/>

* 持久性攻击
	>提交恶意脚本请求，保存在数据库中
<img src="resource/Security-2_persistentXSS.png" alt="持久性XSS攻击" style="width:650"/>

**主要防御手段**

* 消毒
	> 对请求进行过滤和消毒，对某些html危险字符进行转义
* HttpOnly
	> 浏览器禁止页面JS访问带有HttpOnly属性的Cookie 防止Cookie信息被窃取

### 1.2 注入攻击

* SQL注入
	> **攻击者在http请求中注入恶意SQL命令，服务器构建SQL语句时恶意指令被一并构造**
	> 
	>- 开源： 数据库结构已知
	>
	>- 错误回显： 构造非法参数，通过网站错误回显猜测数据库表结构

	**主要防御手段**
	* 消毒
		> 过滤请求中可能得恶意SQL命令
	* 参数绑定
		> SQL预编译和参数绑定
	
* OS注入

### 1.3 CSRF攻击 
> CSRF(Cross Site Request Forgery，跨站请求伪造)，通过跨站请求以合法用户身份进行非法操作，**其核心是利用浏览器的Cookie或服务器的Session策略，盗取用户身份**
<img src="resource/Security-4_CSRF.png" alt="持久性XSS攻击" style="width:450"/>


**主要防御手段**
> 主要的防御方式是**识别请求者的身份** 

* 表单Token
	> CSRF需要伪造用户请求的所有参数，所以在表单中加入随机数Token
* 验证码
	> 避免攻击 体验不好 谨慎使用
* Referer Check
	> 检测HTTP请求头的Referer域

### 1.4 其他攻击和漏洞
* Error Code
	> 可将HTTP 500跳转到指定错误页面
* HTML 注释
* 文件上传
	>限制文件类型
* 路径遍历
	> 将JS CSS等部署在独立服务器，使用独立域名，动态参数不包含路径信息
 
### 1.5 Web应用防火墙
 开源ModeSecurity

### 网站安全漏洞扫描

## 2. 信息加密技术及密钥安全管理
> 信息加密技术分类：**单项散列加密，对称加密 非对称加密**

### 2.1 单项散列加密
>对不同长度的输入信息进行散列加密 得到相同长度的输出

>常用MD5 SHA等方法

### 2.2 对称加密

<img src="resource/Security-8_SymmetricCryptography.png" alt="对称加密" style="width:550"/>
> 优点：效率高，系统开销小，使用大量数据加密

> 缺点：同一密钥加密解密，远程传输密钥是关键

> 常用算法： DES 、 RC 

### 2.3 非对称加密
<img src="resource/Security-AsymmetricCryptography.png" alt="非对称加密" style="width:650"/>
> RAS 算法

### 2.4 密钥安全管理
	>密钥和算法放在独立服务器上，甚至独立硬件上---成本较高 消耗大 远程调用可能成为瓶颈
	
	>密钥分片存储在不同设备中，算法部署在应用服务器上
<img src="resource/Security-10_KeySecurityMgt.png" alt="密钥安全管理" style="width:550"/>

## 3.信息过滤与反垃圾

### 3.1文本匹配
> Trie树 、 Hash
### 3.2 分类算法
> Bayes / Native Bayes
### 3.3 <span id="23" href="123">黑名单

## 4.电子商务风险控制

### 4.1风控
机器自动风控的方式： 规则引擎和统计模型
<img src="resource/Security-14.png" alt="基于规则引擎的风险控制系统" style="width:550"/>
<img src="resource/Security-15.png" alt="基于统计模型的风险控制系统" style="width:550"/>


