什么是框架？你知道哪些能够用于移动web开发的前端框架？针对一种框架介绍其在移动web应用开发上的优缺点（性能、适配性、易用性等角度）

[toc]

## 框架

框架是对基础的代码进行了封装并提供相应的API，开发者在使用框架时直接调用封装好的api可以省去很多代码编写，从而提高工作效率和开发速度。

##　移动web开发的前端框架

Vue,Bootstrap,Nodejs,AngularJS,ZeptoJS,Jquery

## Vue

Vue 是一套用于构建用户界面的**渐进式框架**。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与[现代化的工具链](https://vuejs.bootcss.com/guide/single-file-components.html)以及各种[支持类库](https://github.com/vuejs/awesome-vue#libraries--plugins)结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

优点：
1、数据驱动视图，对真实 dom 进行抽象出 virtual dom（本质就是一个 js 对象）， 并配合 diff 算法、响应式和观察者、异步队列等手段以最小代价更新 dom，渲染 页面
2、组件化，组件用单文件的形式进行代码的组织编写，使得我们可以在一个文 件里编写 html\css（scoped 属性配置 css 隔离）\js 并且配合 Vue-loader 之后，支 持更强大的预处理器等功能
3、强大且丰富的 API 提供一系列的 api 能满足业务开发中各类需求
4、由于采用虚拟 dom，让 Vue ssr 先天就足
5、生命周期钩子函数，选项式的代码组织方式，写熟了还是蛮顺畅的，但仍然 有优化空间（Vue3 composition-api）
6、生态好，社区活跃

缺点：
1、由于底层基于 Object.defineProperty 实现响应式，而这个 api 本身不支持 IE8 及以下浏览器
2、csr 的先天不足，首屏性能问题（白屏）