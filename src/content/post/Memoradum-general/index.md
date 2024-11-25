---
title: "[备忘录]集腋成裘，记录一些零碎的信息"
publishDate: "5 October 2024"
updatedDate: "5 October 2024"
description: "备忘录，记录开发（整活）中遇到的零碎信息"
tags: ["备忘录", "image"]
coverImage: 
    src: "./cover.png"
    alt: "Rena"
---

## 软件

### pybind11的使用

激活环境后，pip install pybind11

在当前路径下新建cpp和py文件:

```python title = "setup.py"
from setuptools import setup, Extension
import pybind11

setup(
    name="test",  # Name of the module
    ext_modules=[
        Extension(
            "test",  # Must match the module name in PYBIND11_MODULE
            ["test.cpp"],
            include_dirs=[pybind11.get_include()],
            language="c++",
        )
    ],
)

```

```cpp title = "test.cpp"
#include <pybind11/pybind11.h>

namespace py = pybind11;

int add(int a, int b) {
    return a + b;
}

// Define the module named "test"
PYBIND11_MODULE(test, m) {
    m.doc() = "Example module created with Pybind11";  // Optional docstring
    // Add functions, classes, etc., here.
    m.def("add", &add, "A function that adds two numbers");
}

```

运行`python setup.py build_ext --inplace`完成构建，此时可以通过在`setup.py`中设置的模块名称调用c++程序：`import module_name`


### VWmare设置ssh

直接运行虚拟机的表现实在惨不忍睹，不如ssh一下试试看？

打开`虚拟网络设置`，选择`NAT`，`更改配置（管理员）`，选择添加规则，
主机和虚拟机端口均为22，在ip中填写虚拟机ip（ifconfig得到），然后在宿主机运行

```bash
cat ~/.ssh/id_rsa.pub | ssh 虚拟机用户名@虚拟机IP "cat >> ~/.ssh/authorized_keys"
```

按提示操作后即可ssh

参考文献：
https://www.cnblogs.com/BinarySong/p/16244415.html

### Spotify Premium的购买

因为支付地区检测的原因，可能难以直接购买会员。

低成本的方案是家庭车，但是不稳定。听说前不久一大批低价区的家庭车被Spotify处理了。

不在意成本的话可以直接去Amazon或者咸鱼代买礼品卡。

### VS Code LTeX – LanguageTool grammar/spell checking插件报错解决

参考方案：https://github.com/valentjn/vscode-ltex/issues/884#issuecomment-2278544786

由于版本更新，原来的行号已经不正确。检索到对对应位置并修改即可

## 硬件

### GeekWorm X1011 M2扩展板的2.4GHz无线干扰问题

疑似排线干扰了2.4GHz的信号，改用5GHz频段可解决问题。没试过屏蔽排线，不确定是不是排线的原因。

另外，他们家的板子真好看，包装也好看
