# Unity.Graphics

## Unity渲染绘制接口（三）Graphic

### 四、Graphics

与GL相比，Graphics是上层封装的渲染绘制接口，使用方式更符合Unity的数据结构，效率与易用性也更好。

#### 4.1 静态对象

Graphics的静态对象提供了缓冲区数据与渲染Setting。

* Graphics.activeColorBuffer：当前绘制区域颜色缓冲。
* Graphics.activeColorGamut：当前绘制区域色域。
* Graphics.activeDepthBuffer：当前绘制区域深度缓冲。
* Graphics.minOpenGLESVersion：最低OpenGL ES版本，可在PlayerSettings中设置。
* Graphics.preserveFramebufferAlpha：若勾选后可在Android平台中保留原生UI的缓冲alpha值，用以在原生UI上绘制对象。

#### 4.2 纹理

* Graphics.DrawTexture：在屏幕空间绘制纹理。
* Graphics.Blit(source, dest，mat)：使用着色器拷贝原纹理到RenderTexture
* Graphics.CopyTexture(sre, dst)：复制纹理（可对区域拷贝，无法改变分辨率）

在绘制屏幕纹理时，需要注意API导致的坐标系变化。在默认坐标系中，顶点坐标原点在屏幕左上角，uv坐标原点在绘制对象的左下角。Graphics.DrawTexture只能在OnGUI以及之后的生命周期方法中调用，否则无法显示。

```csharp
void TextureToScreen()
{
    Graphics.DrawTexture(screenRects[0], texture, uiRects[0], 0, 0, 0, 0, null);
    Graphics.DrawTexture(screenRects[1], texture, uiRects[1], 0, 0, 0, 0, null);
    Graphics.DrawTexture(screenRects[2], texture, uiRects[2], 0, 0, 0, 0, null);
    Graphics.DrawTexture(screenRects[3], texture, uiRects[3], 0, 0, 0, 0, null);
}
```

<figure><img src="https://pic1.zhimg.com/80/v2-6442e11b84023033f136eae67ca97e80_1440w.webp" alt=""><figcaption><p>Graphics.DrawTexture默认坐标系</p></figcaption></figure>

Graphics.Blit通常用于后处理，需要注意的是使用后会修改RenderTexture.active，在此接口后直接使用Graphics绘制接口，不会生效。

```csharp
var rt = RenderTexture.GetTemporary(texture.width, texture.height, 0);

Graphics.Blit(texture, rt, mats[0]);
RenderTexture.active = null;
Graphics.DrawTexture(screenRects[0], rt, uiRects[0], 0, 0, 0, 0, null);

Graphics.Blit(texture, rt, mats[1]);
RenderTexture.active = null;
Graphics.DrawTexture(screenRects[1], rt, uiRects[1], 0, 0, 0, 0, null);

Graphics.Blit(texture, rt, mats[2]);
RenderTexture.active = null;
Graphics.DrawTexture(screenRects[2], rt, uiRects[2], 0, 0, 0, 0, null);

Graphics.Blit(texture, rt, mats[3]);
RenderTexture.active = null;
Graphics.DrawTexture(screenRects[3], rt, uiRects[3], 0, 0, 0, 0, null);

RenderTexture.ReleaseTemporary(rt);
```

<figure><img src="https://pic2.zhimg.com/80/v2-1aaf28d9ea3bb2608948b2121d82ab95_1440w.webp" alt=""><figcaption><p>Graphics.Blit拷贝纹理</p></figcaption></figure>

Graphics.CopyTexture可以从一个纹理的像素数据拷贝到另一个纹理，也可以从mip或子区域中拷贝。拷贝对象时，不会进行放缩操作。因此原纹理与目标纹理的大小与格式必须匹配。当目标纹理与原纹理大小不一致时，只会读取目标纹理大小区域。若希望改变读取的纹理分辨率，只能手动创建与目标相同尺寸的RT，拷贝后再读取。

```csharp
public void ScreenShot()
{
    ...
    tempTexture2D = new Texture2D(Screen.width, Screen.height);
    tempTexture2D2 = new Texture2D(100, 100);

    // Cmaera渲染RT
    Camera.main.targetTexture = rt;
    Camera.main.Render();
    // 读取RT数据
    RenderTexture.active = rt;
    tempTexture2D.ReadPixels(new Rect(0, 0, Screen.width, Screen.height), 0, 0);
    tempTexture2D.Apply();

    ChangeTextureSize(rt, tempTexture2D2);
    ...
}

private void ChangeTextureSize(RenderTexture source, Texture2D target)
{
    var rt = RenderTexture.GetTemporary(target.width, target.height);
    var old = RenderTexture.active;
    Graphics.Blit(source, rt);
    target.ReadPixels(new Rect(0, 0, target.width, target.height), 0, 0);
    target.Apply();
    RenderTexture.active = old;
    RenderTexture.ReleaseTemporary(rt);
}
```

<figure><img src="https://pic3.zhimg.com/80/v2-ceae616fa96bc5b972c580d1e9f810da_1440w.webp" alt=""><figcaption><p>以不同分辨率保存截屏纹理</p></figcaption></figure>

QualitySettings.masterTextureLimit会影响Graphics.CopyTexture获取纹理的质量，具体见[API说明](https://link.zhihu.com/?target=https%3A//docs.unity3d.com/ScriptReference/Graphics.CopyTexture.html)。

#### 4.3 绘制Mesh

* Graphics.DrawMesh：绘制Mesh
* Graphics.DrawMeshInstanced：使用GPU instancing多次绘制相同的Mesh
* Graphics.DrawMeshNow：立即绘制Mesh（对象无法逐像素光照着色，也无法接收与投射实时阴影）

Graphics.DrawMesh在一帧中绘制网格，而无需创建GameObject。此接口不会立即绘制Mesh，只是将数据提交到正常的渲染流程中，如果希望立即绘制对象，使用Graphics.DrawMeshNow。

Graphics.DrawMeshInstanced使用GPU instancing特性绘制大量相同Mesh，同样为一帧渲染一次。需要注意的是，此接口将所有需要实例化的网格作为一组进行culling与sorting。在Mesh合并完成后，不会对其进行额外的视锥体剔除与遮挡剔除，也不会为透明度或深度对实例进行排序。单次调用实例数量上限为1023。

虽然看似Graphics.DrawMeshInstanced性能更高，但其没有Unity自带的视锥体剔除、LOD以及遮挡剔除等优化流程，因此要达到效率最优还需要额外的处理。

```csharp
if (useGPU)
{
    Graphics.DrawMeshInstanced(mesh, 0, meshMat[0], worldMats, worldMats.Length);
    Graphics.DrawMeshInstanced(mesh, 1, meshMat[1], worldMats, worldMats.Length);
    Graphics.DrawMeshInstanced(mesh, 2, meshMat[2], worldMats, worldMats.Length);
}
else
{
    foreach (var pos in worldPos)
    {
        Graphics.DrawMesh(mesh, pos, Quaternion.identity, meshMat[0], 0, Camera.main, 0);
        Graphics.DrawMesh(mesh, pos, Quaternion.identity, meshMat[1], 0, Camera.main, 1);
        Graphics.DrawMesh(mesh, pos, Quaternion.identity, meshMat[2], 0, Camera.main, 2);
    }
}
```

<figure><img src="https://pic3.zhimg.com/80/v2-69a8dd5e5f7092b3e073c63a90b96f72_1440w.webp" alt=""><figcaption><p>Graphics.DrawMesh与Graphics.DrawMeshInstanced对比</p></figcaption></figure>

#### 4.4 Mesh与Compute Shader

* Graphics.DrawMeshInstancedIndirect：与Graphics.DrawMeshInstanced类似，不同在于其参数来自于ComputeBuffer或GraphicsBuffer
* Graphics.DrawMeshInstancedProcedural：与Graphics.DrawMeshInstancedIndirect类似，不同点在于当能够从脚本中知道绘制对象数量时，可直接使用这一接口。
* Graphics.DrawProcedural：从ComputeBuffer中读取数据绘制对象。主要在支持Shader Model 4.5的硬件上有效，顶点着色器中使用SV\_VertexID和SV\_InstanceID变量从缓冲区获取数据。
* Graphics.DrawProceduralIndirect：与Graphics.DrawProcedural作用类似，直接从ComputeBuffer中读取几何数据。
* Graphics.DrawProceduralIndirectNow：与Graphics.DrawMeshNow作用类似，立即绘制对象
* Graphics.DrawProceduralNow：与Graphics.DrawMeshNow作用类似，立即绘制对象

由于此部分涉及到Compute Shader，篇幅过长，会在之后的章节中详细拆解。

#### 4.5 其它

* Graphics.SetRenderTarget(rt/rb)：设置渲染对象
* Graphics.SetRandomWriteTarget(index, cb/rt)：为Shader Model 4.5以上的PS设置随机渲染目标（UAV）
* Graphics.ConvertTexture(sre, dst)：转换纹理格式（RenderTextureFormat）
* Graphics.CreateGraphicsFence(GraphicsFenceType,SynchronisationStage)：创建一个GraphicsFence，在最后Blit、Clear、Draw、Dispatch或Texture Copy命令之后传递到GPU。通常用于GPU计算同步。
* Graphics.WaitOnAsyncGraphicsFence：GPU等待GraphicsFence同步。

## Mesh

Mesh.vertices不能单独赋值

```
            //无法通过索引器给vertices单个元素赋值
            mesh.vertices[i] = verBase + offset;
            //正确,整体赋值
            mesh.vertices=vertices;
```

## 参考资料

1. [https://zhuanlan.zhihu.com/p/442781670](https://zhuanlan.zhihu.com/p/442781670)
