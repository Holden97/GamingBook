# 常见错误及解决

## Destroying assets is not permitted to avoid data loss.

该报错是试图使用GameObject.Destroy()方法删除一个赋值为预设的变量，预设不允许被删除，否则会造成数据丢失，可以删除用预设初始化后的实例，即使这个实例被赋值给一个成员变量。

**error: Could not set up a toolchain for Architecture x64. Make sure you have the right build tools installed for il2cpp builds. Details: UnityEngine.GUIUtility:ProcessEvent (int,intptr,bool&)**

用Visual Studio Installer安装Desktop Development With C++

**UnityException: FindObjectsOfType is not allowed to be called from a MonoBehaviour constructor (or instance field initializer), call it in Awake or Start instead. Called from MonoBehaviour 'UIManager' on game object '\[MonoSingleton]UIManager'. See "Script Serialization" page in the Unity Manual for further details.**



不能在MonoBehaviour 的构造器或Instance属性中使用FindObjectsOfType

也就是说MonoBehaviour其实是可以有构造函数的，只不过有额外的一些限制。

Resource.Load方法无法在继承了MonoBehaviour的类中的构造器中使用。具体可见Unity手册中脚本序列化(Script Serialization)一节。

**Resource ID out of range in SetResource**

创建资源过多，导致Resource ID不够用，

例如在Update中创建过多Mesh对象，导致上述问题。

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>
