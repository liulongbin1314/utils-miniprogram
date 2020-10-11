## 功能

1. **为原生小程序的 Page 页面提供 mixins 支持**

2. **合并响应式数据对象的辅助方法**

## 安装

```bash
npm install @escook/utils-miniprogram
```

## 为原生小程序的 Page 页面提供 mixins 支持

### 需求说明

由于原生微信小程序只为**自定义组件**提供了 mixins 的支持，**并没有为 Page 页面提供 mixins 的支持**。因此，多个页面之间想要共享通用的代码是一件困难的事情，您可能需要把相同的代码在多个页面之间进行复制粘贴的操作，这非常不利于项目的维护。

**如果您需要在多个 Page 页面之间共享通用的代码**（例如：共享 data 数据定义、共享生命周期函数的处理逻辑等），此时，您可以使用 `@escook/utils-miniprogram` 为 Page 页面拓展的 mixins 功能，来实现代码逻辑的复用！

### 导入拓展的 mixins 功能

在 `app.js` 中导入 `mixins.js` 之后，即可自动为 `Page` 拓展 `mixins` 功能：

```js
import '@escook/utils-miniprogram'
```

### 定义 mixins

```js
// 向外导出一个 mixin 对象
export default {
  /**
   * data - 私有数据
   */
  data: {
    // msg 数据名称冲突，因此 msg 的值以 Page 中的定义为准
    msg: 'Hello escook',
    // address 没有在 Page 的 data 中定义，所以 address 对应的值会生效
    address: '北京市顺义区',
  },

  /**
   * onLoad - 生命周期函数
   */
  onLoad() {
    console.log('触发了 mixin 中定义的 onLoad')
  },
}
```

### 在 Page 中使用 mixin

```js
// 导入需要的 mixin 对象
import testMix from '../test-mixin'

Page({
  // 通过 mixins 数组挂载需要的 mixin 对象
  mixins: [testMix],

  /**
   * 页面的初始数据
   */
  data: {
    // 注意：如果 Page 中的数据名称与 mixin 中的数据名称冲突，则以 Page 中的声明为准
    msg: 'Hi! escook',
  },

  /**
   * 生命周期函数--监听页面加载
   */
  onLoad: function (options) {
    // 注意：如果分别在 mixin 中和 Page 中声明了相同的生命周期函数，
    //       则会按照顺序，先执行 mixin 中的生命周期函数、再执行 Page 中的生命周期函数
    console.log('触发了 Home 页面的 onLoad')
  },
})
```

### 合并后 - 最终的数据

<img src='data:img/jpg;base64,iVBORw0KGgoAAAANSUhEUgAAAigAAADdCAIAAAAerx5RAAAACXBIWXMAABJ0AAASdAHeZh94AAAc
GklEQVR42u2d228cVZrA83f4j5iXvGxgHtC87CtidnZGedgnKwI8GkBck3EmTjYSwUwuHrLM2kgo
5rIbsqgdyCYbhJYNaBS8cdtBG4RHKNhOPIlpUO6BQSGZ7Kn7d06dU13dXVV98e+nT1HcXZdTXdXn
1993TldvuAUAAFAhG3gJAAAA8QAAAOIBAABAPAAAgHgAAAB6Wzz1ep3TAACAeBAPAAB0TzzXr1//
/PPPT548+d577x11o55VyywuLqrlEQ8AALQvnitXrrz66qt79+7dt2/fXjf7fF577TW1POIBAID2
xfPNN99s3bpVJT1Nl1TLbNu2TS2PeAAAoCPxPP/88znFo5bsoniW3x/dnDB6bKWSV/HM1ObNU3Pd
Oofe3mO614zKWDnmneNJ40CX1aNTZ7p4Cip+5ZePiSt99P3l8vY0N9kz15V/qdsP1n+qzQugS+/f
C0dGNm4cObxUwJYOP75RZ3wW8VQlHv+tOHpsWVxPFfVE3ROPL9pk18vvT1Xk2q6Lx+xl8oinNDlV
fQHMTWn979zU5Fx119tk9xwUfsZKv9ShhvtKPMoWIyOPbxw5cqEQ8YyfNpSmPWLn9PjGxw9f6MaZ
LFg8q6urx48fv3r1avXi8T6Xdest0TXxeO+3Uj/t9qp4po7pxl1f4unuB53uimd0aip9Er1LYnR0
tK/Es3R4RHX6xXT9pnhCqTTNe/pRPBcvXlxaWlKmaTQayjQ3b95srwXFiMe/8jI+7MsSnPZR0fuU
lFQtxIU7lxSwxDvNLzukFtYvXLFM6Vdzlm5XRDEmSQSDQ7b1xcFRBB8qo22KY4lfXlHkSTZrfbBE
8cwFe0yO3ZBKqj0rWmnq2Iq+vHfU2gFGT4nLQMssvcttzr+ovMtJXgAZ5aAiXwHX1e46EeJA/MeD
Q1jW+vTgz+DTzJy/HW8v0ZJmcc+89vzzUr54/Jddv8ZUS4IGS/HY36q2l6Lq0xclJX6uMzu+MZWs
BEIyi2aeXdQqsy9Hz7w8myUe7UFZiws3mGwn3pRy4cb0xntMPEeOHNm3b9/rr79+4sSJc+fOra2t
XfG5fv36jRs38nuoGPEkbxtH76z3vNGFFVyC4XtYlK1kJrF87P255C0dv9P8jkzrslMmWE69Q8qq
O6X3ovWk8hXIFk/KsvGWzxzzt6blWPHByqOee78a8Rj9r7SFvZGGnOSH9+BziZCNv1l5ivWq5nKs
HOOTh75KuR84LDtyHbhWmgtOULZ45MUjl9QyHv3zVhXJUNjI6ATdkufLvADsb1XbS1H96fN9E47u
eAIQvXxQJYsf8f8MVBHKQ3NJuJhVPN6Wwzre0uHxqKCn7U7PeC4cGY8GnEwd9pB4Tp06tWPHjqd9
nnvuOfX/yclJJSElEpUMXbt2rUrxZF306Y+HyXssVSsPl7TVZFL5eLJT7cKVyxjvkBILbroz0iW4
uCXZ4hGtNY/FJvhoGf8jZ1UVP9Ew0SfqB2JrpHlakxOqHh+dmhRn0189lU0mqxtDa9GmjMupgoKb
PoPGceDWd0cT8YjlneLRrqVKJndEjTQ/NHj/T+XutreqvaOo/vTJHl+viQnTGA6QpjFWdIrHkrgY
u3aU2hJp9Zp4FhcXx8fHfxPx5JNPPvPMM6Ojo+rBkydP5p9fUJh4XLlFuoCb9ERGL5z8GZbmxDYt
u4jfq/EutAlm1jS/xA/BU0k5yDgu2S80K7VlvqT6pEFRgnOO+pYrHvHRwbCCrZFm5ygyG3Wkwb/+
6vEGjW4o9qul1/bHGKofb5O5l+PA7QOBzUptyznEI9ycWXIoXDy2DxP6BWB/qzrGRCs/fXq3rqUX
YanNUjELS23JM15lrIl4kuWT2t3GDPHI+luPimd1dXVqauo3gmeffVZZRyU9Kysr+ecXFFZqc/V6
6bdEDvHIgkbyrjY+K9nF08WJp/H7Kp1ptSWeph+TrVoqu95ipGJhszP7Hfs4UCiSyDRGxcbyEb6p
eKrofN0Xv+PA7blIIeK5Fdm6opRXvJfDPerNTi4A+1vVkZZVffpmxzemkLW1YsST+MzXSZRFOTMe
v1VRM3o341Fqeffdd5966qlYPC+++OLs7GyrEw0KmtXmnt/VrNSWIR6tm8tbahvt4oTm6P2fUWpL
f+p3FCisEm1u1vKri6kaoP+5+1hGpcXVBQdncG4ybLDaztQZ1X7LcJ2l1GaKJz3foSrcl+gt+4FY
PpAZRcu84vGrlMdWqikp6232/39sUozF5ii1OV+KKk9fusDlKSQc8kmJJx4NSokn2Y5FPGI7yXhS
lnj0VvWueBQfffSRevyJJ55Q+nn66aeVeE6dOtUl8di+Vxh9jyc9uUCONNp6YfHFCG0kwBixjN5s
xuQCOddrstTPUKpJ4g1mNkmfXCAnqomPjZtd4jGOV0wu0Kr8QXk9PswK1JsefDK/2WNtpO3TSbii
NqPJMSid6n8t4okKnuWPscujMGZPWA5cP5BwRF0+qE1RaSYePS3w8sXR0YpyBU2WwdBmfCVkTy6I
rknrS1Ht6Tts+e5OIpVgcsGIZS5AMLlgxDb+b4pHS3E0Y/lpjXV4ScgvqMv1hHist8w5e/bs7t27
lXX27t179OjRl156aefOnR9//HFXvscjrkXLdFLH3EqXeOR2Un1x+rYIWpettaHsAR55XJvTiZ1t
RricThpMJXcnCtbXQc4wTnKF6u4WYZv1oE9LszdSvCb61HCHaaTSmgz4yZcu6sRLfgU2Nz25qaki
5vLJmJA6lnyltnj7yQLVzQSzZGmine759MY1mX4pqj19I7a7FcQJSvCf2SMj5nhM6I/ZeGK0EIN5
5wLTGck86fFZLbOJVvTddiHe6cuzPZHxGDcJPXjwYHAb0OXlZfX4tm3bzpw5c/HixU8++WTPnj2B
eyqeXAAA3cE6+xHaJVVqs2RFA0A7P4sQ35vg66+//vDDD9955x2lmZs3b6o/lXvGx8cPHTq0urqK
eAAGnS4NayGe9SAeF0o/KtE5f/78jRs3gkeUgT799NPTp0+T8QAMNmF9FesgnorFUwiIBwBgXbGB
lwAAABAPAAAgHgAAAMQDAACIBwAAAPEAAJTDf5+7PvT018TDr3yLeAAAEE+viqcOAADtcujEOayj
4u9fupD/RdtwHwAAoEIQDwAAIB4AAEA8AAAAiAcAABAPAAAA4gEAAMQDAACIBwAAAPEAAADiAQAA
QDwAAIB4AAAAihHP3/3zHYIgCIJoLxAPQRAEgXgIgiAIxIN4CIIgCMRDEARBIB6CIAiCQDwEQRAE
4iEIgiAQD+IhCIIgBlI8D+3xgpeeIAgC8VQknkN/uqeCl54gCALxVCGePcfv/tf/3VPx4n/e5dUn
CIJAPOWK59HpHwPrBKH+rOAI//V/7sZ7rPTF3fWD/HPT2LUHRy/+dNv5n77wZy+2fvngb1ce2HHF
WKzAUBs39zh60duju5EEQRADJZ6H/6BZJwj14KCKR3X0m3be9pSz8/aD25bC3j8dW7984HffFrtr
JbnEN+nYdl4tEFhHNWzTru95GxAEMYDieWjPnZn5e2nxqAdLmmjw5L/9+MJ/3FXx77PJ7oJH1FNl
v6xe1+938UoqTgGIUNlPUcnHA6N/ybPHB7Y3Ah0Wrj2CIIieEM/Y0bsq81BxdCHRQPCIeqqMA7Mm
WJWlWVm9/9bz1gTIc0/H+31w+5ply9uWMlKuQvbrPO/zf1u87Ip7k7wDCQLxVDC5oMrClzGkVOXA
krKLraTWkGmNN+rz2xUtC+ks+QjTLOEbbURn1w/qT0vDXvhzNSM9voSQDUEQAy0eOYkuCPVnBTvd
tPN2unMPxnua5EZbv+zEAVIqD45etG9q1w9qL2bZzZhxgHgIghgk8Tz8hx9/9ccwqjnCQ38KrVPZ
l4dUZmOtd7mkIvOetpMeLd3Zer6lGqB6sDviOXVPPVKLynEfnQoev/uRqMjVZuRG7tZWkqei5QmC
QDw9Fg/tuXN04Z6Kym6X4BpQcfXvm3Z93/mIizTKprEbrY08qUyra+L52+LKvbH4kZl78+qRxbty
iChyjy+keGF/3Wz3TC7qo0pyRwRBDJ54fn7wRxn/8C9h/ELEP74axi9FtJ0JZezx0ekfH5sufo9N
LZKM7W9f82Ywu8to0lVt2i5OmzIssusHb2Bp+5oxtpThqgrEI+Xhq0Iu46c4vjB0CVkXTofMkKjy
EcSgi8c1oyxPtHck1e/RnlXsuBIkLk1l45qN1uYAT+s5k5LNA79rBBJSDeiWeIRL/Jxm8a5trcRA
7tVtEaRQ1OUIAvEMsHja+z6mTEEqE48xIaJXxGOfeK2N7iw6B4Ec+13k/kwEsQ7E86s/auWsX0Y1
rl/o1bagGmZUydo7kow9PhaV2ordY0nDQp1uoZIBm/LEMz9/11k0wx8EgXj6ZXLBzHyJt0godvp1
IZMLqpkeXYJ4/GEbvZ4WP5JenfnZBIF48k6njjOSQZ1O3XKyMnqx2OnUGVO3e1w8xqw2ffaBPqvN
X9KRHhEEgXhEdPcLpD34WwwlfoG0H8XD93gIAvFwy5wSK2xjN4xv/Hj31Olog9eMu1D3Zs2NIAjE
U+lNQuU9qgf4JqEtZDmF5ijWm4QqA3GhEwSx7sQT3D4g7YDybigQ/yzC4f/tws8i5J88ndzUoMyf
ReBCJwhiPZba1tsPweUVT0k/BKffCZQLnSCIdTrGs75++rqZeLyfvlbKKfunr30DcaETBLF+JxfE
08yq+YUCgiAIYr2LJ/hiTc9+pYYgCIIYQPE8tOdOz95EgCAIghhA8RAEQRCIB/EQBEEQiIcgCIJA
PIiHIAiCQDwEQRAE4iEIgiAIxEMQBEEgHoIgCALxIB6CIAgC8RAEQRDrXTz3AQAA2gLxAAAA4gEA
AMSDeAAAAPEAAADiAQAAGADxLC0tfeGzlEm8DOfVxdqJA2MHjq8Ff8xPj6k/LnW/JQtvjCWt6qVX
6/iBMY+cbfNeT4/peS40gIEQz7ncIB43C9OyW+ymeLSW9KZ4bK2KVDTmFlJXdQ6AeIoXT3bGg3hy
fCSfXuiFLlJvSU+KR5e0Kx+yNNt7/MCJNS43gP4WT6PRCKSSvViwjFqY8+rqKLUOsWviMVvSn+Jx
vYCIBwDx5OBuM77//vu2PtEvTMdVGa0n0io2Rifl9cI6yQLz08mjbyyk9phg6TEvef2h1kumGqmv
pTUyeiroVReCp1TDvKEaozEZjXS0xDtktWSy4vRCygFJeetS3pbIV7L1cZec4pleIOMBGEjxlF1q
m52d/adMlpeXWxeP6O/8P8WQhuh2zadEt3vJr+VEXZjfscbdnG+FuJP1l8zuJS1ZRdjIsDH+9uOG
+X19vH1/Sb8loY3UvvzZAQcOhMKQG3E00t2S0BDhg3oJy/IiBEea3ZKWX5/WxeMtYBMM4gEYLPE0
MulkjGfv3r0u67z11lstb07XiasLTvVfxmLyz1Q/KD9uNy+a2bpRYy3RO+v+kLZIrJCIKtlOZiPd
LTFUJPeeslT8mmS2JLXfMKlqOWe11wmzJhfYXj0AoNRm4auvvrJa57HHHrt9+3Zb4klVk/RP8dZi
WjrjCfvo1CrpulNGTcneFbrFY+mmw444cWGyzXg7TRrpbIlbPJbsIZqHndWSsOyWakgLs6JzWErP
EdPlxN6cIw6AeHpGPIo333wzLZ4PPvignW1ZxRP0ZUGXlPRrWsXG6DGTPjdfsSge1UgPKVmKP+WI
p+lMsHRLyhFPZzlHrpkXeWuJAIB47Ny6devRRx+V1tmxY0dB3ZboQM2npHgyxhVcIwoO/ciOLz2t
oMNSm0s8TRvpaEn7pTZXSwqYsJdjcoHdo4zxACCeVjh58qQUz9mzZwsRj7OAFs3XkuJx9VliXL21
j+HO4Q23eMy0LBmyyhZPk0a6WpIhHjGvwahuZbckqD12kvQ0F492WhEPAOJpm+3btwfWOXjwYGeF
GufogqynTc/r/ZSx4li6Rx5LT1Z2Pd6kA80Qj5CiZTDJLZ7MxjhbkiWe+8bQkWXKnKMla8aIUwGz
2oxBLPs4EOIBQDytftBdWAjEc+lSB5Wa9ko96TESfTJxG/TODKt+m+uVp9R2H/EAIJ5i7lzwyiuv
HDlypKNNtCcecxK27ZF+7QT7rjt23RGn9U8PANCP4qn4JqGrq6t37nR2jO0ObqfmAXO7yS4nPW3c
nZp0BwDxAAAA4mldPAHZdy6IF+O8AgAgHgAAAMQDAACIBwAAEA/iAQAAxAMAAIgHAAAA8QAAAOIB
AADEg3gAAADxAAAA4kE8AACAeAAAAPEAAAAgHgAAGFTxpH8aR/6gDi86AADi6XXxqNW/8FnKJF6G
89pvrL19YMOGsQ27rb8YPb/b+VRvc/qNDRsOvM3PkQL0q3j4BdL+xet/3zid2SMjHgDE05l4ZBbi
eqQ98WRnPIinchamxxTTC52KJxPEA4B4qp9c0Gg08mRLwTJqYc4r4kE8AIgH8RRCfWJoaGLe+3fI
+8/9+n7vP8Mz8SE3aluGErbU5GsRLCwRK5bc/zrFExbZ/Hjk7Uu29S+9/Uir4vFXCTdrdP2+xuIw
NyuefeSEXE+201jL+ZQmnqhJ/WhQgPUoHkptUjzKF7XLnkWGtwwrczRmhpWD6rFadNno1gkXu3+5
NlyVdfJmPF6PX5B49E2le3/XptZOPCLFsHZid7SR07vHxCH4coq0lPGU2HVgHf1FAEA8RYqnIXA9
0p54GpmsF/Hsr0uRCPH46c7+uvWc6E9lLFmOeMZS0Zp4HE/Z9KHyDy1Zkas305ue5Ugh7T5tE2fG
U8n/dRsBQH/MaqPUJsUTZCpxciMznvvzE9YimzXjmaiq5lNExpNfPH4Kkoooj0nKYrYim30v6fb7
C3u+yXhKMy65DgDiGVjxCK8Ew0BJjjMzXMLoTnGTCwoVT9P0IlZCsmRJ4iHjAUA860I8lrQmmJVQ
eHsqEU9r+DlNvgwj5/BPp6W21OgRAOIpXjy3Ba5HEE/p4vHKbt40BGPF6ulQPEGv3ULGkDfDMBXl
p0FJG5LJBcbUADm9O+OptIRwD0B54ikcxJNPPPUJfbJ0ZJ3YQzqO+W/Visc2JKMv3LJ47ptzpuPe
P9iUa0fGAvoeZTsNQTqf0v0aDi9RcwNAPOuD9GyCamdUAwAgnnWGn+5o4kk/AgDQv+LhJqE9iDGr
zSzEAQAgHsQDAIB4uimegOw7F3RyA2wAAOhj8RT+swgAAIB4AAAAEA8AACAeAAAAxAMAAIgHAAAQ
D+IBAADEAwAAiAcAAADxAAAA4gEAAEA8AACAeADyXr5jG6z/z79WGau0sf221wJAPAB9IJ4y3JNu
yYb0T3r70cnhuLaZfxcAAyuewn8WAaBt8eTsrDvssmPZpFuSsdmMJV3i4aQD4qlIPGr1L3yWMomX
4bz61CeGhoZn7D8EXt8/NLSl1u8/Eu7qqTvvoFsVlUs8+VOcPE+1vTUAxNOOePgFUsSTUzwl9b95
On3j3zwZVUuCyVYa4oF1LZ7CfwguFk92xoN4+lk8C9NjiumFFnMRa9ffahJTknjadkwe8RSe5AH0
t3gKp9Fo5MmWgmXUwpzXARZPRs/bXiWqbPE0TcXy5y6uJbEOIB7EU4VRYibmU0YRaOKZn9Cei8TT
mBn2/59sVlvrcm04tYp1g3pLtEYO7a+Xcvm28pG/VfG0NBOhQPEUO/ADgHjahFKbtHBty0Tci3vO
GBquXY6fkm7QMh59SS3j8Z8S5vB0Ei0p/2/kSb6QDO2JRpYlmyrF0+qf7Ykn/4GQ8QDiyUpQYlyP
tCeeRibrcYxH9v66JHTxmGW3lHjEisk2PX+ksh+7k9I5mavK10XxtDTGU6V4ciqEMR5APE1KXgXO
aqPUdt9d4IozlahiZuv9pTDs4pmoO/yRJspy/LTGVu6TWVSpeU+pXyCtWDytLsCsNkA8iKdS6yTJ
hMh4ShJP08QlHlWyLBk7svlEhgImF+SpSuU3ViHiafU7PTkzHgZ7APEgnuowJWGIJ/WUEI/MS7TR
ILd4WhiqyZoml1WUa1M8runUbYgnZz9udPpNxZNfJ/c7GOMBQDwhtwWuRxBPBxlP1IlH881Co6RM
I7IQaZqoRNZcPKkEy32KMhSVtf1Wr1fdN/ld0pKWmq6SPcifp3mtNgbxAOJhOnU3EROmlYG8aliS
yiTDP6qjN6YGJAM2anlZl2sihoxJ2K4J03IGdtiY4i7ZTNlkfLc0OwXpZAAmI/tpKpjsydwu73Kf
UEA8iAe6dPlW+AMELf3sQtOUJacesAggHsQDAAA9KR5uEgoAAIgHAAAQTyviCci+c0EnN8AGAIA+
Fk/hP4sAAACIBwAAAPEAAADiAQAAQDwAAIB4AAAA8SAeAABAPAAAgHgAAAAQDwAAIB4AAADEAwAA
iAcAABAP4gEAAMQDAACIpySWlpa+8FnKJF6G81oU9f1DQ1tq7f6WeH1iSDFcu1xpmxszw0NDE3VO
HgDi6VA8/ALpOheP1xKD/fXeFs9nN372k6s/G7m5Zjz+7c1fq8eD+P13+nN/rY1ET/3k6q9rf+US
BMTTdfFkZzyIp8fEUwKXa8PNTFameBamxxTTC82W++6AJ5WbnkUM8Xg2ulb7Vi72nWad+E/fT7gH
EE+3xNNoNPL8bHawjFqY84p4uigezx8HPotEoonHe0RzifSQZ5rYSSkPASAexNN3/kgVprwOOpbK
/IS1hBWumDwbdfqeAIYm5rWdTCTr1sXmLA6QdbNoI94q+ga9Nta2DA3PNJqLx2/PUOZOqyctHi/F
0cTjpzW+pfT/WxcGQDwVQqmtjMQltpGeInh9feye0BDRumI7/mJym56cTCXYkg9t+0Jgnng0x1ht
ZBWP78V4sd6ZXJBDPP4jsWzqv4+HdoLBnhvMkQDE03XxNDJBPM6UMU5uko47TiZSnbvXj4cdt2ks
2e/rprG6zeIAsXHdf0lyY2tthnh0k/W2eAK1JDoJTCOynGhKwk8osgHi6bJ4KLV1StTd+51y0L97
/bXnG7NIpZXUUjqRlpI5irVQZnFA0AATfxfSQGEDvLbpCrGIx9x1T4snlk0QBz5LZzzBMI8/70Ab
8gFAPIinr4j66/r+4dp8bdjr6KP+2jJao6cjZsajF7WCZ1N5TKZ47FaItlaf2FKrzwx7Sktvtsvi
yTurLUs8GnJCgTbhLd/qAIgH8fQuqndW/bXXpze8lMLXT9iDWwdXHOIxBnISn9m34Ci1OaalqafU
vuYnvE1d9uxYl9MfMsUj9+6PS/WJeLwUJyqprdWuGYM63rOIBxAP4ulX/BGULcNiEGU4rmIF5S9r
0qOLR85bE5udqU04XOKcXGAVgy8V1chkCGrLsDkZzza5QJommi/Xw6U27VlpmuCLpXyPBxAP4hmY
lGe/ORlaJhPm0EvU3RuPW9KaYKa1RQ+pUaNk3cA99hnVcavEcJQ0ivXOBckG7QW6ivFzl6tGhAM5
8dwBq1TkTQ2wDiAexAMAAIgH8QAAIJ5S4CahAZZyUzIj+efup2rrUsXy7gkmzz3rfm4/X9oEQDyI
BwAA8VQvnoDsOxfEi3FeAQAQDwAAAOIBAADEAwAAiAfxAAAA4gEAAMQDAACAeAAAAPEAAADiQTwA
ANDT4iEIgiCI9uL/AV5teiMfZp4WAAAAAElFTkSuQmCC'/>

### 合并后 - 生命周期函数的执行顺序

<img src='data:img/jpg;base64,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
6jwf6rgAAAAASUVORK5CYII='/>

## 合并响应式的数据对象

### 按需导入 completeAssign 辅助方法

```js
import { completeAssign } from '@escook/utils-miniprogram'
```

### 配合 `mobx-miniprogram` 使用

`mobx-miniprogram` 是通过 `getter` 和 `setter` 来实现响应式数据绑定的。

当使用 ES6 的`展开运算符(...)`合并多个 Store 模块时，会丢失对应的 `getter` 和 `setter`，从而导致 `mobx-miniprogram` 的响应式数据绑定失效！！！

此时，可以使用 `@escook/utils-miniprogram` 提供的辅助函数 `completeAssign`，来进行多个响应式对象的合并操作。

最终合并的结果，会保留每个响应式对象中的 `getter` 和 `setter` 描述符，从而保证 `mobx-miniprogram` 的响应式数据绑定能够正常工作。

示例用法如下：

```js
import { observable } from 'mobx-miniprogram'

// 导入需要的 Store 模块
import common from './common'
import cart from './cart'
import user from './user'

// 把分散的 Store 模块合并为一个
const combineModule = completeAssign({}, common, cart, user)
// 创建 Store 的实例对象
export const store = observable(combineModule)
```

## 开源协议

![MIT](https://img.shields.io/badge/License-MIT-blue)

**enjoy!**
