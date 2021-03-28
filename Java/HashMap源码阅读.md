一、HashMap.put()
主要的代码就是putVal函数  
第一步：  
首先是初始化tab数组，这里会做扩容处理;  
判断目标位置是否是null，是就直接插进去
![](image-kibi4elb.png)

第二步：  
出现了哈希冲突，关注于桶  
三种情况：
1. 如果哈希值相同，并且key值也相同（ == || equals ）,将新的value更换原来的value
2. 因为在1.8，当链表的个数超过 ``` 8 ``` 会转为红黑树，
3. 若这个节点下个节点为null，将这个entry添加在当前节点的后面，并且判断节点数是否超过8.

![](image-kibiskrm.png)

补：为什么修改equals函数一定要改hash函数  
就应在这里了，这里规定是如果hashcode相同，那就去做equals判断了，如果没有修改，那得了，就会出现相同的key值，value不同的情况，那获取就会有二义性。


二、HashMap.resize()  // 重点

三、HashMap.treeBin