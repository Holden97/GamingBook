# 代码技巧

## 屏蔽特定Warning

在产生对应warning的语句前后添加

\#progma warning disable **414**\[对应warning编号,如CS0414,那么就写414]

语句

\#progma warning restore **414**\[与上方对应]

## Debug

清除所有控制台中的Error信息

[Debug.ClearDeveloperConsole();](https://docs.unity3d.com/ScriptReference/Debug.ClearDeveloperConsole.html)
