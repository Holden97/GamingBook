# KeyValuePair

KeyValuePair 和 Dictionary 的关系&#x20;

1、KeyValuePair&#x20;

a、KeyValuePair 是一个结构体（struct）；

b、KeyValuePair 只包含一个Key、Value的键值对。&#x20;

2、Dictionary&#x20;

a、Dictionary 可以简单的看作是KeyValuePair 的集合；&#x20;

b、Dictionary 可以包含多个Key、Value的键值对。

下面的代码可能会帮助更好的理解它们之间的关系：&#x20;

<pre><code><strong>Dictionary&#x3C;int, string> myDic = new Dictionary&#x3C;int, string>();
</strong>foreach (KeyValuePair&#x3C;int, string> item in myDic)
{
    Console.WriteLine("Key = {0}, Value = {1}", item.Key, item.Value);
}
</code></pre>
