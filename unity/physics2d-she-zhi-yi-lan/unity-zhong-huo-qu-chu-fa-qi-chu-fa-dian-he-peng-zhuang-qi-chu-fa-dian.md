# Unity中获取触发器触发点和碰撞器触发点

触发器不能直接获取碰撞点，但是可以通过获取离该包围盒上最近的点来判断。

```
private void OnTriggerEnter2D(Collider2D other)
{
        hitPos = other.bounds.ClosestPoint(transform.position);
        print("碰撞点" + hisPos);
}
```

1 2 3 4 5 碰撞器可以直接取到碰撞点

```
public virtual void OnCollisionEnter(Collision pOther)
{
        ContactPoint contact = pOther.contacts[0];
        Vector3 pos = contact.point;    //这个就是碰撞点
}
```



参考

1. [https://blog.csdn.net/qq\_39162826/article/details/120702433](https://blog.csdn.net/qq\_39162826/article/details/120702433)
