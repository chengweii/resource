# String
## String类初始化后是不可变的
String的值是final修饰的，这代表String初始化后就是常量，对于String常量，它的值是在常量池中的。而JVM中的常量池在内存当中是以表的形式存在的， 对于String类型，有一张固定长度的CONSTANT_String_info表用来存储文字字符串值，注意：该表只存储文字字符串值，不存储符号引用。在程序执行的时候,常量池会储存在Method Area,而不是堆中。常量池中保存着很多String对象; 并且可以被共享使用，因此它提高了效率。
```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence 
{
    /** The value is used for character storage. */
    private final char value[];

    /** Cache the hash code for the string */
    private int hash; // Default to 0
}
```

## ""与new String()的区别

# StringBuffer、StringBuilder
## StringBuffer 与 StringBuilder 相同之处
StringBuffer 与 StringBuilder 中的方法和功能完全是等价的，因为他们有共同的基类AbstractStringBuilder。
## StringBuffer 与 StringBuilder 不同之处 
StringBuffer 中的方法大都采用了 synchronized 关键字进行修饰，因此是线程安全的，而 StringBuilder 则没有，所以是线程不安全的。所以在单线程程序下， StringBuilder 效率更快，因为它不需要加锁，不具备多线程安全，而 StringBuffer 则每次都需要判断锁，效率相对更低。
```java
 public final class StringBuffer
    extends AbstractStringBuilder
    implements java.io.Serializable, CharSequence
{
    @Override
    public synchronized StringBuffer append(String str) {
        toStringCache = null;
        super.append(str);
        return this;
    }
}

public final class StringBuilder
    extends AbstractStringBuilder
    implements java.io.Serializable, CharSequence
{
    @Override
    public StringBuilder append(CharSequence s) {
        super.append(s);
        return this;
    }
}

abstract class AbstractStringBuilder implements Appendable, CharSequence {
    @Override
    public AbstractStringBuilder append(CharSequence s) {
        if (s == null)
            return appendNull();
        if (s instanceof String)
            return this.append((String)s);
        if (s instanceof AbstractStringBuilder)
            return this.append((AbstractStringBuilder)s);

        return this.append(s, 0, s.length());
    }
}
```

## 功能实现原理
StringBuffer 与 StringBuilder 的功能继承于 AbstractStringBuilder ， AbstractStringBuilder 中采用一个char数组来保存需要 append 的字符串，char数组有一个初始大小，当 append 的字符串长度超过当前char数组容量时，则对char数组进行动态扩展，也即重新申请一段更大的内存空间，然后将当前char数组拷贝到新的位置，因为重新分配内存并拷贝的开销比较大，所以每次重新申请内存空间都是采用申请大于当前需要的内存空间的方式，这里是2倍。
```java
abstract class AbstractStringBuilder implements Appendable, CharSequence {
    public AbstractStringBuilder append(String str) {
        if (str == null)
            return appendNull();
        int len = str.length();
        ensureCapacityInternal(count + len);
        str.getChars(0, len, value, count);
        count += len;
        return this;
    }
    private void ensureCapacityInternal(int minimumCapacity) {
        // overflow-conscious code
        if (minimumCapacity - value.length > 0)
            expandCapacity(minimumCapacity);
    }
    void expandCapacity(int minimumCapacity) {
        int newCapacity = value.length * 2 + 2;
        if (newCapacity - minimumCapacity < 0)
            newCapacity = minimumCapacity;
        if (newCapacity < 0) {
            if (minimumCapacity < 0) // overflow
                throw new OutOfMemoryError();
            newCapacity = Integer.MAX_VALUE;
        }
        value = Arrays.copyOf(value, newCapacity);
    }
}
```

# String、StringBuffer、StringBuilder性能比较
```java
public class StringTest {

	private static final String base = " base string. ";
	private static final int count = 2000000;

	public static void stringTest() {
		long begin, end;
		begin = System.currentTimeMillis();
		String test = new String(base);
		for (int i = 0; i < count / 100; i++) {
			test = test + " add ";
		}
		end = System.currentTimeMillis();
		System.out.println((end - begin) + " millis has elapsed when used String. ");
	}

	public static void stringBufferTest() {
		long begin, end;
		begin = System.currentTimeMillis();
		StringBuffer test = new StringBuffer(base);
		for (int i = 0; i < count; i++) {
			test = test.append(" add ");
		}
		end = System.currentTimeMillis();
		System.out.println((end - begin) + " millis has elapsed when used StringBuffer. ");
	}

	public static void stringBuilderTest() {
		long begin, end;
		begin = System.currentTimeMillis();
		StringBuilder test = new StringBuilder(base);
		for (int i = 0; i < count; i++) {
			test = test.append(" add ");
		}
		end = System.currentTimeMillis();
		System.out.println((end - begin) + " millis has elapsed when used StringBuilder. ");
	}

	public static void main(String[] args) {
		stringTest();
		stringBufferTest();
		stringBuilderTest();
	}

}
```
>比较结果：  
866 millis has elapsed when used String.   
64 millis has elapsed when used StringBuffer.   
30 millis has elapsed when used StringBuilder.  
采用String对象时，即使运行次数仅是采用其他对象的1/100，其执行时间仍然比其他对象高出 <font color=#66CDAA size=5>25</font> 倍以上；而采用StringBuffer对象和采用StringBuilder对象的差别也比较明显，前者是后者的 <font color=#66CDAA size=5>1.5</font> 倍左右。  

* StringBuffer 始于 JDK 1.0  
* StringBuilder 始于 JDK 1.5  
* 从 JDK 1.5 开始，带有字符串变量的连接操作（+），JVM 内部采用的是 StringBuilder 来实现的，而之前这个操作是采用 StringBuffer 实现的。  

因为每次执行“+”操作时jvm都要new一个StringBuffer对象来处理字符串的连接，这在涉及很多的字符串连接操作时开销会很大,所以还是建议使用StringBuilder。

# 参考文献  
[深入理解Java：String](http://www.cnblogs.com/ITtangtang/p/3976820.html)