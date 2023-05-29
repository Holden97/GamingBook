# sqrMagnitude和magnitude比较

Vector3.magnitude 是指向量的长度，Vector3.sqrMagnitude 是指向量长度的平方 在Unity当中使用平方的计算要比计算开方的速度快很多。

因为向量的大小由勾股定理得出，所以有开方操作，所以如果只是单纯比较向量之间的大小的话，建议使用Vector3.sqrMagnitude进行比较即可。提高效率和节约性能 。
