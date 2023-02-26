# Unity自带对象池

对象池是一种对象复用模式。

Unity在2021.1之后的版本中给予了ObjectPool作为自带对象池类进行使用，除此之外，还有LinkedPool等多种对象池供使用。

Unity的自动化内存管理系统采用的是Boehm垃圾回收器，每当进行垃圾回收时，会停止运行程序代码，并且它只会在垃圾回收器完成所有工作后，才恢复正常的执行。可能会导致游戏延迟执行，持续时间一毫秒到几百毫秒不等。

实践过程中有问题需要日后进一步明确：

为什么FixedUpdate里调用创建方法会导致游戏表现极其卡顿，而在Update中调用就不会出现这样的情况？

## API

createFunc:对象实例化实现；

actionOnGet:出池回调；

actionOnRelease:进池回调；

actionOnDestroy:销毁回调；

collectionCheck:检查集合，对象进池前，会先检查是否在池中；

defaultCapacity:默认容量;

maxSize:池大小的最大值，超出这数值，进池的对象直接销毁;&#x20;

## 对象池示例

```
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Pool;

public class ObjectPoolTest : MonoBehaviour
{
    public int generateNum;
    public bool usePool = false;
    public GameObject cube;
    private ObjectPool<GameObject> pool;

    private void Start()
    {
        pool = new ObjectPool<GameObject>(OnCreate, OnGet, OnRelease, OnDestoryGameObject, true, 5000, 10000);
    }

    private void Update()
    {
        for (int i = 0; i < generateNum; i++)
        {
            //对象池
            if (usePool)
            {
                pool.Get();
            }
            //直接创建/销毁的对照组
            else
            {
                var go = Instantiate(cube, UnityEngine.Random.insideUnitSphere, Quaternion.identity);
                go.GetComponent<CubeTest>().Init(() => Destroy(go));
            }
        }
    }

    private void OnDestoryGameObject(GameObject obj)
    {
        Destroy(obj);
    }

    private void OnRelease(GameObject obj)
    {
        obj.SetActive(false);
    }

    private void OnGet(GameObject obj)
    {
        obj.SetActive(true);
        obj.transform.position = UnityEngine.Random.insideUnitSphere;
    }

    private GameObject OnCreate()
    {
        var go = Instantiate(cube, UnityEngine.Random.insideUnitSphere, Quaternion.identity);
        go.GetComponent<CubeTest>().Init(() => pool.Release(go));
        return go;
    }
}

```

```
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CubeTest : MonoBehaviour
{
    public Action OnFinish;
    private void OnCollisionEnter(Collision collision)
    {
        //碰撞到地面后销毁/回收
        if (collision.gameObject.tag == "Finish")
        {
            OnFinish?.Invoke();
        }
    }

    public void Init(Action OnFinish)
    {
        this.OnFinish = OnFinish;
    }
}

```

## 参考资料

1. 对象池介绍 [https://www.bilibili.com/read/cv10921967?from=search](https://www.bilibili.com/read/cv10921967?from=search)
