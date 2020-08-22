---
title: HSL/HSV/RGB
categories: notes
date: 2020-07-18 10:08:39
tags:
	- Color
photos:
---



# HSL&HSV

>**HSL**和**HSV**都是一种将[RGB色彩模型](https://zh.wikipedia.org/wiki/三原色光模式)中的点在[圆柱坐标系](https://zh.wikipedia.org/wiki/圓柱坐標系)中的表示法。这两种表示法试图做到比基于[笛卡尔坐标系](https://zh.wikipedia.org/wiki/笛卡尔坐标系)的几何结构RGB更加直观。

>因为HSL和HSV是设备依赖的RGB的简单变换，(*h*, *s*, *l*)或 (*h*, *s*, *v*)三元组定义的颜色依赖于所使用的特定[红色](https://zh.wikipedia.org/wiki/红色)、[绿色](https://zh.wikipedia.org/wiki/绿色)和[蓝色](https://zh.wikipedia.org/wiki/蓝色)“[加法原色](https://zh.wikipedia.org/wiki/原色)”。每个独特的RGB设备都伴随着一个独特的HSL和HSV空间。

![400px-Hsl-hsv_models.svg](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/16/110837-922292.png)

在我理解，HSL和HSV本质上都是RGB的衍生模型，展示颜色的另一种表达方式，在某些场景下使用更为方便。

## HSL

(Hue, Saturation, Lightness)

H(Hue)：色相，色彩的基本属性，取值0~360%

S(Saturation)：饱和度，色彩的纯度，取值0~100%

L(Lightness)：亮度，取值0~100%

## HSV（又名：HSB）

(Hue, Saturation, Value)

H(Hue)：色相，色彩的基本属性，取值0~360%

S(Saturation)：饱和度，色彩的纯度，取值0~100%

V(Value)：明度，取值0~100%

B(Brightness)：亮度

## HSL&HSV比较

|        | HSL                                    | HSV                                                          |
| ------ | -------------------------------------- | ------------------------------------------------------------ |
| 饱和度 | 从完全饱和色变化到等价的灰色           | 在极大值V的时候，饱和度从全饱和色变化到白色，这可以被认为是反直觉的 |
| 亮度   | 跨越从黑色过选择的色相到白色的完整范围 | V分量只走一半行程，从黑到选择的色相                          |

![400px-HSL_HSV_cylinder_color_solid_comparison](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/16/104937-633960.png)

# RGB

>**三原色光模式**（**RGB color model**），又称**RGB颜色模型**或**红绿蓝颜色模型**，是一种[加色模型](https://zh.wikipedia.org/wiki/加色法)，将[红](https://zh.wikipedia.org/wiki/红)（**R**ed）、[绿](https://zh.wikipedia.org/wiki/绿色)（**G**reen）、[蓝](https://zh.wikipedia.org/wiki/蓝)（**B**lue）三[原色](https://zh.wikipedia.org/wiki/原色)的色光以不同的比例相加，以合成产生各种色彩光。

# RGB to HSL&HSV

设 (*r*, *g*, *b*)分别是一个颜色的红、绿和蓝坐标，它们的值是在0到1之间的实数。设*max*等价于*r*, *g*和*b*中的最大者。设*min*等于这些值中的最小者。要找到在HSL空间中的 (*h*, *s*, *l*)值，这里的*h* ∈ [0, 360）度是角度的色相角，而*s*, *l* ∈ [0,1]是饱和度和亮度，计算为：

![fffb33944b2529ac9398bfb365968795a03b3ddc](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/18/103327-962988.svg%2Bxml)

![eeaf1fee3d0a586a19d95e964249b5970c90b2fd](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/18/103344-294137.svg%2Bxml)

*h*的值通常规范化到位于0到360°之间。而*h* = 0用于*max* = *min*的（定义为灰色）时候而不是留下*h*未定义。

HSL和HSV有同样的色相hue定义，但是其他分量不同。HSV颜色的*s*和*v*的值定义如下：

![b37c2df97b6e7c75eaf3865185d53a1f2364d71e](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/18/105105-899075.svg%2Bxml)

![9ff1cd8d741c0aeb626d2ca4e974f1151955fd20](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/18/103408-267151.svg%2Bxml)



# HSL to RGB

## 公式推导

给定HSL空间中的 (*h*, *s*, *l*)值定义的一个颜色，带有*h*在指示色相角度的值域[0, 360]中，分别表示饱和度和亮度的*s*和*l*在值域[0, 1]中，相应在RGB空间中的 (*r*, *g*, *b*)三原色，带有分别对应于红色、绿色和蓝色的*r*, *g*和*b*也在值域[0, 1]中，它们可计算为：

首先，如果*s* = 0，则结果的颜色是非彩色的、或灰色的。在这个特殊情况，*r*, *g*和*b*都等于*l*。注意*h*的值在这种情况下是未定义的。

当*s* ≠ 0的时候，可以使用下列过程：

![31218c97930f2fb0ae17a11fcf6868881390fedc](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/18/103905-222782.svg%2Bxml)

![d772c2636abcc1648795983eab33420f37f820b2](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/18/103952-866041.svg%2Bxml)

![15a30519eb199214fa54e0d46b3c13655f0e5139](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/18/104019-808468.svg%2Bxml)

![11813c13aef3b4e9d47e88ebe8ca1fb9335ce661](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/18/104021-25366.svg%2Bxml)

![57abb79554d826f589807a2ceb880b3e204b7486](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/18/104223-522781.svg%2Bxml)

![9751ca52fc3af67f6e9d9ed4c54e301f073bfeb8](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/18/104249-370233.svg%2Bxml)

![6d996f0bb614dd366c387d27cf0b6426f1ab2f71](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/18/104247-338152.svg%2Bxml)

![e915c9c9b12b4b4898464bbad8ab262cb4cfdafe](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/18/104322-212626.svg%2Bxml)

对于每个颜色[向量](https://zh.wikipedia.org/wiki/位置向量)*Color* = (*ColorR*, *ColorG*, *ColorB*) = (*r*, *g*, *b*),

![4f90cdc844d556702d0190513a6c903fc9bda77f](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/18/104357-670818.svg%2Bxml)

![ee78051dcbe7f73270a192d8514040ed997c09c1](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/18/104413-8706.svg%2Bxml)

## 代码实现

以下是从HSL到RGB转换的代码实现（javascript）

```javascript
// hsl转rgb,h:色相，s:饱和度，l:亮度
hsl2rgb (h, s, l) {

    let r, g, b

    if (s === 0) {
        r = g = b = l; // achromatic
    } else {
        let q = l < 0.5 ? l * (1 + s) : l + s - l * s
        let p = 2 * l - q
        r = hue2rgb(p, q, h + 1/3)
        g = hue2rgb(p, q, h)
        b = hue2rgb(p, q, h - 1/3)
    }

    return [Math.floor(r * 255), Math.floor(g * 255), Math.floor(b * 255)]

},

// hue转rgb
hue2rgb (p, q, t) {

    if(t < 0) t += 1
    if(t > 1) t -= 1
    if(t < 1/6) return p + (q - p) * 6 * t
    if(t < 1/2) return q
    if(t < 2/3) return p + (q - p) * (2/3 - t) * 6
    return p

},

// 百分比转颜色，i介于0和1之间，min设置i颜色范围的最小值及max最大值，可调整参数使颜色效果更合适
percent2color (i, min, max) {

    let ratio = i

    if (min > 0 || max < 1) {
        if (i < min) {
            ratio = 0
        } else if (i > max) {
            ratio = 1
        } else {
            let range = max - min
            ratio = (i - min) / range
        }
    }

    // hue中,red = 0° and green = 120°
    // 参数可以稍微调整一下，调到合适的值
    let hue = (max - ratio) * 110 / 360
    let rgb = hsl2rgb(hue, 0.95, 0.65)

    return 'rgb(' + rgb[0] + ',' + rgb[1] + ',' + rgb[2] + ')'

}
```



# HSV to RGB

类似的，给定在HSV中 (*h*, *s*, *v*)值定义的一个颜色，带有如上的变化于0到360之间的*h*，和分别表示饱和度和明度的变化于0到1之间的*s*和*v*，在RGB空间中对应的 (*r*, *g*, *b*)三原色可以计算为（R,G,B变化于0到1之间）：

![92674874764731b6d853f58a143998aec422cbfa](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/18/104649-312600.svg%2Bxml)

![ca476d6c78c174d918811a6efd4ffb3ab3aec493](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/18/104829-235181.svg%2Bxml)

![03a237ea233744959cef47f832cacc2e305f6bda](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/18/104746-22204.svg%2Bxml)

![6585efc49ac1107fbd6a7a8aa45bd2cb466e2369](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/18/104906-221303.svg%2Bxml)

![5bc991a5b2b9422eee7a1e647908b42731625566](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/18/104917-337473.svg%2Bxml)

对于每个颜色向量 (*r*, *g*, *b*),

![5d344b1b86e409a3956e5fa1b76c963f09c019d1](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/18/104937-636649.svg%2Bxml)

# percentage to HSL

以下内容是，在可用HSL定义颜色的网页中，可用数值计算对应颜色。

```javascript
/* 
   百分比,颜色从hue0到hue1
   percentage:占比，取值0~1
   hue0:起始颜色色相hue
   hue1:终点颜色色相hue
*/
percentageToHsl (percentage, hue0, hue1) {
	let hue = (percentage * (hue1 - hue0)) + hue0
	return 'hsl(' + hue + ', 100%, 63%)'
}
```



# 参考链接

[Calculate color values from green to red | stack overflow](https://stackoverflow.com/questions/17525215/calculate-color-values-from-green-to-red)

[hsl to rgb code](http://jsfiddle.net/0bu579sd/)

[HSL to RGB color conversion | stackflow](https://stackoverflow.com/questions/2353211/hsl-to-rgb-color-conversion)

[HSL和HSV色彩空间 | wiki](https://zh.wikipedia.org/wiki/HSL和HSV色彩空间#从HSL到RGB的转换)

[三原色光模式 | wiki]([https://zh.wikipedia.org/wiki/%E4%B8%89%E5%8E%9F%E8%89%B2%E5%85%89%E6%A8%A1%E5%BC%8F](https://zh.wikipedia.org/wiki/三原色光模式))



---

写文不易，如需转载，请注明出处。

注意文章编写时间，一切以官方文档为主。

如果某处写的有问题，欢迎发邮件，一起交流讨论，共同进步。