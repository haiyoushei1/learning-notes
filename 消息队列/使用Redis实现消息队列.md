# 使用Redis实现消息队列

### 一、消息队列概述

##### 什么是消息队列

消息队列是分布式系统中的重要组件，使用于不需要立即获得结果，并且对并发量有要求的情况。

##### 消息队列的作用

1. 异步处理:在web环境下，很多时候响应都需要做成异步。

   比如我现在做的数据导出，导出2w(还没有做优化)需要差不多1min左右，Nginx会报504（？过后验证下）。

   这种情况就需要异步，前端发个请求，立刻响应，后端开个线程去处理前端的请求，同时前端每隔一段时间去访问，如果导出完毕，就进行下载。

2. 削峰填谷：当并发量很大的时候，进行控制

   上面的方案有个漏洞:当客户点了多个导出，或者，同时多个客户点导出，后端会为每个导出都分配一个线程，这么搞，离系统崩不远了，内存会迅速的耗光。这个时候就需要对请求进行一个控制，不能同时大量请求，对每一个请求我都开一个线程去处理。

   这里就需要消息队列:还是以导出部分为例。

   生产者：前端发送请求后，后端将筛选条件存入mysql，生成一个id，将id放进消息队列（rabbitmq，kafka，redis实现的消息队列等）。

   消费者：当监听器监听到消息队列有新消息时，调用相关的导出代码，开启一个线程进行导出，对消息队列进行一个消费。

3. 服务解耦：多个服务通过消息队列进行工作，而不依靠方法之间的传参调用，降低服务耦合。

   这个没什么好解释的，原本的功能之间都是通过调用对象方法，传参，进行一些操作。加上消息队列就不需要调用了，直接把消息发送到消息队列里，被调用的服务直接从消息队列获取信息。

### 二、为什么Redis可以做消息队列

只要是个队列，都可以做消息队列，Redis有list的数据结构，底层是双向队列，故可以做消息队列，就是要考虑的东西不少。

Redis大多时候是拿来做缓存，消息队列可以是可以，但有更好的工具如rabbitmq,kafka等。没必要硬用，本文只是记录，不过在性能上，看知乎说的（没压测过）几百万的并发量能撑住。

### 三、如何用Redis实现消息队列

还是拿导出做例子：导出就是个消费者生产者模型，但redis没提供这个功能，所以要自己写。

具体明天再说，，今天





