# 函数调用形参和实参之间加冒号

看到下面的语法，其他语言都没见过这种语法

int GetValue(string prompt, int min, int max) { int result; do { result = SnapsEngine.ReadInteger(prompt); } while (result < min || result > max); return result; } int age = GetValue(prompt:"Enter your age in years", min:0, max:100); int height = GetValue(prompt:"Enter your height in inches", min:30, max:96); 1 2 3 4 5 6 7 8 9 10 11 然后在stack overflow上看到使用冒号的各种情形： https://stackoverflow.com/questions/17034475/in-c-sharp-what-category-does-the-colon-fall-into-and-what-does-it-really

作用就是，指定形参名：

Console.WriteLine(value: "Foo"); 1 value一定要与声明中的形参名一致，不能改成其他名称，否则出错，所以， 猜测就是，冒号前的是形参名，冒号后的是实参，如果不用这种写法，参数顺序一定不能乱，如果使用这种语法，对参数位置没有要求，可以随意写。 ———————————————— 版权声明：本文为CSDN博主「小公鸡卡哇伊呀\~」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。 原文链接：https://blog.csdn.net/ftell/article/details/81706126
