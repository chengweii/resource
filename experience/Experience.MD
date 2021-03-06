# 【Java】java mail Domain contains control or whitespace
## 异常描述
邮箱地址中包含特殊字符导致异常。
## 处理方案
1.将邮箱地址中非合法字符全部替换为空。 
"2555@qq.com".replaceAll("[^\\.\\@0-9a-zA-Z]*","")

# 【前端】Button未设置type属性自动提交表单异常
## 异常描述
Button未设置type属性时，在某些浏览器下会认为是submit，触发自动提交表单的动作。
## 处理方案
1.在使用Button时，务必要设置type属性（type=button）。 
2.在Button的click事件中添加 return false，防止提交动作。

# 【Java】java.lang.IllegalStateException：getOutputStream() has already been called
## 异常描述
出现此错误一般都是在请求处理中使用了输出流（如输出图片验证码，文件下载等），没有妥善处理好的原因。
## 处理方案
response在释放时，调用response.getWriter()会与response.getOutputStream()冲突，会出现以上异常。解决办法，在使用完输出流以后调用以下两行代码即可：
out.flush();
return null; 
ythhhhhhhhh
# 【Java】java.lang.OutOfMemoryError：PermGen space
## 异常描述
JVM内存溢出异常，产生该错误的原因大都出于以下原因：JVM内存过小、程序不严密，产生了过多的垃圾。
## 处理方案
检查是否存在以下情况
  1. 检查代码中是否有死循环或递归调用。
  2. 检查是否有大循环重复产生新对象实体。
  3. 检查对数据库查询中，是否有一次获得全部数据的查询。一般来说，如果一次取十万条记录到内存，就可能引起内存溢出。这个问题比较隐蔽，在上线前，数据库中数据较少，不容易出问题，上线后，数据库中数据多了，一次查询就有可能引起内存溢出。因此对于数据库查询尽量采用分页的方式查询。
  4. 检查List、Map等集合对象是否有使用完后，未清除的问题。List、Map等集合对象会始终存有对对象的引用，使得这些对象不能被GC回收。

# 【Java】java.lang.NullPointerException
## 异常描述
空指针异常抛出的异常信息一般是：Exception in thread ... java.lang.NullPointerException，所谓的空指针异常，就是一个指针是空指针，你还要去操作它，既然它指向的是空对象（null），那它就不能使用这个对象的实例成员（方法和属性）（注意：此时依然可以使用对象的静态成员的）。
## 处理方案
  ● 在使用对象的实例成员时，必须首先检查对象是否是空对象（null），不是才可以使用。
  ● 通过安全的方式使用对象的实例成员
      ○ 使用String.valueOf(对象) 替代 对象.toString()
      ○ 使用Arrays.toString(数组) 替代 数组.toString()

# 【Mysql】Table 'zntg_jxweb.act_activity' doesn't exist
## 异常描述
Mysql表名区分大小写，其他关键字、字段不区分。
## 处理方案
  ● SQL中表名要注意大小写

# 【Java】Tomcat启动报错，Caused by：java.lang.NoClassDefFoundError：LLogger
## 异常描述
邮箱Maven项目导入后，部署到tomcat运行报错：
Caused by: org.apache.catalina.LifecycleException: Failed to start component [StandardEngine[Catalina].StandardHost[localhost].StandardContext[/fwpt]]
at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:162)
... 6 more
Caused by: java.lang.NoClassDefFoundError: LLogger;
at java.lang.Class.getDeclaredFields0(Native Method)
at java.lang.Class.privateGetDeclaredFields(Class.java:2583)
at java.lang.Class.getDeclaredFields(Class.java:1916)
## 处理方案
1.检查tomcat的webapp部署目录下发现lib包未成功引入，原因为eclipse的配置中：Deployment Assembly中未将Maven 依赖包部署导致，将其配置进去，重新部署即可。 

![Deployment Assembly](https://raw.githubusercontent.com/chengweii/resource/master/experience/resource/experience-1.jpg)