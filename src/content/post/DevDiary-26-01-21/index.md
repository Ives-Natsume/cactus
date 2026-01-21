---
title: "[日记 rust-lio 2025-12-29]"
publishDate: "21 January 2026"
updatedDate: "21 January 2026"
description: "开发日记 rust-lio 2026-01-21"
coverImage:
    src: "./image.png"
    alt: "midori"
tags: ["SLAM", "Rust"]
---

## rust-lio 开发日记 2026-01-21

本来想测试一下ikd tree的下采样，结果发现下采样的保留率是100%

找了好久，中途还排查了udp解码器的bug，最后定位到imu的后向传播，时间戳处理有bug

处理完时间戳依然存在问题，并且imu存在漂移，找了很久最后怀疑是初始化imu的时候重力方向偏移的问题

最后通过映射重力测量值到S2流形上解决了这个问题，勉强不会偏移了，但是ikd tree依然在新增点，虽然保留率非常低

以及以后记得写一个断开传感器的impl，不然会占用udp端口，不方便跑单元测试

## 新玩具 midori

回家路上无聊写了个音频的频谱图，后续发展一下用来玩我的pluto

mac上装adi的libiio花了很久，趁现在没忘记先写个草稿，以后来考古

 - github下载libiio的pkg
 - brew install libusb和另外一个串口库，有点忘记了
 - 用install_name_tool改一下依赖库的路径，libiio默认安装在`/Library/Frameworks/libiio.framework`

梦到哪句写哪句 就这样吧
