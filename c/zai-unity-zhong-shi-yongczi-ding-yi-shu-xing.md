# 在Unity中使用C#自定义属性

C#关于自定义属性的论述参见[https://docs.microsoft.com/zh-cn/dotnet/standard/attributes/writing-custom-attributes](https://docs.microsoft.com/zh-cn/dotnet/standard/attributes/writing-custom-attributes)

在Unity当中使用自定义属性的难点我认为不在与定义，而在于在何时调用。



调用属性使用的是Attribute.GetCustomAttribute或Attribute.GetCustomAttributes方法，这两个方法的重载非常多。

## 如何使用字符串访问属性的方法？
