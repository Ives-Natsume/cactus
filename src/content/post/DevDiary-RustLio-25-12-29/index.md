---
title: "[日记 rust-lio 2025-12-29]"
publishDate: "29 December 2025"
updatedDate: "29 December 2025"
description: "开发日记 rust-lio 2025-12-29"
coverImage:
    src: "./image.png"
    alt: "Togawa Sakiko :)"
tags: ["SLAM", "Rust"]
---

好不容易暂时考试告一段落，不太想修rinko，刚好很久以前就一直想复现fast lio，所以开始重复造轮子

吸取上次的教训，渲染和算法混在一起了，这次要特别注意整体架构的解耦

论文还没有看，晚上先把几个结构体实现了一下。源程序里用到了`pcl::PointCloud<PointT>`，查看定义大致如下：

``` cpp
template <typename PointT>
struct PointCloud {
    std::vector<PointT> points;

    uint32_t width;
    uint32_t height;

    bool is_dense;
};
```

一开始我还以为`width`和`height`是包围盒类似物，但是类型又是uint，问了ai才知道其实在这里代表数组形状（逃

深度相机之类的传感器，获取的是一个矩阵，而对激光雷达，一般就是一个一维序列了
