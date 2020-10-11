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

![](https://github.com/liulongbin1314/utils-miniprogram/blob/master/assets/images/data.png)

### 合并后 - 生命周期函数的执行顺序

![](https://github.com/liulongbin1314/utils-miniprogram/blob/master/assets/images/console.png)

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
