# AnimatorOverrideController

AnimatorOverrideController被用来覆盖指定Avatar身上的特定动画。使用AnimatorOverrideController来替换一个基于相同AnimatorController的Animator.runtimeAnimatorController不会重置该Animator的当前状态。

可以通过三种方式来使用AnimatorOverrideController。

<mark style="color:blue;">**一**</mark>**、在编辑器中创建一个AnimatorOverrideController。**

<mark style="color:blue;">**二**</mark>**、\[基础用法]在运行时每帧改变AnimationClip。**

```
// 示例代码
using UnityEngine;

public class SwapWeapon : MonoBehaviour
{
    public AnimationClip[] weaponAnimationClip;

    protected Animator animator;
    protected AnimatorOverrideController animatorOverrideController;

    protected int weaponIndex;

    public void Start()
    {
        animator = GetComponent<Animator>();
        weaponIndex = 0;

        animatorOverrideController = new AnimatorOverrideController(animator.runtimeAnimatorController);
        animator.runtimeAnimatorController = animatorOverrideController;
    }

    public void Update()
    {
        if (Input.GetButtonDown("NextWeapon"))
        {
            weaponIndex = (weaponIndex + 1) % weaponAnimationClip.Length;
            animatorOverrideController["shot"] = weaponAnimationClip[weaponIndex];
        }
    }
}
```

注意，这种用法使用了AnimatorOverrideController.this\[string]方法(`animatorOverrideController["shot"] = weaponAnimationClip[weaponIndex];`)，它返回对应名称的AnimationClip。应避免在同一帧的同一Animator中多次调用该方法。因为每次调用都会重新分配Animator的clip绑定。针对这种情况，应该使用<mark style="color:blue;">**三**</mark>**、AnimatorOverrideController.ApplyOverrides。**

```
using UnityEngine;
using System.Collections.Generic;

//注意，这里List<KeyValuePair<AnimationClip, AnimationClip>中，前一个AnimationClip是替换之前的AnimationClip，而后一个AnimationClip是替换之后的AnimationClip。
public class AnimationClipOverrides : List<KeyValuePair<AnimationClip, AnimationClip>>
{
    public AnimationClipOverrides(int capacity) : base(capacity) {}

    public AnimationClip this[string name]
    {
        get { return this.Find(x => x.Key.name.Equals(name)).Value; }
        set
        {
            int index = this.FindIndex(x => x.Key.name.Equals(name));
            if (index != -1)
                this[index] = new KeyValuePair<AnimationClip, AnimationClip>(this[index].Key, value);
        }
    }
}

public class Weapon
{
    public AnimationClip singleAttack;
    public AnimationClip comboAttack;
    public AnimationClip dashAttack;
    public AnimationClip smashAttack;
}

public class SwapWeapon : MonoBehaviour
{
    public Weapon[] weapons;

    protected Animator animator;
    protected AnimatorOverrideController animatorOverrideController;

    protected int weaponIndex;

    protected AnimationClipOverrides clipOverrides;
    public void Start()
    {
        animator = GetComponent<Animator>();
        weaponIndex = 0;

        animatorOverrideController = new AnimatorOverrideController(animator.runtimeAnimatorController);
        animator.runtimeAnimatorController = animatorOverrideController;

        clipOverrides = new AnimationClipOverrides(animatorOverrideController.overridesCount);
        animatorOverrideController.GetOverrides(clipOverrides);
    }

    public void Update()
    {
        if (Input.GetButtonDown("NextWeapon"))
        {
            weaponIndex = (weaponIndex + 1) % weapons.Length;
            clipOverrides["SingleAttack"] = weapons[weaponIndex].singleAttack;
            clipOverrides["ComboAttack"] = weapons[weaponIndex].comboAttack;
            clipOverrides["DashAttack"] = weapons[weaponIndex].dashAttack;
            clipOverrides["SmashAttack"] = weapons[weaponIndex].smashAttack;
            animatorOverrideController.ApplyOverrides(clipOverrides);
        }
    }
}
```

