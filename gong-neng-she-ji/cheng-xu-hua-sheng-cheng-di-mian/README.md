# 程序化生成地面

## Voxels=Volumn+Pixels&#x20;

## Mesh.vertices

Vector3\[]

为什么 unity 中正方体网格的顶点数量为 24？

1.首先看下在 3Dmax 中的顶点数量为 8，面片（四角面片）为 6（图中显示三角形数量是为了方便我们计算）：

2.模型导入unity 之后，会对四边形（或多边形）进行处理，转成三角面片。因此，对于上述例子来说，面数 = 原面数 \* 2 = 12；

3.unity 中显示的顶点数和面片数是实际传递给 GPU 的数量。由于传递过程中顶点是无法被得知是属于哪个面片上的顶点，而且每个顶点连 接 3 个面片，因此一个顶点同时拥有 3 个法线等属性，这就需要 3 个顶点来表示，这就有了 3 \* 8 = 24 个顶点。

4.一个四角面片分割成两个三角面片，同一个顶点应该连接着 6 面片，为什么是 24 而不是 36？

确实是这样，但是这样做需要处理很多个顶点。

unity 中 选中模型之后，Inspector 面板有个 Weld Vertices 的属性，默认开启，开启会筛除掉具有相同信息的顶点。

## Mesh.normal

Vector3\[]

Vector2\[] 顶点法线，和顶点一一对应。

## Mesh.uv

Vector2\[] 顶点坐标的纹理映射坐标

## Mesh.triangles

int\[] 三角形顶点，所存储数字与vertices索引对应，三角形三个点所代表的的顺时针方向为可见的一面，反之则不可见。triangles内部各个三角形之间的顺序可以变动，不影响渲染。



## Texture的Filter Mode



参考

1.[https://blog.csdn.net/qq\_19533777/article/details/104528361](https://blog.csdn.net/qq\_19533777/article/details/104528361)
