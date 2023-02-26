# 人物移动

## 人物移动的几种方式

### Rigidbody.MovePosition

Rigidbody和Rigidbody2D均有此方法。

用鼠标操作时并不能每次都能正确运行，在弄清楚原因之前不采用此方式；键盘操作可以采用此方式。

因为涉及到移动，所以需要放在FixedUpdate里更新，时间间隔也应该采用Time.fixedDeltaTime。

代码示例：

```
    private void FixedUpdate()
    {
        var localPlayer = GameObject.FindObjectOfType<FarmPlayer>();
        var rigidgBody = localPlayer.GetComponent<Rigidbody2D>();
        rigidgBody.MovePosition(rigidgBody.position + runSpeed * Time.fixedDeltaTime * input);
    }
```

**在按下行走键使人物从奔跑切换成行走时，镜头会有反复轻微抖动的现象，暂时不知道如何解决。**

### Rigidbody.velocity

Rigidbody和Rigidbody2D均有此属性。

### transform.position

直接修改物体位置，鼠标操作推荐采用此方式。移动时要注意z轴数值，不要让目标点z轴数值到摄像机后方。

## 接口思想

移动可以通过修改Rigidbody.velocity实现，也可以通过修改transform.position，可以抽象出IMove接口统一这两种移动的接口，外部只需要统一调用譬如MoveTo()这样的方法，就可以忽略内部实现而实现统一调用。

同样，攻击也有很多种方式，让每一种攻击方式都实现IAttack接口的Attack(Vector3 dir)接口可以统一外部调用“朝某一方向攻击"这个动作。

**移动是使用鼠标还是键盘？是径直移动还是寻路？是使用Transform还是Rigidbody（是否使用物理模拟）？**

每一种选择背后都可以使用接口来隐藏具体的选择细节，外部只需要知道“我想移动了”，可以不关心你是如何移动的。接口让这些选择更加自由，也让代码更加简洁清晰。
