# Cinemachine

## 镜头转换的基本使用

1. 向一个Camera组件下添加CinemachineBrain组件
2. 创建两个空节点（注意不要向这两个节点再添加Camera组件），并各自挂上一个CinemchineVirtualCamera组件
3. 调整CinemchineVirtualCamera组件的Priortiy值，即可让Camera在这两个虚拟相机间来回切换。
