# 工厂方法模式/工厂模式

简单工厂模式很常见，简单工厂模式有一个不足，他虽然遵循了单一职责原则，但它违反了另一条很重要的原则：**开放封闭原则**。如果新增一个文员职位，那么我们还要修改对应代码，增加一个case，这是很可怕的，因为写好的代码如果我们再去修改可能会造成未知的效果。

工厂方法模式是简单工厂的进一步深化， 在工厂方法模式中，我们不再提供一个统一的工厂类来创建所有的对象，而是针对不同的对象提供不同的工厂。也就是说每个对象都有一个与之对应的工厂。

**定义：**\
定义一个用于创建对象的接口，让子类决定将哪一个类实例化。工厂方法模式让一个类的实例化延迟到其子类。\
这次我们先用实例详细解释一下这个定义，最后在总结它的使用场景。

**实例：**\
现在需要设计一个这样的图片加载类，它具有多个图片加载器，用来加载jpg，png，gif格式的图片，每个加载器都有一个read（）方法，用于读取图片。下面我们完成这个图片加载类。

首先完成图片加载器的设计，编写一个加载器的公共接口。

```csharp
public interface Reader {
    void read();
}
```

Reader 里面只有一个read（）方法，然后完成各个图片加载器的代码。

Jpg图片加载器

```java
public class JpgReader implements Reader {
    @Override
    public void read() {
        System.out.print("read jpg");
    }
}
```

Png图片加载器

```java
public class PngReader implements Reader {
    @Override
    public void read() {
        System.out.print("read png");
    }
}
```

Gif图片加载器

```java
public class GifReader implements Reader {
    @Override
    public void read() {
        System.out.print("read gif");
    }
}
```

现在我们按照定义所说定义一个抽象的工厂接口ReaderFactory

```csharp
public interface ReaderFactory {
    Reader getReader();
}
```

里面有一个getReader（）方法返回我们的Reader 类，接下来我们把上面定义好的每个图片加载器都提供一个工厂类，这些工厂类实现了ReaderFactory 。

Jpg加载器工厂

```java
public class JpgReaderFactory implements ReaderFactory {
    @Override
    public Reader getReader() {
        return new JpgReader();
    }
}
```

Png加载器工厂

```java
public class PngReaderFactory implements ReaderFactory {
    @Override
    public Reader getReader() {
        return new PngReader();
    }
}
```

Gif加载器工厂

```java
public class GifReaderFactory implements ReaderFactory {
    @Override
    public Reader getReader() {
        return new GifReader();
    }
}
```

在每个工厂类中我们都通过复写的getReader（）方法返回各自的图片加载器对象。

客户端使用

读取Jpg

```dart
ReaderFactory factory=new JpgReaderFactory();
Reader  reader=factory.getReader();
reader.read();
```

读取Png

```dart
ReaderFactory factory=new PngReaderFactory();
Reader  reader=factory.getReader();
reader.read();
```

读取Gif

```dart
ReaderFactory factory=new GifReaderFactory();
Reader  reader=factory.getReader();
reader.read();
```

可以看到上面三段代码，分别读取了不同格式的图片，不同之处在于针对不同的图片格式声明了不同的工厂，进而创建了相应的图片加载器。

和简单工厂对比一下，最根本的区别在于简单工厂只有一个统一的工厂类，而工厂方法是针对每个要创建的对象都会提供一个工厂类，这些工厂类都实现了一个工厂基类（本例中的ReaderFactory ）。下面总结一下工厂方法的适用场景。

**适用场景：**

（1）客户端不需要知道它所创建的对象的类。例子中我们不知道每个图片加载器具体叫什么名，只知道创建它的工厂名就完成了创建过程。\
（2）客户端可以通过子类来指定创建对应的对象。\
以上场景使用于采用工厂方法模式。

## 参考资料

1. 工厂模式应用场景 [https://segmentfault.com/q/1010000005849224](https://segmentfault.com/q/1010000005849224)
2. 工厂模式——看这一篇就够了 [https://www.jianshu.com/p/83ef48ce635b](https://www.jianshu.com/p/83ef48ce635b)
