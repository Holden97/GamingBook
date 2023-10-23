# Mesh

Mesh顺时针渲染才看得见

## 什么是submesh？

mesh的子mesh，当mesh比较复杂，submesh可以区分复杂mesh使用不同材质(material)的

## Mesh.RecalculateNormals的作用是什么？

## 使用六个顶点绘制的正方形和四个顶点绘制的正方形有哪些区别？

## 什么是sharedmesh？

MeshFilter.mesh只要执行过getter就会导致该meshFilter的.sharedMesh与其他本身同一实例引用的，变成了不同的&#x20;

对MeshFilter.sharedMesh getter执行就没有这个问题丢失统一引用的问题&#x20;

对MeshFilter.sharedMesh修改，其他相同实例的sharedMesh都会同时修改 对MeshFilter.mesh修改，再退出unity play模式后，就会还原数据&#x20;

对MeshFilter.sharedMesh修改，退出了unity play模式一样会保留模型外观，应该是在哪里的文件cache了持久化&#x20;

对MeshFilter.sharedMesh修改后，如果想还原被修改后的mesh，只要关掉(退出unity进程)，再重新打开unity的对应项目就可以看到模型还原了&#x20;

## Matrix4x4.Translate

平移方法，使用该方法可将一个mesh整体平移

## 参考

1. [https://blog.csdn.net/linjf520/article/details/90601102](https://blog.csdn.net/linjf520/article/details/90601102)
