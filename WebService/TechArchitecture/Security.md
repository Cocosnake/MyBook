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
*