# readonly

readonly 关键字是一个可在四个上下文中使用的修饰符： 在字段声明中， readonly 指示只能在声明期间或在同一个类的构造函数中向字段赋值。 可以在字段声明和构造函数中多次分配和重新分配只读字段。 构造函数退出后，不能分配 readonly 字段

readonly是一个修饰字段的关键字。被它修饰的字段只有在初始化或者构造函数中才能够赋值。

```csharp
   class Program
    {
        public static readonly List<string> DefaultPracticeAreas = new List<string>() { "Criminal Law",
            "Estate Planning", "Family Law", "Litigation", "Personal Injury", "Real Estate", "Transactional",
            "Unassigned" };
        public static readonly string aString = "abbb";
        public static readonly List<Person> Personlist = new List<Person>() { new Person(1, "aaa"), new Person(2, "bbb") };

        static void Main(string[] args)
        {
            DefaultPracticeAreas.ElementAt(0) = "111";
            aString = "ccc";

            Personlist.ElementAt(0).age = 0;
            Console.WriteLine(Personlist.ElementAt(0).age);

            Console.ReadLine();
        }
    }
    public class Person
    {
        public int age { get; set; }
        public string name { get; set; }
        public Person(int a, string n) { age = a; name = n; }
    }
```

对于下面的string型的list 或者纯string 当我们尝试去修改它的值，会发现错误：

```csharp
DefaultPracticeAreas.ElementAt(0) = "111";
aString = "ccc";
```

A static readonly field cannot be assigned to (except for a static constructor or a variable initializer)

但是对于引用型变量的list来说 我们还是可以修改其中某个变量的值：

```csharp
Personlist.ElementAt(0).age = 0;
Console.WriteLine(Personlist.ElementAt(0).age);
```

Personlist的第一个person元素的age被成功的修改成了0，这说明readonly对于引用类型变量，它限制的不可修改性是针对于引用类型的指针，至于地址里面存放的值，是可以修改的。

参考

1. [https://zhuanlan.zhihu.com/p/28713508](https://zhuanlan.zhihu.com/p/28713508)
