# 一、关于GameResource
本项目源自于[码问社区](https://www.mawen.co/)
涉及技术栈：springboot，elaticsearch，redis，nginx，git，maven
个人基于码问社区进行二次开发，使用elaticsearch实现搜索功能，redis实现最热帖子，点赞功能（还未添加），使用nginx进行反向代理。
项目时长（本人开发总计20天）
项目是用全栈，没有用前后端分离（关于前后端分离请看这篇博客[前后端分离](http://172.81.243.159:8090/archives/qian-hou-duan-fen-li)）

# 二、本系列打算由以下几点组成
1. 第三方登录
2. 发布问题
3. 展示问题
4. 回复问题、回复通知
5. elaticsearch实现搜索问题
6. redis实现点赞，最热帖子功能

# 三、关于Web项目
经过我这段时间刷面经以及和人聊天，觉得一个普通的Web项目很简单，就比如这个码问，就是普通CRUD。
开发流程就是：
根据需求数据库建表->Controller层接收请求，json数据，调用Service层进行数据处理，对数据库CRUD，返回json数据->前端再接收json，展示。
写一两个功能挺新鲜，写多了觉得没什么意思。
更多的提升还是在中间件，如何处理高并发，这个是难点。
学了半年的Java（除多线程，JVM）结果发现还没到最重要的东西（苦笑）