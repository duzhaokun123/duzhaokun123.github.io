---
title: Windows 窗口截图带透明通道
date: 2023-12-04 21:00:55
tags: [ windows, tools, ]
excerpt: 猜测出来的 (
---

## 起因

之前发的文章 有一些终端窗口的截图 不半透明的

之前用 kde 的时候 截图工具可以直接截出来 还带窗口装饰

但在 Windows 上的窗口截图似乎是截全屏然后裁剪的

## 探索

搜一下 windows screenshot with alpha 之类的关键词

结果是

- 直接干不成
- 有一种方法可以"还原"透明度 https://www.interact-sw.co.uk/iangblog/2007/01/30/recoveralpha

## 原理

根据不同背景时呈现结果不同计算 alpha

假设 alpha 混合是线性的

有

- 背景色:`c0`
- 前景色:`c1`
- alpha:`a`
- 结果:`cr`

则
{% mathjax %}
cr = c1 \times a + c0 \times (1 - a)
{% endmathjax %}

即 前景色和背景色的加权平均

记 `c0`为`0`时的结果为`cb` `c0`为`1`时的结果为`cw`

则

{% mathjax %}
\begin{align*}
cb &= c1 \times a + 0 \times (1 - a) \\
&= c1 \times a \\
cw &= c1 \times a + 1 \times (1 - a)
\end{align*}
{% endmathjax %}

代入得

{% mathjax %}
\begin{align*}
cw &= cw + 1 \times (1 - a) \\
cw &= cw + 1 - a \\
a &= 1 + cb - cw \\
cb &= c1 \times (1 + cb - cw) \\
c1 &= \frac{cw}{1 + cb - cw}
\end{align*}
{% endmathjax %}

解得

{% mathjax %}
\left\{
\begin{align*}
a &= 1 + cb - cw \\
c1 &= \frac{cw}{1 + cb - cw}
\end{align*}
\right.
{% endmathjax %}

## 实践

搞两张截图

| ![b](b.png) | ![w](w.png) |
|-------------|-------------|

写点代码

```python
import cv2 as cv
import numpy as np


def main():
    bb = cv.imread('b.png')
    wb = cv.imread('w.png')

    assert bb.shape == wb.shape

    tb = np.zeros((bb.shape[0], bb.shape[1], 4), dtype=np.uint8)

    for y in range(bb.shape[0]):
        for x in range(bb.shape[1]):
            b1, g1, r1 = bb[y, x]
            b2, g2, r2 = wb[y, x]
            b, ab = getColoeAlpha(b1, b2)
            g, ag = getColoeAlpha(g1, g2)
            r, ar = getColoeAlpha(r1, r2)
            a = int((ab + ag + ar) / 3)
            if a > 255:
                a = 255
            tb[y, x] = [b, g, r, a]

    cv.imshow('t.png', tb)
    cv.imwrite('t.png', tb)
    cv.waitKey(0)


def getColoeAlpha(cb, cw) -> (int, int):
    """
    
    :param cb: color with black background
    :param cw: color with white background
    :return: (c1, alpha)
    """
    alpha = 255 + cb - cw
    if alpha == 255:
        return cb, 255
    c1 = cb / ((255 + cb - cw) / 255)
    if c1 != c1:  # nan
        c1 = 0
    return int(c1), alpha


if __name__ == '__main__':
    main()
```

得到 (跑的好慢)

![t](t.png)

可能看不清 处理一下

![t2](t2.png)

## 反思

- 这里假设了 alpha 混合是线性的 但如果实际上不是呢
- 处理方法复杂过程慢
- 为什么不润回 Linux 呢(就是因为工业软件用不了啊)