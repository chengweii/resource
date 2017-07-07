# String
## String类初始化后是不可变的
String的值是final修饰的，这代表String的值初始化后就是常量，不可改变，只能新建。
```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence 
{
    private final char value[];
    private int hash;

    public String() {
        this.value = new char[0];
    }

    public String(String original) {
        this.value = original.value;
        this.hash = original.hash;
    }

    public String(char value[]) {
        this.value = Arrays.copyOf(value, value.length);
    }
}
```

## String常量是在常量池中的，并且常量池中的String可以被共享使用。
常量池中的String被共享使用，达到节省内存，使数据集更小，（通常会）让程序运行更快的效果，注意：默认只有String常量(字面量常量、编译期认定的常量)会被放进常量池。
```java
public class StringTest {
	public static void stringStorageTest() {
		/**
		 * 情景一：字符串池 JAVA虚拟机(JVM)中存在着一个字符串池，其中保存着很多字符串; 并且可以被共享使用，因此它提高了效率。
		 * 由于String类是final的，它的值一经创建就不可改变。
		 * 字符串池由String类维护，我们可以调用intern()方法来访问字符串池。
		 */
		String s1 = "abc";
		// ↑ 在字符串池创建了一个字符串
		String s2 = "abc";
		// ↑ 字符串池已经存在字符串 "abc" (共享),所以创建0个对象，累计创建一个对象
		System.out.println("s1 == s2 : " + (s1 == s2));
		// ↑ true 指向同一个对象
		System.out.println("s1.equals(s2) : " + (s1.equals(s2)));
		// ↑ true 值相等
		// ↑------------------------------------------------------over

		/**
		 * 情景二： 由于常量的值在编译的时候就被确定(优化)了。 在这里，"ab"和"cd"都是常量，因此变量str3的值在编译时就可以确定。
		 * 这行代码编译后的效果等同于： String str3 = "abcd";
		 */
		String str1 = "ab" + "cd"; // 1个对象
		String str11 = "abcd";
		System.out.println("str1 = str11 : " + (str1 == str11));
		// ↑true 编译期优化
		// ↑------------------------------------------------------over

		/**
		 * 情景三： 局部变量str2,str3存储的是存储两个拘留字符串对象(intern字符串对象)的地址。
		 * 
		 * 第三行代码原理(str2+str3)： 运行期JVM首先会在堆中创建一个StringBuilder类，
		 * 同时用str2指向的拘留字符串对象完成初始化， 然后调用append方法完成对str3所指向的拘留字符串的合并，
		 * 接着调用StringBuilder的toString()方法在堆中创建一个String对象，
		 * 最后将刚生成的String对象的堆地址存放在局部变量str3中。
		 * 
		 * 而str5存储的是字符串池中"abcd"所对应的拘留字符串对象的地址。 str4与str5地址当然不一样了。
		 * 
		 * 内存中实际上有五个字符串对象： 三个拘留字符串对象、一个String对象和一个StringBuilder对象。
		 */
		String str2 = "ab"; // 1个对象
		String str3 = "cd"; // 1个对象
		String str4 = str2 + str3;
		String str5 = "abcd";
		System.out.println("str4 = str5 : " + (str4 == str5));
		// false
		// ↑------------------------------------------------------over

		/**
		 * 情景四： JAVA编译器对string + 基本类型/常量 是当成常量表达式直接求值来优化的。
		 * 运行期的两个string相加，会产生新的对象的，存储在堆(heap)中
		 */
		String str6 = "b";
		String str7 = "a" + str6;
		String str67 = "ab";
		System.out.println("str7 = str67 : " + (str7 == str67));
		// ↑false
		// ↑str6为变量，在运行期才会被解析。
		final String str8 = "b";
		String str9 = "a" + str8;
		String str89 = "ab";
		System.out.println("str9 = str89 : " + (str9 == str89));
		// ↑true
		// ↑str8为常量变量，编译期会被优化
		// ↑------------------------------------------------------over
	}

	public static void main(String[] args) {
		stringStorageTest();
	}

}
```

## ""与new String()的区别

## 通过intern使用常量池
由于只有String常量默认会被放进常量池，所以针对重复使用率较高的String变量，可以通过intern方法来使用常量池来提升程序性能。


# StringBuffer、StringBuilder
## StringBuffer 与 StringBuilder 相同之处
StringBuffer 与 StringBuilder 中的方法和功能完全是等价的，因为他们有共同的基类AbstractStringBuilder。
## StringBuffer 与 StringBuilder 不同之处 
StringBuffer 中的方法大都采用了 synchronized 关键字进行修饰，因此是线程安全的，而 StringBuilder 则没有，所以是线程不安全的。在单线程程序下， StringBuilder 效率更快，因为它不需要加锁，不具备多线程安全，而 StringBuffer 则每次都需要判断锁，效率相对更低。
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
    char[] value;

    AbstractStringBuilder(int capacity) {
        value = new char[capacity];
    }

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