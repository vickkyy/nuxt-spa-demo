# Nuxt在SPA模式下的鉴权处理(2)

> 上次提到了使用手写了一个简单的sessionStorage来存储Vuex状态中的数据, 防止刷新页面的丢失, 这次主要介绍一个很完美的插件了, 来处理更复杂的需求场景

那么这次介绍的是[`vuex-persistedstate`](https://github.com/robinvdvleuten/vuex-persistedstate)插件, Nuxt项目中使用这个插件也是折腾了小阵子.

> **为何仅推荐在`SPA`模式下使用该插件**
>
> 因为起初我对Nuxt的SSR不太熟悉, 使用了非`SPA`模式下的渲染, 该插件总是存在一些问题, 其实主要问题是我自己带来的. 当时没有理解到服务端渲染, 一直以客户端渲染的方式去理解文档, 所以一直处于困惑状态... 因此,
> 
> 非`SPA`模式下, 所有内容都是在服务端(Node)处理, 自然无法正常使用`window`对象, 也就不能很好的运用该插件了!

## vuex-persistedstate在Nuxt下的使用

* 安装

  ```bash
  npm install vuex-persistedstate
  ```

* 使用

> 这里我没有完全按照官方文档中Nuxt下的使用方法.

我直接在`store`内的`index.js`中进行相关设置, 对比旧的代码在分支[nuxt-auth-a](https://github.com/whidy/nuxt-spa-demo/blob/nuxt-auth-a/store/index.js)

```javascript
import createPersistedState from 'vuex-persistedstate'
export const state = () => ({
  counter: 0
})

export const mutations = {
  increment: state => {
    state.counter++
  },
  decrement: state => {
    state.counter--
  }
}

export const plugins = [
  createPersistedState({ storage: window.sessionStorage })
]
```

而页面部分无需改动, 简单测试一下, 非常有效, 代码看起来清爽多了.

当然在插件的这个`createPersistedState`中, 可以配置的参数很多, 你也可以直接改成`localStorage`来永久存储.

> 上面这个示例来自[Nuxt官方example](https://github.com/nuxt/nuxt.js/tree/3b2ed038da93d2bef8de8848b30d4071091e58cd/examples/vuex-persistedstate)

