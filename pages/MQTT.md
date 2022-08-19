- [[MQTT]]介绍及工作原理
- MQTT协议运行在TCP/IP或其他网络协议，提供有序、无损、双向连接。其特点包括：
	- 使用的发布/订阅消息模式，它提供了一对多消息分发，以实现与应用程序的解耦。
	- 对负载内容屏蔽的消息传输机制。
	- 对传输消息有三种服务质量（QoS）：
	- 最多一次，这一级别会发生消息丢失或重复，消息发布依赖于底层TCP/IP网络。即：<=1
	- 至多一次，这一级别会确保消息到达，但消息可能会重复。即：>=1
	- 只有一次，确保消息只有一次到达。即：＝1。在一些要求比较严格的计费系统中，可以使用此级别
	- 数据传输和协议交换的最小化（协议头部只有2字节），以减少网络流量
	  通知机制，异常中断时通知传输双方
	  使用 Last Will 和 Testament 特性通知有关各方客户端异常中断的机制；