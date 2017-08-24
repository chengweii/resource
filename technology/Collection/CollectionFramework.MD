# 集合框架
![集合框架](https://raw.githubusercontent.com/chengweii/resource/master/technology/Collection/resource/collection-framework-1.jpg)
## 链表
### ArrayList
ArrayList实现了List接口，是顺序容器，即元素存放的数据与放进去的顺序相同，允许放入null元素，底层通过对象数组实现。  
该类还实现了Serializable、Cloneable接口，支持序列化和克隆。  
除该类未实现同步外，其余跟Vector大致相同。  
#### 实现原理
每个ArrayList都有一个容量（capacity），表示底层数组的实际大小，容器内存储元素的个数不能多于当前容量。当向容器中添加元素时，如果容量不足，容器会自动增大底层数组的大小。  
size(), isEmpty(), get(), set()方法均能在常数时间内完成，add()方法的时间开销跟插入位置有关，addAll()方法的时间开销跟添加元素的个数成正比。其余方法大都是线性时间。  
为追求效率，ArrayList没有实现同步（synchronized），如果需要多个线程并发访问，用户可以手动同步，也可使用Vector替代。 
> 扩容机制: ArrayList自动扩容的方式是通过创建容量为旧数组容量1.5倍的新数组，然后通过Arrays.copyOf()方法将旧数组的值拷贝过去(注：此操作为浅拷贝)。  
说到Arrays.copyOf()方法，此方法内部通过System.arraycopy()方法实现，而System.arraycopy()方法是使用的内存复制，在数组较大时，效率要比手工循环寻址复制要好。

> Fail-Fast机制:由于ArrayList不是线程安全的，因此如果在使用迭代器的过程中有其他线程修改了ArrayList，那么将抛出ConcurrentModificationException，这就是所谓fail-fast策略。  
这一策略在源码中的实现是通过modCount域，modCount顾名思义就是修改次数，对ArrayList内容的修改都将增加这个值，那么在迭代器初始化过程中会将这个值赋给迭代器的expectedModCount。在迭代过程中，判断modCount跟expectedModCount是否相等，如果不相等就表示已经有其他线程修改了ArrayList。(注意：在java8之前modCount声明为volatile，保证线程之间修改的可见性，但java8开始,modCount去除了volatile修饰符，毕竟这个类是声明为非线程安全的。volatile变量的读写需要一些内存关卡，开销大点。)
#### 使用注意
* 由于数组容量不足而自动扩容的机制会造成性能损耗，在实际添加大量元素前，可以提前预估并指定ArrayList实例的容量，或者通过调用ensureCapacity()方法来手动增加ArrayList实例的容量，以减少递增式再扩容导致的性能损耗。
#### 特点总结
* 有序
* 可序列化
* 可克隆
* 允许放入null元素
* 非线程安全
* 
### Vector
Vector内部与ArrayList大致相同，但该类实现了同步，是ArrayList的线程安全版本实现。
## 队列

## 哈希表

## 功能对比
|实现类|保持插入顺序|可重复|可排序|性能对比|
|-|-|-|-|-|
|ArrayList|是|是|否|随机访问元素较快，但插入、删除元素较慢（数组特性）|
## 线程安全
# 参考文献
[ArrayList详解](http://www.cnblogs.com/qiutianyou/p/5440065.html)  
[Java集合---ArrayList的实现原理](http://www.cnblogs.com/ITtangtang/p/3948555.html)  
[Arrays.copy和System.arraycopy](http://blog.csdn.net/yangqillohe/article/details/5625159)  
[java.util.HashMap 源码解读及其进化](https://coderbee.net/index.php/java/20131018/519)  