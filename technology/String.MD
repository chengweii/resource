# String
* ""与new String()的区别

# StringBuffer、StringBuilder
* StringBuffer 与 StringBuilder 中的方法和功能完全是等价的，因为他们有共同的基类AbstractStringBuilder。
* 只是StringBuffer 中的方法大都采用了 synchronized 关键字进行修饰，因此是线程安全的，而 StringBuilder 没有这个修饰，可以被认为是线程不安全的。 
* 在单线程程序下，StringBuilder效率更快，因为它不需要加锁，不具备多线程安全，而StringBuffer则每次都需要判断锁，效率相对更低。
```
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