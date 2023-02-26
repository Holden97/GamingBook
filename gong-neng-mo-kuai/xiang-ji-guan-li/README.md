# 相机管理

正交投影的大小是屏幕高度的一半。所以当正交投影的尺寸是8.4375时，屏幕的高度则是16.875。当图片分辨率为每单位16像素时，目标屏幕的像素为16\*16.875=270px。宽度在16:9的屏幕下就是270\*16/9=480px。

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

这里的高度(Height)其实意思就是屏幕长度。

## 2D相机边界限制

1. CinemachineVirtualCamera选择AddExtension，然后选择添加CinemachineConfiner.
2. 添加一个包含PolygonCollider2D组件的根节点。编辑该组件，使其外形符合自己的限定。
3.  在CinemachineVirtualCamera所在节点添加脚本，获取PolygonCollider2D所设置边框，将该边框运用到CinemachineConfiner的Bounding Shape 2D中。\
    示例代码：

    ```
    using UnityEngine;
    using Cinemachine;

    public class SwitchConfineBoundingShape : MonoBehaviour
    {
        // Start is called before the first frame update
        void Start()
        {
            SwitchBoundingShape();
        }

        /// <summary>
        /// Switch the collider that cinemachine uses to define the edges of the screen.
        /// </summary>
        private void SwitchBoundingShape()
        {
            PolygonCollider2D polygonCollider2D = GameObject.FindGameObjectWithTag(Tags.BoundsConfiner).GetComponent<PolygonCollider2D>();
            CinemachineConfiner cinemachineConfiner = GetComponent<CinemachineConfiner>();
            cinemachineConfiner.m_BoundingShape2D = polygonCollider2D;

            cinemachineConfiner.InvalidatePathCache();
        }
    }

    ```
