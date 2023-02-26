# Animator

使用Animator组件来给你场景中的某个游戏对象挂载Animation。

## Animator.StringToHash

此方法将Animator参数的字符串名转换成Hash值。

## Animation中SetBool和SetTrigger的区别 <a href="#articlecontentid" id="articlecontentid"></a>

SetBool通常代表对象的一种状态，比如跳跃、跑步等，可以通过GetBool来判断现在是否在这个状态中；而SetTrigger代表当时那一下触发的动画，比如攻击、受伤、死亡等，触发了它们的条件则进行播放。

SetTrigger

比较概念性，通常情况下都可以使用，区别不大。

ResetTrigger

重置参数状态为false，当动画机已经处于参数指定的触发状态时，该参数不会被自动置为false。

例如，当die作为trigger参数，Die作为die参数将触发的状态，当动画机已经处于Die状态时，将die参数置为true不会主动置为false。

## Animator Event

Animation Clip在动画机中可以添加事件，添加后在Animator同一节点上添加脚本，实现同名函数可以实现对事件的监听。



## \[Animator]获取到StateMachineBehaviour

怎么对Animator里面的状态机添加StateMachineBehaviour的脚本。

并在里面添加 自己的逻辑，传递参数 变量。



```
 
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
 
 //动画机的基类StateMachineBehaviour
public class TestStateMachine : StateMachineBehaviour
{
    /// <summary>
    /// 当过渡到状态机时，在第一个 Update 帧上进行调用。当过渡到状态机子状态时，不进行调用。
    /// </summary>
    /// <param name="animator"></param>
    /// <param name="stateInfo"></param>
    /// <param name="layerIndex"></param>
    public override void OnStateEnter(Animator animator, AnimatorStateInfo stateInfo, int layerIndex)
    {
        base.OnStateEnter(animator, stateInfo, layerIndex);
        int index = animator.GetInteger("Index");
        TestStateMachine [] machines = animator.GetBehaviours<TestStateMachine >();
        if (machines != null)
        {
            Debug.Log("!OnStateEnter Length:" + machines.Length);
            for (int i = 0; i < machines.Length; i++)
            {
                Debug.Log("!OnStateEnter i:"+i+"/" + machines[i].name+"/"+ index);
            }//machines[i]为脚本对象
        }
        Debug.Log("!OnStateEnter ");
    }//
    /// <summary>
    /// 
    /// </summary>
    /// <param name="animator"></param>
    /// <param name="stateInfo"></param>
    /// <param name="layerIndex"></param>
    public override void OnStateUpdate(Animator animator, AnimatorStateInfo stateInfo, int layerIndex)
    {
        base.OnStateEnter(animator, stateInfo, layerIndex);
 
        Debug.Log("!OnStateUpdate "+this.name+"/"+ animator.GetInteger("Index"));
    }//
    /// <summary>
    /// 
    /// </summary>
    /// <param name="animator"></param>
    /// <param name="stateInfo"></param>
    /// <param name="layerIndex"></param>
    public override void OnStateExit(Animator animator, AnimatorStateInfo stateInfo, int layerIndex)
    {
        base.OnStateEnter(animator, stateInfo, layerIndex);
 
        Debug.Log("!OnStateExit");
    }//
}
```



## 参考资料

1.CSDN [https://blog.csdn.net/Cleve\_baby/article/details/119037731](https://blog.csdn.net/Cleve\_baby/article/details/119037731)

2.CSDN [https://blog.csdn.net/BuladeMian/article/details/120309000](https://blog.csdn.net/BuladeMian/article/details/120309000)
