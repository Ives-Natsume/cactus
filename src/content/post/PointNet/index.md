---
title: "[备忘录]PointNet"
publishDate: "18 January 2025"
updatedDate: "18 January 2025"
description: "PointNet/PointNet++"
tags: ["备忘录","深度学习"]
coverImage:
    src: "./kumiko-sense.png"
    alt: "KumikoSense"
draft: true
---


## Ball Query

```python title="Ball Query"
#./models/pointnet2_utils.py

def square_distance(src, dst):
    """
    Calculate Euclid distance between each two points.

    src^T * dst = xn * xm + yn * ym + zn * zm;
    sum(src^2, dim=-1) = xn*xn + yn*yn + zn*zn;
    sum(dst^2, dim=-1) = xm*xm + ym*ym + zm*zm;
    dist = (xn - xm)^2 + (yn - ym)^2 + (zn - zm)^2
         = sum(src**2,dim=-1)+sum(dst**2,dim=-1)-2*src^T*dst

    Input:
        src: source points, [B, N, C]
        dst: target points, [B, M, C]
    Output:
        dist: per-point square distance, [B, N, M]
    """
    B, N, _ = src.shape
    _, M, _ = dst.shape
    dist = -2 * torch.matmul(src, dst.permute(0, 2, 1))
    dist += torch.sum(src ** 2, -1).view(B, N, 1)
    dist += torch.sum(dst ** 2, -1).view(B, 1, M)
    return dist


def query_ball_point(radius, nsample, xyz, new_xyz):
    """
    Input:
        radius: local region radius
        nsample: max sample number in local region
        xyz: all points, [B, N, 3]
        new_xyz: query points, [B, S, 3]
    Return:
        group_idx: grouped points index, [B, S, nsample]
    """
    device = xyz.device
    B, N, C = xyz.shape
    _, S, _ = new_xyz.shape
    group_idx = torch.arange(N, dtype=torch.long).to(device).view(1, 1, N).repeat([B, S, 1])    # Create index for all points
    sqrdists = square_distance(new_xyz, xyz)
    group_idx[sqrdists > radius ** 2] = N   # Set point index as 'N' if dist > radius
    group_idx = group_idx.sort(dim=-1)[0][:, :, :nsample]
    group_first = group_idx[:, :, 0].view(B, S, 1).repeat([1, 1, nsample])
    mask = group_idx == N
    group_idx[mask] = group_first[mask]
    return group_idx

```

**1** `square_distance(src, dst)`

`src`: A tensor of shape [B, N, C]

B: Batch size

N: Number of points in source set

C: Number of spatial dimensions, typically 3 (e.g, x, y, z)

`dst`: A tensor of shape [B, M, C]

M: Number of points in target set

Output: A tensor of shape [B, N, M]

`dist[B, N, M]`中的每个元素都代表一对点的平方距离。

**2** `query_ball_point(radius, nsample, xyz, new_xyz)`

说明见代码注释

