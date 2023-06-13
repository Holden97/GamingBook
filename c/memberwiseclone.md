# MemberwiseClone

小技巧：利用 MemberwiseClone() 简化 C# 中 Clone() 深克隆代码

郝伟博士

于 2019-08-19 15:43:49 发布

1514 收藏 5 分类专栏： C#语言详解 设计模式 文章标签： Clone() 深克隆 简化 MemberwiseClone() 版权

C#语言详解 同时被 2 个专栏收录 91 篇文章18 订阅 订阅专栏

设计模式 17 篇文章1 订阅 订阅专栏 前言 在单例模式的应用场景中，Clone() 方法会大量使用，我们在写代码的时候经常会写很多的代码，用于字段或属性的复制，这样的操作不仅麻烦，而且在变更时也不易于修改。实际上我们有更简单的方法，现在就以一个实例来解释这个技巧。

示例 class Student { public int ID {get; set;} = 0; public string Name {get; set;} = "Tom Cruise"; public List NickNames {get; set;} = new List(){ "Finger", "TC", "K"};

```
public Student Clone()
{
    Student stu = new Student();
    stu.ID = this.ID;
    stu.Name = this.Name;
    stu.NickNames = new List<string>();
    stu.NickNames.AddAll(this.NickNames);
    return stu;
}
```

} 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 问题1 这样虽然没有问题，但是太麻烦。如果属性多的话，会有一个很长的列表，而且如果属性添加、修改或删除都需要修改此处的代码，不符合设计模式中的开放闭合原则。

解决方案1 好在C#中有个 MemberwiseClone() 方法，能够对得有的属性进行浅拷贝（即只是引用，不是新建数据）。修改后的Clone方法如下所示。

public Student Clone() { return (Student) MemberwiseClone(); } 1 2 3 4 如此一来，所有的属性都会得到复制，任务完成。

问题2 由于 MemberwiseClone() 方法对属性进行的是浅拷贝，所以一些引用类型原对象和克隆对象引用的是同一个实例。比如 Name 和 NickNames 属性。由于 Name 属性是字符串，而字符串是只读的，所以虽然引用了相同的对象，但是在实际使用中也不会有什么问题。然而对于第二个属性，则会有潜在的问题。因为引用的是同一个列表，当原对象添加或删除其中的一个名子后，另一个对象也会发生改变。

解决方法2 为了解决这个问题，我们再对Clone方法进行修改，如下所示。

public Student Clone() { Student stu = (Student) MemberwiseClone(); stu.NickNames = new List(); stu.NickNames.AddAll(this.NickNames); return stu; } 1 2 3 4 5 6 7 如上所示，我们先用浅拷贝生成一个新克隆对象，然后再根据实际情况对需要深拷贝的属性进行复制。这样以来，我们一方面利用 MemberwiseClone() 对基本的可直接复制的对象进行复制，另一方面再根据实际需求，对需要深拷贝的对象进行复制。这样即使代码量大大降低，又保证了深拷贝的需求。

总结 通过对 MemberwiseClone() 函数的利用，我们可以在满足需求的前提下，大大减少复制操作的代码量，从而尽可能地满足开放封闭原则。实际上，这个方法仍然没有完全满足这个原则。但是根据实际的项目情况，我们只要少量修改代码即可。而且 ，一般情况下类的属性也应该是封闭的，所以这各方法能够圆满地解决复制代码过多和 MemberwiseClone() 只能浅拷贝的问题。



## 参考资料

1. https://blog.csdn.net/weixin\_43145361/article/details/99729743
