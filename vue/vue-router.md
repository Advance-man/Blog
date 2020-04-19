##### vue-router

vue的官方的路由管理器；于vue的核心深度集成；

功能包含：

1.嵌套的路由以及视图表；

2.模块化的基于组件的路由配置；

3.路由参数，查询，通配符；

4.基于vue的过渡系统的视图过渡效果；

5.细粒度的导航控制；

6.带有自动激活的class 连接；

7.html5历史模式或hash模式，在ie9中自动降级；

8.自定义的滚动条行为；



初始化：

先定义一个路由,里面定义了path。component扽参数；

在使用定义的路由，创建路由实例，

再将创建的路由实例挂载到vue实例上；

在组件内部可以通过使用 this.$router访问路由实例；使用

this.$route访问当前路由；



动态路由匹配：

/user/：id 使用一种模式来匹配所有的路由的模式；

其中冒号后面的参数会被设置到this.$route.params;可以在每个组件中使用

$route.query,$route.hash获取请求查询参数以及哈希值；





响应路由参数的变化：当路由参数变化实际的组件没有变化时，原来的组件会被复用；

组件的生命周期钩子不会再被调用；这种情况下需要需要对参数变化作出响应有两个版本：

1.监听$route的变化，当参数变化之后重新获取数据，修改页面中的数据对象；

2.使用导航守护：beforeRouteUpdate



###### 捕获所有的路由或者404

```js
{
  // 会匹配所有路径
  path: '*'
}
{
  // 会匹配以 `/user-` 开头的任意路径
  path: '/user-*'
}
```

需要注意通配符的位置，一般将其放在路由对象的最后一个；

当使用一个*通配符*时，`$route.params` 内会自动添加一个名为 `pathMatch` 参数。它包含了 URL 通过*通配符*被匹配的部分：

```js
// 给出一个路由 { path: '/user-*' }
this.$router.push('/user-admin')
this.$route.params.pathMatch // 'admin'
// 给出一个路由 { path: '*' }
this.$router.push('/non-existing')
this.$route.params.pathMatch // '/non-existing'
```

## 匹配优先级

有时候，同一个路径可以匹配多个路由，此时，匹配的优先级就按照路由的定义顺序：谁先定义的，谁的优先级就最高。所以通配符需要放在最后，保证正常的路由可以优先匹配到；

#  



# 嵌套路由

创建的 app：

```html
<div id="app">
  <router-view></router-view>
</div>
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}

const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User }
  ]
})
```

`User` 组件的模板添加一个 ``：

```js
const User = {
  template: `
    <div class="user">
      <h2>User {{ $route.params.id }}</h2>
      <router-view></router-view>
    </div>
  `
}
```

要在嵌套的出口中渲染组件，需要在 `VueRouter` 的参数中使用 `children` 配置：

```js
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User,
      children: [
        {
          // 当 /user/:id/profile 匹配成功，
          // UserProfile 会被渲染在 User 的 <router-view> 中
          path: 'profile',
          component: UserProfile
        },
        {
          // 当 /user/:id/posts 匹配成功
          // UserPosts 会被渲染在 User 的 <router-view> 中
          path: 'posts',
          component: UserPosts
        }
      ]
    }
  ]
})
```

**要注意，以 `/` 开头的嵌套路径会被当作根路径。 这让你充分的使用嵌套组件而无须设置嵌套的路径。**





# 导航守卫

**植入路由导航过程中：全局的, 单个路由独享的, 或者组件级的。**

**参数或查询的改变并不会触发进入/离开的导航守卫**你可以通过[观察 `$route` 对象](https://router.vuejs.org/zh/guide/essentials/dynamic-matching.html#响应路由参数的变化)来应对这些变化，或使用 `beforeRouteUpdate` 的组件内守卫

