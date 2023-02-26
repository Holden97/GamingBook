# 单例模式

## 普通单例

```
public class Singleton<T> where T : new()
{
    private static T instance;
    public static T Instance
    {
        get
        {
            if (instance == null) instance = new T();
            return instance;
        }
    }
}
```

## Unity下的Mono单例

```
using UnityEngine;

public abstract class MonoSingleton<T> : MonoBehaviour where T : MonoBehaviour
{
    protected static bool AppIsQuit;
    private static T instance;
    public static T Instance
    {
        get
        {
            if (AppIsQuit)
            {
                return null;
            }
            if (instance == null)
            {
                instance = FindObjectOfType<T>();
                if (instance == null)
                {
                    var go = new GameObject($"[MonoSingleton]{typeof(T).Name}");
                    instance = go.AddComponent<T>();
                }
                DontDestroyOnLoad(instance);
            }

            return instance;
        }
    }

    /// <summary>
    /// 已经拥有实例
    /// </summary>
    public static bool hasInstance
    {
        get { return instance != null; }
    }

    protected virtual void Awake()
    {
        if (instance == null)
        {
            instance = this as T;
            AppIsQuit = false;
            DontDestroyOnLoad(instance);
        }
        else
        {
            Destroy(gameObject);
        }
    }

    private void OnApplicationQuit()
    {
        AppIsQuit = true;
    }
}

```

网络中有些版本实现了[线程锁](https://blog.csdn.net/xdedzl/article/details/85039761)这里暂时不考虑[线程安全](https://www.jianshu.com/p/854649bc0ce6)的问题。

## 几个问题/解释

单例模式在程序退出时，由于Destroy顺序不可控（即使修改了脚本执行顺序也不可控），所以需要在程序退出时添加标志位，用标志位来告诉其他调用MonoInstance的对象，程序退出了，我的Instance先你一步被销毁了，如果在程序退出阶段你再让我创建新的单例，程序就会报错了。（[Some objects were not cleaned up when closing the scene. (Did you spawn new GameObjects from OnDestroy?](https://www.cnblogs.com/weiqiangwaideshijie/p/9674111.html)），所以我只能给你返回空值了。
