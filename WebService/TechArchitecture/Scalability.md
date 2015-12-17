# 网站的伸缩性架构 #

> 不断的向集群中添加服务器来增强整个集群的处理能力

## 1. 网站架构的伸缩性设计

### 1.1 不同功能进行物理分离实现伸缩

- 纵向分离
	
	> 分层后分离：将业务处理流程上的不同部分分离部署 

- 横向分离

	> 业务分割后分离：将不同的业务模块分离部署

### 1.2 单一功能通过集群规模实现伸缩
	
- 应用服务器集群

- 数据服务器集群
		
	- 缓存服务器集群
		
	- 存储服务器集群


## 2. 应用服务器集群的伸缩性设计

**负载均衡 :感知/配置集群服务器数量，处理服务器上线/下线，改变请求分发策略**

### 2.1 HTTP重定向负载均衡

<!-- ![HTTP重定向负载均衡原理](resource/Scalability-5_HTTP_loadbalance.png) -->
<img src="resource/Scalability-5_HTTP_loadbalance.png" alt="HTTP重定向负载均衡原理" style="width:450"/>

> 将Web服务器地址写入到HTTP重定向请求中(Code 302)返回给用户浏览器

- 优点：
	比较简单
- 缺点
	- 两次请求才能完成
	- 重定向服务器有可能成为瓶颈
	- HTTP302 可能被搜索引擎认为是SEO作弊

### 2.2 DNS域名解析负载均衡

- 优点
	- 负载工作交给DNS 节省网站成本
	- DNS基于地理位置，加快用户访问速度
- 缺点
	- 多级解析，状态同步慢，有可能跳转到失效服务器
	- 归域名服务商所属，管理不便

### 2.3 反向代理负载均衡

<img src="resource/Scalability-7_Reverse_Proxy_loadbalance.png" alt="反向代理负载均衡原理" style="width:450"/>

- 优点
	- 反向代理和负载结合，部署简单
- 缺点
	- 容易成为瓶颈

### 2.4 IP负载均衡
<img src="resource/Scalability-8_IP_loadbalance.png" alt="IP负载均衡原理" style="width:450"/>

### 2.5 数据链路层负载均衡
> 在通信协议的数据链路层修改Mac地址进行负载均衡

<img src="resource/Scalability-9_tcp_loadbalance.png" alt="数据链路层负载均衡原理" style="width:450"/>

>又称为三角传输模式 分发过程中修改MAC 

### 2.6 负载均衡算法
- 轮询(Round Robin, RR)
	> 所有请求被依次分发到每台服务器上
- 加权轮询（Weighted RR）
	> 根据服务器的性能，在轮询的基础上，按照配置的权重进行分发
- 随机(Random)
- 最少连接（Least Connection）	
	> 将最新请求分发到最少连接的服务器上
- 源地址散列 (Source Hashing)
	> 将请求来源的IP地址进行hash，同IP将总在一个服务器上处理，在一个会话周期内保存上下文，实现会话粘滞
 
## 3.分布式缓存集群的伸缩性设计

分布式缓存 受制于缓存数据的存储，必须要先找到拥有请求数据的缓存的服务器。服务器之间不对等 

必须使新上线的缓存服务器对整个缓存集群的影响最小， 这个是伸缩性设计的最主要目标 

### 3.1 Memcached分布式缓存集群的访问模型
<img src="resource/Scalability-10_Memcached_distributedcache.png" alt="Memcached分布式缓存" style="width:450"/>
> 客户端由 API，集群路由算法，集群服务器列表，通讯模块组成


### 3.2 分布式缓存的一致性Hash算法
> 余数Hash算法最为简单，但是在扩容的时候，命中率极低

一致性Hash算法，通过一致性Hash环的数据结构实现KEY到缓存服务器的Hash映射
<img src="resource/Scalability-11_consistenthashing.png" alt="Memcached分布式缓存" style="width:550"/>

一致性Hash环存在负载不均衡的问题，可使用虚拟层手段
> 将每天物理缓存服务器虚拟成一组虚拟缓存服务器，将虚拟服务器的hash放在环上

> 物理节点对应的虚拟节点越多，负载越均衡，新近服务器的影响也越小


```扩展阅读：http://blog.csdn.net/sparkliang/article/details/5279393 ```

## 4. 数据存储服务器集群的伸缩性设计

### 4.1 关系数据库集群的伸缩性设计

- 主从服务器： 
	> 主服务器写 从服务器读
- 业务分割
	> 不同业务表部署在不同服务器上， 不能跨服务器join
- 数据库分片
	> 开源 Amoeba & Cobar 
	>
	> 将数据库分解成不同的Schema，伸缩时从现有集群服务器中分别复制一定数量的Schema到新进服务器中，以达到负载均衡
	
### 4.2 NoSql数据库集群的伸缩性设计
> 摒弃了以关系代数为基础的结构化啊查询语言（sql）和失误的一致性保证（ACID）强化了高可用性和伸缩性

通过代理管理一定的KEY直区间，在达到一定阈值的时候，拆分代理从而达到伸缩的效果
