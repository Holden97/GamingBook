# Task

在 C# 中，可以使用 `Task` 类型和 `await` 关键字来执行异步操作。以下是一个简单的示例：

```
csharp复制代码async Task<int> MyAsyncMethod()
{
    // 等待一个异步操作完成
    await Task.Delay(1000);

    // 返回一个整数值
    return 42;
}
```

在上面的代码中，`MyAsyncMethod()` 是一个异步方法，它使用 `await` 关键字等待一个 `Task.Delay()` 方法完成，该方法模拟了一个异步操作。当异步操作完成时，方法将会返回一个整数值。

要调用这个异步方法并获取其返回值，可以使用如下代码：

```
csharp复制代码int result = await MyAsyncMethod();
```

在上面的代码中，`await MyAsyncMethod()` 表达式将会等待异步方法完成，并返回其结果。由于 `MyAsyncMethod()` 返回一个整数值，因此可以将其赋给一个整数类型的变量 `result`。

需要注意的是，异步方法必须标记为 `async`，并且返回值类型必须是 `Task` 或者 `Task<TResult>` 才能使用 `await` 关键字。另外，`await` 关键字只能在异步方法中使用。



### 前言 <a href="#qian-yan" id="qian-yan"></a>

大家都知道程式是由上至下，有順序地逐步執行的。

但是有時候還是會希望它可以不要這麼乖，有些地方如果是要等一下下才會完成的工作，會希望程式不要卡死在那等它完成，先繼續往下跑，這時候就需要 **『異步執行』** (Asynchronous Execution)的幫助!

微軟的文件雖然蠻多都寫得不是很易懂，但是文件對異步執行給了一個蠻有趣的例子:

> 您撰寫了類似下列清單的指示，來說明如何準備早餐：

> 1.倒杯咖啡

> 2.熱鍋，然後煎兩顆蛋

> 3.煎三片培根

> 4.烤兩片吐司

> 5.在吐司塗上奶油和果醬

> 6.倒杯柳橙汁。

> 如果您有烹飪經驗，您會非同步地執行這些指示。 您會從為雞蛋熱鍋開始，然後開始煎培根。 等到將麵包放入烤麵包機，再開始煎蛋。 在程序的每個步驟，您會開始一個工作，然後將注意轉移到其他需要您注意的工作。

開發上最常見的用途在使用者介面(GUI)，我們不希望使用者在送出請求後整個程式就完全停擺了! 所以必須要將等候資料或是計算這一段擺在背景執行，以免影響使用者體驗。

而C#提供了`async`及`await`的關鍵字及`Task`，讓我們可以在不用寫很多程式碼的情況下撰寫出有異步執行功能的程式碼。

### 範例 <a href="#fan-li" id="fan-li"></a>

這邊用一個簡單的程式來示範異步執行的用法。

程式有兩個按鈕，一個會同步處理文件，另一個則用異步執行的方式處理。

![](https://i.imgur.com/boHlPpJ.jpg)

#### 程式碼 <a href="#cheng-shi-ma" id="cheng-shi-ma"></a>

```
public partial class Form1 : Form
{
    public Form1()
    {
        InitializeComponent();
    }

    private int CountChars()
    {
        int count = 0;
        // 讀取文件裡的字元
        using(StreamReader reader = new StreamReader("C:\\Users\\ryanchen34057\\Desktop\\test.txt"))
        {
            // 將所有內容讀進記憶體暫存
            string content = reader.ReadToEnd();
            count = content.Length;

            // 暫停5秒鐘
            Thread.Sleep(5000);
        }
        return count;
    }
    // 同步處理
    private void Btn_processFile_Click(object sender, EventArgs e)
    {
        label_message.Text = "處理文件中.....";
        int count = CountChars();
        label_message.Text = "總共有" + count.ToString() + "個字";
    }

    // 異步處理
    private async void Button_async_Click(object sender, EventArgs e)
    {
        Task<int> task = new Task<int>(CountChars);
        task.Start();

        label_message.Text = "處理文件中.....";
        int count = await task;
        label_message.Text = "總共有" + count.ToString() + "個字";
    }
}
```

先來看**同步處理**的效果(請無視Label沒置中這件事XD)

![](https://i.imgur.com/jTRRlgr.gif)

可以發現按鈕按下去程式會卡住，然後視窗也沒辦法移動，要等到文件處理完後使用者才能拿回對視窗的控制。

再來是**異步處理**

![](https://i.imgur.com/jxMgZdB.gif)

不僅按鈕按下去會馬上跳出處理文件的訊息，而且處理的同時使用者也可以做其他事!

接下來來看一下異步處理的程式碼吧!

### 異步處理程式碼 <a href="#yi-bu-chu-li-cheng-shi-ma" id="yi-bu-chu-li-cheng-shi-ma"></a>

```
// 異步處理
private async void Button_async_Click(object sender, EventArgs e)
{
    Task<int> task = new Task<int>(CountChars);
    task.Start();

    label_message.Text = "處理文件中.....";
    // 用await來讓程式等task執行完後再繼續
    int count = await task;
    label_message.Text = "總共有" + count.ToString() + "個字";
}
```

首先，異步處理的函數必須用`Task`這個物件包起來，`Task`是一個可以處理泛型的物件，`<>`裡的是函數會回傳的型別，然後呼叫`start`。

因為我們要等程式處理完文件後才能顯示字數，所以要在`task`前面加上`await`的關鍵字， **讓程式知道我們要等那個函數有回傳值之後才繼續往下跑!**

這邊要注意的是，只要在函數裡有用到`await`，一定要加上`async`!(IDE會提醒你的XD)

## 参考资料

1. [https://ryanchen34057.github.io/2019/09/11/CSharpAsyncAwait/](https://ryanchen34057.github.io/2019/09/11/CSharpAsyncAwait/)
