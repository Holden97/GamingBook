# FlowCanvas项目阅读

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

UI控制流文档位置。



OnPointerUp是Selectable的一个虚方法，在继承Selectable类的子类中使用该方法调用onUp()，用onUp做get属性，获取到m\_onUp字段，注册类型为UnityEvent\<PointerEventData>的事件到m\_onUp当中。



<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

