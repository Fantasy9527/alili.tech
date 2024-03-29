---
title: 手摸手，带你用vue撸后台 系列二(登录权限篇)
hidden: true
categories: [reprint]
slug: 469e6b78
date: 2018-10-23 00:00:00
---

{{< raw >}}

                    
<p>完整项目地址：<a href="https://github.com/PanJiaChen/vue-element-admin" rel="nofollow noreferrer" target="_blank">vue-element-admin</a>  </p>
<p>系列文章：</p>
<ul>
<li><a href="https://juejin.im/post/59097cd7a22b9d0065fb61d2" rel="nofollow noreferrer" target="_blank">手摸手，带你用vue撸后台 系列一（基础篇）</a></li>
<li><a href="https://juejin.im/post/591aa14f570c35006961acac" rel="nofollow noreferrer" target="_blank">手摸手，带你用vue撸后台 系列二(登录权限篇)</a></li>
<li><a href="https://juejin.im/post/593121aa0ce4630057f70d35" rel="nofollow noreferrer" target="_blank">手摸手，带你用vue撸后台 系列三 (实战篇)</a></li>
<li><a href="https://juejin.im/post/595b4d776fb9a06bbe7dba56" rel="nofollow noreferrer" target="_blank">手摸手，带你用vue撸后台 系列四(vueAdmin 一个极简的后台基础模板)</a></li>
<li><a href="https://segmentfault.com/a/1190000009090836">手摸手，带你封装一个vue component</a></li>
<li><a href="https://juejin.im/post/59bb864b5188257e7a427c09" rel="nofollow noreferrer" target="_blank">手摸手，带你优雅的使用 icon</a></li>
<li><a href="https://juejin.im/post/5b56909a518825195f499806" rel="nofollow noreferrer" target="_blank">手摸手，带你用合理的姿势使用webpack4（上）</a></li>
<li><a href="https://juejin.im/post/5b5d6d6f6fb9a04fea58aabc" rel="nofollow noreferrer" target="_blank">手摸手，带你用合理的姿势使用webpack4（下）</a></li>
</ul>
<h2 id="articleHeader0">前言</h2>
<blockquote>拖更有点严重，过了半个月才写了第二篇教程。无奈自己是一个业务猿，每天被我司的产品虐的死去活来，之前又病了一下休息了几天，大家见谅。</blockquote>
<p>进入正题，做后台项目区别于做其它的项目，权限验证与安全性是非常重要的，可以说是一个后台项目一开始就必须考虑和搭建的基础核心功能。我们所要做到的是：不同的权限对应着不同的路由，同时侧边栏也需根据不同的权限，异步生成。这里先简单说一下，我实现登录和权限验证的思路。</p>
<ul>
<li>登录：当用户填写完账号和密码后向服务端验证是否正确，验证通过之后，服务端会返回一个<strong>token</strong>，拿到token之后（我会将这个token存贮到cookie中，保证刷新页面后能记住用户登录状态），前端会根据token再去拉取一个 <strong>user_info</strong> 的接口来获取用户的详细信息（如用户权限，用户名等等信息）。</li>
<li>权限验证：通过token获取用户对应的 <strong>role</strong>，动态根据用户的 role 算出其对应有权限的路由，通过 <strong>router.addRoutes</strong> 动态挂载这些路由。</li>
</ul>
<p>上述所有的数据和操作都是通过vuex全局管理控制的。(<strong>补充说明</strong>：刷新页面后 vuex的内容也会丢失，所以需要重复上述的那些操作)接下来，我们一起手摸手一步一步实现这个系统。</p>
<h2 id="articleHeader1">登录篇</h2>
<blockquote>首先我们不管什么权限，来实现最基础的登录功能。</blockquote>
<p>随便找一个空白页面撸上两个input的框，一个是登录账号，一个是登录密码。再放置一个登录按钮。我们将登录按钮上绑上click事件，点击登录之后向服务端提交账号和密码进行验证。<br>这就是一个最简单的登录页面。如果你觉得还要写的更加完美点，你可以在向服务端提交之前对账号和密码做一次简单的校验。<a href="https://github.com/PanJiaChen/vue-element-admin/blob/master/src/views/login/index.vue" rel="nofollow noreferrer" target="_blank">详细代码</a></p>
<p><span class="img-wrap"><img src="https://static.alili.tech/img/remote/1460000016613452?w=478&amp;h=285" del-src="https://static.alili.tech/v-5bbf1b3b/global/img/squares.svg" alt="" title="" style="cursor: pointer;"></span></p>
<p><strong>click事件触发登录操作:</strong></p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="this.$store.dispatch('LoginByUsername', this.loginForm).then(() => {
  this.$router.push({ path: '/' }); //登录成功之后重定向到首页
}).catch(err => {
  this.$message.error(err); //登录失败提示错误
});
" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="javascript hljs"><code class="js"><span class="hljs-keyword">this</span>.$store.dispatch(<span class="hljs-string">'LoginByUsername'</span>, <span class="hljs-keyword">this</span>.loginForm).then(<span class="hljs-function"><span class="hljs-params">()</span> =&gt;</span> {
  <span class="hljs-keyword">this</span>.$router.push({ <span class="hljs-attr">path</span>: <span class="hljs-string">'/'</span> }); <span class="hljs-comment">//登录成功之后重定向到首页</span>
}).catch(<span class="hljs-function"><span class="hljs-params">err</span> =&gt;</span> {
  <span class="hljs-keyword">this</span>.$message.error(err); <span class="hljs-comment">//登录失败提示错误</span>
});
</code></pre>
<p><strong>action:</strong></p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="LoginByUsername({ commit }, userInfo) {
  const username = userInfo.username.trim()
  return new Promise((resolve, reject) => {
    loginByUsername(username, userInfo.password).then(response => {
      const data = response.data
      Cookies.set('Token', response.data.token) //登录成功后将token存储在cookie之中
      commit('SET_TOKEN', data.token)
      resolve()
    }).catch(error => {
      reject(error)
    });
  });
}" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="javascript hljs"><code class="js">LoginByUsername({ commit }, userInfo) {
  <span class="hljs-keyword">const</span> username = userInfo.username.trim()
  <span class="hljs-keyword">return</span> <span class="hljs-keyword">new</span> <span class="hljs-built_in">Promise</span>(<span class="hljs-function">(<span class="hljs-params">resolve, reject</span>) =&gt;</span> {
    loginByUsername(username, userInfo.password).then(<span class="hljs-function"><span class="hljs-params">response</span> =&gt;</span> {
      <span class="hljs-keyword">const</span> data = response.data
      Cookies.set(<span class="hljs-string">'Token'</span>, response.data.token) <span class="hljs-comment">//登录成功后将token存储在cookie之中</span>
      commit(<span class="hljs-string">'SET_TOKEN'</span>, data.token)
      resolve()
    }).catch(<span class="hljs-function"><span class="hljs-params">error</span> =&gt;</span> {
      reject(error)
    });
  });
}</code></pre>
<p>登录成功后，服务端会返回一个 <strong>token</strong>（该token的是一个能唯一标示用户身份的一个key），之后我们将token存储在本地cookie之中，这样下次打开页面或者刷新页面的时候能记住用户的登录状态，不用再去登录页面重新登录了。</p>
<p>ps:为了保证安全性，我司现在后台所有token有效期(Expires/Max-Age)都是Session，就是当浏览器关闭了就丢失了。重新打开游览器都需要重新登录验证，后端也会在每周固定一个时间点重新刷新token，让后台用户全部重新登录一次，确保后台用户不会因为电脑遗失或者其它原因被人随意使用账号。</p>
<h3 id="articleHeader2">获取用户信息</h3>
<p>用户登录成功之后，我们会在全局钩子<code>router.beforeEach</code>中拦截路由，判断是否已获得token，在获得token之后我们就要去获取用户的基本信息了</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="//router.beforeEach
if (store.getters.roles.length === 0) { // 判断当前用户是否已拉取完user_info信息
  store.dispatch('GetInfo').then(res => { // 拉取user_info
    const roles = res.data.role;
    next();//resolve 钩子
  })
" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs javascript"><code><span class="hljs-comment">//router.beforeEach</span>
<span class="hljs-keyword">if</span> (store.getters.roles.length === <span class="hljs-number">0</span>) { <span class="hljs-comment">// 判断当前用户是否已拉取完user_info信息</span>
  store.dispatch(<span class="hljs-string">'GetInfo'</span>).then(<span class="hljs-function"><span class="hljs-params">res</span> =&gt;</span> { <span class="hljs-comment">// 拉取user_info</span>
    <span class="hljs-keyword">const</span> roles = res.data.role;
    next();<span class="hljs-comment">//resolve 钩子</span>
  })
</code></pre>
<p>就如前面所说的，我只在本地存储了一个用户的token，并没有存储别的用户信息（如用户权限，用户名，用户头像等）。有些人会问为什么不把一些其它的用户信息也存一下？主要出于如下的考虑：</p>
<p>假设我把用户权限和用户名也存在了本地，但我这时候用另一台电脑登录修改了自己的用户名，之后再用这台存有之前用户信息的电脑登录，它默认会去读取本地 cookie 中的名字，并不会去拉去新的用户信息。</p>
<p>所以现在的策略是：页面会先从 cookie 中查看是否存有 token，没有，就走一遍上一部分的流程重新登录，如果有token,就会把这个 token 返给后端去拉取user_info，保证用户信息是最新的。<br>当然如果是做了单点登录得功能的话，用户信息存储在本地也是可以的。当你一台电脑登录时，另一台会被提下线，所以总会重新登录获取最新的内容。</p>
<p>而且从代码层面我建议还是把 <code>login</code>和<code>get_user_info</code>两件事分开比较好，在这个后端全面微服务的年代，后端同学也想写优雅的代码~</p>
<hr>
<h2 id="articleHeader3">权限篇</h2>
<p>先说一说我权限控制的主体思路，前端会有一份路由表，它表示了每一个路由可访问的权限。当用户登录之后，通过 <strong>token</strong> 获取用户的 <strong>role</strong> ，动态根据用户的 <strong>role</strong> 算出其对应有权限的路由，再通过<code>router.addRoutes</code>动态挂载路由。但这些控制都只是页面级的，说白了前端再怎么做权限控制都不是绝对安全的，后端的权限验证是逃不掉的。</p>
<p>我司现在就是前端来控制页面级的权限，不同权限的用户显示不同的侧边栏和限制其所能进入的页面(也做了少许按钮级别的权限控制)，后端则会验证每一个涉及请求的操作，验证其是否有该操作的权限，每一个后台的请求不管是 get 还是 post 都会让前端在请求 <code>header</code>里面携带用户的 <strong>token</strong>，后端会根据该 <strong>token</strong> 来验证用户是否有权限执行该操作。若没有权限则抛出一个对应的状态码，前端检测到该状态码，做出相对应的操作。</p>
<h3 id="articleHeader4">权限 前端or后端 来控制？</h3>
<p>有很多人表示他们公司的路由表是于后端根据用户的权限动态生成的，我司不采取这种方式的原因如下：</p>
<ul>
<li>项目不断的迭代你会异常痛苦，前端新开发一个页面还要让后端配一下路由和权限，让我们想了曾经<strong>前后端不分离</strong>，被后端支配的那段恐怖时间了。</li>
<li>其次，就拿我司的业务来说，虽然后端的确也是有权限验证的，但它的验证其实是针对业务来划分的，比如超级编辑可以发布文章，而实习编辑只能编辑文章不能发布，但对于前端来说不管是超级编辑还是实习编辑都是有权限进入文章编辑页面的。所以前端和后端权限的划分是不太一致。</li>
<li>还有一点是就vue2.2.0之前异步挂载路由是很麻烦的一件事！不过好在官方也出了新的api，虽然本意是来解决ssr的痛点的。。。</li>
</ul>
<h3 id="articleHeader5">addRoutes</h3>
<p>在之前通过后端动态返回前端路由一直很难做的，因为vue-router必须是要vue在实例化之前就挂载上去的，不太方便动态改变。<strong>不过好在vue2.2.0以后新增了router.addRoutes</strong></p>
<blockquote>Dynamically add more routes to the router. The argument must be an Array using the same route config format with the routes constructor option.</blockquote>
<p>有了这个我们就可相对方便的做权限控制了。(楼主之前在权限控制也走了不少歪路，可以在项目的commit记录中看到，重构了很多次，最早没用addRoute整个权限控制代码里都是各种if/else的逻辑判断，代码相当的耦合和复杂)</p>
<hr>
<h2 id="articleHeader6">具体实现</h2>
<ol>
<li>创建vue实例的时候将vue-router挂载，但这个时候vue-router挂载一些登录或者不用权限的公用的页面。</li>
<li>当用户登录后，获取用role，将role和路由表每个页面的需要的权限作比较，生成最终用户可访问的路由表。</li>
<li>调用router.addRoutes(store.getters.addRouters)添加用户可访问的路由。</li>
<li>使用vuex管理路由表，根据vuex中可访问的路由渲染侧边栏组件。</li>
</ol>
<h3 id="articleHeader7">router.js</h3>
<p>首先我们实现router.js路由表，这里就拿前端控制路由来举例(后端存储的也差不多，稍微改造一下就好了)</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="// router.js
import Vue from 'vue';
import Router from 'vue-router';

import Login from '../views/login/';
const dashboard = resolve => require(['../views/dashboard/index'], resolve);
//使用了vue-routerd的[Lazy Loading Routes
](https://router.vuejs.org/en/advanced/lazy-loading.html)

//所有权限通用路由表 
//如首页和登录页和一些不用权限的公用页面
export const constantRouterMap = [
  { path: '/login', component: Login },
  {
    path: '/',
    component: Layout,
    redirect: '/dashboard',
    name: '首页',
    children: [{ path: 'dashboard', component: dashboard }]
  },
]

//实例化vue的时候只挂载constantRouter
export default new Router({
  routes: constantRouterMap
});

//异步挂载的路由
//动态需要根据权限加载的路由表 
export const asyncRouterMap = [
  {
    path: '/permission',
    component: Layout,
    name: '权限测试',
    meta: { role: ['admin','super_editor'] }, //页面需要的权限
    children: [
    { 
      path: 'index',
      component: Permission,
      name: '权限测试页',
      meta: { role: ['admin','super_editor'] }  //页面需要的权限
    }]
  },
  { path: '*', redirect: '/404', hidden: true }
];
" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs javascript"><code><span class="hljs-comment">// router.js</span>
<span class="hljs-keyword">import</span> Vue <span class="hljs-keyword">from</span> <span class="hljs-string">'vue'</span>;
<span class="hljs-keyword">import</span> Router <span class="hljs-keyword">from</span> <span class="hljs-string">'vue-router'</span>;

<span class="hljs-keyword">import</span> Login <span class="hljs-keyword">from</span> <span class="hljs-string">'../views/login/'</span>;
<span class="hljs-keyword">const</span> dashboard = <span class="hljs-function"><span class="hljs-params">resolve</span> =&gt;</span> <span class="hljs-built_in">require</span>([<span class="hljs-string">'../views/dashboard/index'</span>], resolve);
<span class="hljs-comment">//使用了vue-routerd的[Lazy Loading Routes</span>
](https:<span class="hljs-comment">//router.vuejs.org/en/advanced/lazy-loading.html)</span>

<span class="hljs-comment">//所有权限通用路由表 </span>
<span class="hljs-comment">//如首页和登录页和一些不用权限的公用页面</span>
<span class="hljs-keyword">export</span> <span class="hljs-keyword">const</span> constantRouterMap = [
  { <span class="hljs-attr">path</span>: <span class="hljs-string">'/login'</span>, <span class="hljs-attr">component</span>: Login },
  {
    <span class="hljs-attr">path</span>: <span class="hljs-string">'/'</span>,
    <span class="hljs-attr">component</span>: Layout,
    <span class="hljs-attr">redirect</span>: <span class="hljs-string">'/dashboard'</span>,
    <span class="hljs-attr">name</span>: <span class="hljs-string">'首页'</span>,
    <span class="hljs-attr">children</span>: [{ <span class="hljs-attr">path</span>: <span class="hljs-string">'dashboard'</span>, <span class="hljs-attr">component</span>: dashboard }]
  },
]

<span class="hljs-comment">//实例化vue的时候只挂载constantRouter</span>
<span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> <span class="hljs-keyword">new</span> Router({
  <span class="hljs-attr">routes</span>: constantRouterMap
});

<span class="hljs-comment">//异步挂载的路由</span>
<span class="hljs-comment">//动态需要根据权限加载的路由表 </span>
<span class="hljs-keyword">export</span> <span class="hljs-keyword">const</span> asyncRouterMap = [
  {
    <span class="hljs-attr">path</span>: <span class="hljs-string">'/permission'</span>,
    <span class="hljs-attr">component</span>: Layout,
    <span class="hljs-attr">name</span>: <span class="hljs-string">'权限测试'</span>,
    <span class="hljs-attr">meta</span>: { <span class="hljs-attr">role</span>: [<span class="hljs-string">'admin'</span>,<span class="hljs-string">'super_editor'</span>] }, <span class="hljs-comment">//页面需要的权限</span>
    children: [
    { 
      <span class="hljs-attr">path</span>: <span class="hljs-string">'index'</span>,
      <span class="hljs-attr">component</span>: Permission,
      <span class="hljs-attr">name</span>: <span class="hljs-string">'权限测试页'</span>,
      <span class="hljs-attr">meta</span>: { <span class="hljs-attr">role</span>: [<span class="hljs-string">'admin'</span>,<span class="hljs-string">'super_editor'</span>] }  <span class="hljs-comment">//页面需要的权限</span>
    }]
  },
  { <span class="hljs-attr">path</span>: <span class="hljs-string">'*'</span>, <span class="hljs-attr">redirect</span>: <span class="hljs-string">'/404'</span>, <span class="hljs-attr">hidden</span>: <span class="hljs-literal">true</span> }
];
</code></pre>
<p>这里我们根据 <a href="https://router.vuejs.org/en/advanced/meta.html" rel="nofollow noreferrer" target="_blank">vue-router官方推荐</a> 的方法通过meta标签来标示改页面能访问的权限有哪些。如<code>meta: { role: ['admin','super_editor'] }</code>表示该页面只有admin和超级编辑才能有资格进入。</p>
<p><strong>注意事项</strong>：这里有一个需要非常注意的地方就是 <code>404</code> 页面一定要最后加载，如果放在<code>constantRouterMap</code>一同声明了<code>404</code>，后面的所以页面都会被拦截到<code>404</code>，详细的问题见<a href="https://github.com/vuejs/vue-router/issues/1176" rel="nofollow noreferrer" target="_blank">addRoutes when you've got a wildcard route for 404s does not work</a></p>
<h3 id="articleHeader8">main.js</h3>
<p><strong>关键的main.js</strong></p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="// main.js
router.beforeEach((to, from, next) => {
  if (store.getters.token) { // 判断是否有token
    if (to.path === '/login') {
      next({ path: '/' });
    } else {
      if (store.getters.roles.length === 0) { // 判断当前用户是否已拉取完user_info信息
        store.dispatch('GetInfo').then(res => { // 拉取info
          const roles = res.data.role;
          store.dispatch('GenerateRoutes', { roles }).then(() => { // 生成可访问的路由表
            router.addRoutes(store.getters.addRouters) // 动态添加可访问路由表
            next({ ...to, replace: true }) // hack方法 确保addRoutes已完成 ,set the replace: true so the navigation will not leave a history record
          })
        }).catch(err => {
          console.log(err);
        });
      } else {
        next() //当有用户权限的时候，说明所有可访问路由已生成 如访问没权限的全面会自动进入404页面
      }
    }
  } else {
    if (whiteList.indexOf(to.path) !== -1) { // 在免登录白名单，直接进入
      next();
    } else {
      next('/login'); // 否则全部重定向到登录页
    }
  }
});
" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs javascript"><code><span class="hljs-comment">// main.js</span>
router.beforeEach(<span class="hljs-function">(<span class="hljs-params">to, <span class="hljs-keyword">from</span>, next</span>) =&gt;</span> {
  <span class="hljs-keyword">if</span> (store.getters.token) { <span class="hljs-comment">// 判断是否有token</span>
    <span class="hljs-keyword">if</span> (to.path === <span class="hljs-string">'/login'</span>) {
      next({ <span class="hljs-attr">path</span>: <span class="hljs-string">'/'</span> });
    } <span class="hljs-keyword">else</span> {
      <span class="hljs-keyword">if</span> (store.getters.roles.length === <span class="hljs-number">0</span>) { <span class="hljs-comment">// 判断当前用户是否已拉取完user_info信息</span>
        store.dispatch(<span class="hljs-string">'GetInfo'</span>).then(<span class="hljs-function"><span class="hljs-params">res</span> =&gt;</span> { <span class="hljs-comment">// 拉取info</span>
          <span class="hljs-keyword">const</span> roles = res.data.role;
          store.dispatch(<span class="hljs-string">'GenerateRoutes'</span>, { roles }).then(<span class="hljs-function"><span class="hljs-params">()</span> =&gt;</span> { <span class="hljs-comment">// 生成可访问的路由表</span>
            router.addRoutes(store.getters.addRouters) <span class="hljs-comment">// 动态添加可访问路由表</span>
            next({ ...to, <span class="hljs-attr">replace</span>: <span class="hljs-literal">true</span> }) <span class="hljs-comment">// hack方法 确保addRoutes已完成 ,set the replace: true so the navigation will not leave a history record</span>
          })
        }).catch(<span class="hljs-function"><span class="hljs-params">err</span> =&gt;</span> {
          <span class="hljs-built_in">console</span>.log(err);
        });
      } <span class="hljs-keyword">else</span> {
        next() <span class="hljs-comment">//当有用户权限的时候，说明所有可访问路由已生成 如访问没权限的全面会自动进入404页面</span>
      }
    }
  } <span class="hljs-keyword">else</span> {
    <span class="hljs-keyword">if</span> (whiteList.indexOf(to.path) !== <span class="hljs-number">-1</span>) { <span class="hljs-comment">// 在免登录白名单，直接进入</span>
      next();
    } <span class="hljs-keyword">else</span> {
      next(<span class="hljs-string">'/login'</span>); <span class="hljs-comment">// 否则全部重定向到登录页</span>
    }
  }
});
</code></pre>
<p>这里的router.beforeEach也结合了上一章讲的一些登录逻辑代码。</p>
<p><span class="img-wrap"><img src="https://static.alili.tech/img/remote/1460000016613453?w=884&amp;h=1003" src="https://static.alili.tech/img/remote/1460000016613453?w=884&amp;h=1003" alt="重构之前权限判断代码" title="重构之前权限判断代码" style="cursor: pointer; display: inline;"></span><br>上面一张图就是在使用<code>addRoutes</code>方法之前的权限判断，非常的繁琐，因为我是把所有的路由都挂在了上去，所有我要各种判断当前的用户是否有权限进入该页面，各种<code>if/else</code>的嵌套，维护起来相当的困难。但现在有了<code>addRoutes</code>之后就非常的方便，我只挂载了用户有权限进入的页面，没权限，路由自动帮我跳转的<code>404</code>,省去了不少的判断。</p>
<p>这里还有一个小hack的地方，就是<code>router.addRoutes</code>之后的<code>next()</code>可能会失效，因为可能<code>next()</code>的时候路由并没有完全add完成，好在查阅文档发现</p>
<blockquote>next('/') or next({ path: '/' }): redirect to a different location. The current navigation will be aborted and a new one will be started.</blockquote>
<p>这样我们就可以简单的通过<code>next(to)</code>巧妙的避开之前的那个问题了。这行代码重新进入<code>router.beforeEach</code>这个钩子，这时候再通过<code>next()</code>来释放钩子，就能确保所有的路由都已经挂在完成了。</p>
<h3 id="articleHeader9">store/permission.js</h3>
<p>就来就讲一讲 <code>GenerateRoutes Action</code></p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="// store/permission.js
import { asyncRouterMap, constantRouterMap } from 'src/router';

function hasPermission(roles, route) {
  if (route.meta &amp;&amp; route.meta.role) {
    return roles.some(role => route.meta.role.indexOf(role) >= 0)
  } else {
    return true
  }
}

const permission = {
  state: {
    routers: constantRouterMap,
    addRouters: []
  },
  mutations: {
    SET_ROUTERS: (state, routers) => {
      state.addRouters = routers;
      state.routers = constantRouterMap.concat(routers);
    }
  },
  actions: {
    GenerateRoutes({ commit }, data) {
      return new Promise(resolve => {
        const { roles } = data;
        const accessedRouters = asyncRouterMap.filter(v => {
          if (roles.indexOf('admin') >= 0) return true;
          if (hasPermission(roles, v)) {
            if (v.children &amp;&amp; v.children.length > 0) {
              v.children = v.children.filter(child => {
                if (hasPermission(roles, child)) {
                  return child
                }
                return false;
              });
              return v
            } else {
              return v
            }
          }
          return false;
        });
        commit('SET_ROUTERS', accessedRouters);
        resolve();
      })
    }
  }
};

export default permission;
" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs javascript"><code><span class="hljs-comment">// store/permission.js</span>
<span class="hljs-keyword">import</span> { asyncRouterMap, constantRouterMap } <span class="hljs-keyword">from</span> <span class="hljs-string">'src/router'</span>;

<span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">hasPermission</span>(<span class="hljs-params">roles, route</span>) </span>{
  <span class="hljs-keyword">if</span> (route.meta &amp;&amp; route.meta.role) {
    <span class="hljs-keyword">return</span> roles.some(<span class="hljs-function"><span class="hljs-params">role</span> =&gt;</span> route.meta.role.indexOf(role) &gt;= <span class="hljs-number">0</span>)
  } <span class="hljs-keyword">else</span> {
    <span class="hljs-keyword">return</span> <span class="hljs-literal">true</span>
  }
}

<span class="hljs-keyword">const</span> permission = {
  <span class="hljs-attr">state</span>: {
    <span class="hljs-attr">routers</span>: constantRouterMap,
    <span class="hljs-attr">addRouters</span>: []
  },
  <span class="hljs-attr">mutations</span>: {
    <span class="hljs-attr">SET_ROUTERS</span>: <span class="hljs-function">(<span class="hljs-params">state, routers</span>) =&gt;</span> {
      state.addRouters = routers;
      state.routers = constantRouterMap.concat(routers);
    }
  },
  <span class="hljs-attr">actions</span>: {
    GenerateRoutes({ commit }, data) {
      <span class="hljs-keyword">return</span> <span class="hljs-keyword">new</span> <span class="hljs-built_in">Promise</span>(<span class="hljs-function"><span class="hljs-params">resolve</span> =&gt;</span> {
        <span class="hljs-keyword">const</span> { roles } = data;
        <span class="hljs-keyword">const</span> accessedRouters = asyncRouterMap.filter(<span class="hljs-function"><span class="hljs-params">v</span> =&gt;</span> {
          <span class="hljs-keyword">if</span> (roles.indexOf(<span class="hljs-string">'admin'</span>) &gt;= <span class="hljs-number">0</span>) <span class="hljs-keyword">return</span> <span class="hljs-literal">true</span>;
          <span class="hljs-keyword">if</span> (hasPermission(roles, v)) {
            <span class="hljs-keyword">if</span> (v.children &amp;&amp; v.children.length &gt; <span class="hljs-number">0</span>) {
              v.children = v.children.filter(<span class="hljs-function"><span class="hljs-params">child</span> =&gt;</span> {
                <span class="hljs-keyword">if</span> (hasPermission(roles, child)) {
                  <span class="hljs-keyword">return</span> child
                }
                <span class="hljs-keyword">return</span> <span class="hljs-literal">false</span>;
              });
              <span class="hljs-keyword">return</span> v
            } <span class="hljs-keyword">else</span> {
              <span class="hljs-keyword">return</span> v
            }
          }
          <span class="hljs-keyword">return</span> <span class="hljs-literal">false</span>;
        });
        commit(<span class="hljs-string">'SET_ROUTERS'</span>, accessedRouters);
        resolve();
      })
    }
  }
};

<span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> permission;
</code></pre>
<p>这里的代码说白了就是干了一件事，通过用户的权限和之前在router.js里面asyncRouterMap的每一个页面所需要的权限做匹配，最后返回一个该用户能够访问路由有哪些。</p>
<hr>
<h3 id="articleHeader10">侧边栏</h3>
<p>最后一个涉及到权限的地方就是侧边栏，不过在前面的基础上已经很方便就能实现动态显示侧边栏了。这里侧边栏基于element-ui的NavMenu来实现的。<br>代码有点多不贴详细的代码了，有兴趣的可以直接去github上看<a href="https://github.com/PanJiaChen/vue-element-admin/tree/master/src/views/layout/components/Sidebar" rel="nofollow noreferrer" target="_blank">地址</a>，或者直接看关于侧边栏的<a href="https://panjiachen.github.io/vue-element-admin-site/#/router-and-nav" rel="nofollow noreferrer" target="_blank">文档</a>。</p>
<p>说白了就是遍历之前算出来的<code>permission_routers</code>，通过vuex拿到之后动态v-for渲染而已。不过这里因为有一些业务需求所以加了很多判断<br>比如我们在定义路由的时候会加很多参数</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="/**
* hidden: true                   if `hidden:true` will not show in the sidebar(default is false)
* redirect: noredirect           if `redirect:noredirect` will no redirct in the breadcrumb
* name:'router-name'             the name is used by <keep-alive> (must set!!!)
* meta : {
   role: ['admin','editor']     will control the page role (you can set multiple roles)
   title: 'title'               the name show in submenu and breadcrumb (recommend set)
   icon: 'svg-name'             the icon show in the sidebar,
   noCache: true                if fasle ,the page will no be cached(default is false)
 }
**/" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs applescript"><code>/**
* hidden: <span class="hljs-literal">true</span>                   <span class="hljs-keyword">if</span> `hidden:<span class="hljs-literal">true</span>` will <span class="hljs-keyword">not</span> show <span class="hljs-keyword">in</span> <span class="hljs-keyword">the</span> sidebar(default <span class="hljs-keyword">is</span> <span class="hljs-literal">false</span>)
* redirect: noredirect           <span class="hljs-keyword">if</span> `redirect:noredirect` will no redirct <span class="hljs-keyword">in</span> <span class="hljs-keyword">the</span> breadcrumb
* <span class="hljs-built_in">name</span>:'router-<span class="hljs-built_in">name</span>'             <span class="hljs-keyword">the</span> <span class="hljs-built_in">name</span> <span class="hljs-keyword">is</span> used <span class="hljs-keyword">by</span> &lt;keep-alive&gt; (must <span class="hljs-keyword">set</span>!!!)
* meta : {
   role: ['admin','editor']     will control <span class="hljs-keyword">the</span> page role (you can <span class="hljs-keyword">set</span> multiple roles)
   title: 'title'               <span class="hljs-keyword">the</span> <span class="hljs-built_in">name</span> show <span class="hljs-keyword">in</span> submenu <span class="hljs-keyword">and</span> breadcrumb (recommend <span class="hljs-keyword">set</span>)
   icon: 'svg-<span class="hljs-built_in">name</span>'             <span class="hljs-keyword">the</span> icon show <span class="hljs-keyword">in</span> <span class="hljs-keyword">the</span> sidebar,
   noCache: <span class="hljs-literal">true</span>                <span class="hljs-keyword">if</span> fasle ,<span class="hljs-keyword">the</span> page will no be cached(default <span class="hljs-keyword">is</span> <span class="hljs-literal">false</span>)
 }
**/</code></pre>
<p>这里仅供参考，而且本项目为了支持无限嵌套路由，所有侧边栏这块使用了递归组件。如需要请大家自行改造，来打造满足自己业务需求的侧边栏。</p>
<p><strong>侧边栏高亮问题</strong>:很多人在群里问为什么自己的侧边栏不能跟着自己的路由高亮，其实很简单，element-ui官方已经给了<code>default-active</code>所以我们只要</p>
<blockquote>:default-active="$route.path"<br>将<code>default-active</code>一直指向当前路由就可以了，就是这么简单</blockquote>
<hr>
<h2 id="articleHeader11">按钮级别权限控制</h2>
<p>有很多人一直在问关于按钮级别粒度的权限控制怎么做。我司现在是这样的，真正需要按钮级别控制的地方不是很多，现在是通过获取到用户的role之后，在前端用v-if手动判断来区分不同权限对应的按钮的。理由前面也说了，我司颗粒度的权限判断是交给后端来做的，每个操作后端都会进行权限判断。而且我觉得其实前端真正需要按钮级别判断的地方不是很多，如果一个页面有很多种不同权限的按钮，我觉得更多的应该是考虑产品层面是否设计合理。当然你强行说我想做按钮级别的权限控制，你也可以参照路由层面的做法，搞一个操作权限表。。。但个人觉得有点多此一举。或者将它封装成一个指令都是可以的。</p>
<hr>
<h2 id="articleHeader12">axios拦截器</h2>
<p>这里再说一说 axios 吧。虽然在上一篇<a href="https://juejin.im/post/59097cd7a22b9d0065fb61d2" rel="nofollow noreferrer" target="_blank">系列文章</a>中简单介绍过，不过这里还是要在唠叨一下。如上文所说，我司服务端对每一个请求都会验证权限，所以这里我们针对业务封装了一下请求。首先我们通过<strong>request拦截器</strong>在每个请求头里面塞入<strong>token</strong>，好让后端对请求进行权限验证。并创建一个respone拦截器，当服务端返回特殊的状态码，我们统一做处理，如没权限或者token失效等操作。</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="import axios from 'axios'
import { Message } from 'element-ui'
import store from '@/store'
import { getToken } from '@/utils/auth'

// 创建axios实例
const service = axios.create({
  baseURL: process.env.BASE_API, // api的base_url
  timeout: 5000 // 请求超时时间
})

// request拦截器
service.interceptors.request.use(config => {
  // Do something before request is sent
  if (store.getters.token) {
    config.headers['X-Token'] = getToken() // 让每个请求携带token--['X-Token']为自定义key 请根据实际情况自行修改
  }
  return config
}, error => {
  // Do something with request error
  console.log(error) // for debug
  Promise.reject(error)
})

// respone拦截器
service.interceptors.response.use(
  response => response,
  /**
  * 下面的注释为通过response自定义code来标示请求状态，当code返回如下情况为权限有问题，登出并返回到登录页
  * 如通过xmlhttprequest 状态码标识 逻辑可写在下面error中
  */
  //  const res = response.data;
  //     if (res.code !== 20000) {
  //       Message({
  //         message: res.message,
  //         type: 'error',
  //         duration: 5 * 1000
  //       });
  //       // 50008:非法的token; 50012:其他客户端登录了;  50014:Token 过期了;
  //       if (res.code === 50008 || res.code === 50012 || res.code === 50014) {
  //         MessageBox.confirm('你已被登出，可以取消继续留在该页面，或者重新登录', '确定登出', {
  //           confirmButtonText: '重新登录',
  //           cancelButtonText: '取消',
  //           type: 'warning'
  //         }).then(() => {
  //           store.dispatch('FedLogOut').then(() => {
  //             location.reload();// 为了重新实例化vue-router对象 避免bug
  //           });
  //         })
  //       }
  //       return Promise.reject('error');
  //     } else {
  //       return response.data;
  //     }
  error => {
    console.log('err' + error)// for debug
    Message({
      message: error.message,
      type: 'error',
      duration: 5 * 1000
    })
    return Promise.reject(error)
  })

export default service
" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="javascript hljs"><code class="js"><span class="hljs-keyword">import</span> axios <span class="hljs-keyword">from</span> <span class="hljs-string">'axios'</span>
<span class="hljs-keyword">import</span> { Message } <span class="hljs-keyword">from</span> <span class="hljs-string">'element-ui'</span>
<span class="hljs-keyword">import</span> store <span class="hljs-keyword">from</span> <span class="hljs-string">'@/store'</span>
<span class="hljs-keyword">import</span> { getToken } <span class="hljs-keyword">from</span> <span class="hljs-string">'@/utils/auth'</span>

<span class="hljs-comment">// 创建axios实例</span>
<span class="hljs-keyword">const</span> service = axios.create({
  <span class="hljs-attr">baseURL</span>: process.env.BASE_API, <span class="hljs-comment">// api的base_url</span>
  timeout: <span class="hljs-number">5000</span> <span class="hljs-comment">// 请求超时时间</span>
})

<span class="hljs-comment">// request拦截器</span>
service.interceptors.request.use(<span class="hljs-function"><span class="hljs-params">config</span> =&gt;</span> {
  <span class="hljs-comment">// Do something before request is sent</span>
  <span class="hljs-keyword">if</span> (store.getters.token) {
    config.headers[<span class="hljs-string">'X-Token'</span>] = getToken() <span class="hljs-comment">// 让每个请求携带token--['X-Token']为自定义key 请根据实际情况自行修改</span>
  }
  <span class="hljs-keyword">return</span> config
}, error =&gt; {
  <span class="hljs-comment">// Do something with request error</span>
  <span class="hljs-built_in">console</span>.log(error) <span class="hljs-comment">// for debug</span>
  <span class="hljs-built_in">Promise</span>.reject(error)
})

<span class="hljs-comment">// respone拦截器</span>
service.interceptors.response.use(
  <span class="hljs-function"><span class="hljs-params">response</span> =&gt;</span> response,
  <span class="hljs-comment">/**
  * 下面的注释为通过response自定义code来标示请求状态，当code返回如下情况为权限有问题，登出并返回到登录页
  * 如通过xmlhttprequest 状态码标识 逻辑可写在下面error中
  */</span>
  <span class="hljs-comment">//  const res = response.data;</span>
  <span class="hljs-comment">//     if (res.code !== 20000) {</span>
  <span class="hljs-comment">//       Message({</span>
  <span class="hljs-comment">//         message: res.message,</span>
  <span class="hljs-comment">//         type: 'error',</span>
  <span class="hljs-comment">//         duration: 5 * 1000</span>
  <span class="hljs-comment">//       });</span>
  <span class="hljs-comment">//       // 50008:非法的token; 50012:其他客户端登录了;  50014:Token 过期了;</span>
  <span class="hljs-comment">//       if (res.code === 50008 || res.code === 50012 || res.code === 50014) {</span>
  <span class="hljs-comment">//         MessageBox.confirm('你已被登出，可以取消继续留在该页面，或者重新登录', '确定登出', {</span>
  <span class="hljs-comment">//           confirmButtonText: '重新登录',</span>
  <span class="hljs-comment">//           cancelButtonText: '取消',</span>
  <span class="hljs-comment">//           type: 'warning'</span>
  <span class="hljs-comment">//         }).then(() =&gt; {</span>
  <span class="hljs-comment">//           store.dispatch('FedLogOut').then(() =&gt; {</span>
  <span class="hljs-comment">//             location.reload();// 为了重新实例化vue-router对象 避免bug</span>
  <span class="hljs-comment">//           });</span>
  <span class="hljs-comment">//         })</span>
  <span class="hljs-comment">//       }</span>
  <span class="hljs-comment">//       return Promise.reject('error');</span>
  <span class="hljs-comment">//     } else {</span>
  <span class="hljs-comment">//       return response.data;</span>
  <span class="hljs-comment">//     }</span>
  error =&gt; {
    <span class="hljs-built_in">console</span>.log(<span class="hljs-string">'err'</span> + error)<span class="hljs-comment">// for debug</span>
    Message({
      <span class="hljs-attr">message</span>: error.message,
      <span class="hljs-attr">type</span>: <span class="hljs-string">'error'</span>,
      <span class="hljs-attr">duration</span>: <span class="hljs-number">5</span> * <span class="hljs-number">1000</span>
    })
    <span class="hljs-keyword">return</span> <span class="hljs-built_in">Promise</span>.reject(error)
  })

<span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> service
</code></pre>
<hr>
<h3 id="articleHeader13">两步验证</h3>
<p><span class="img-wrap"><img src="https://static.alili.tech/img/remote/1460000016613454?w=1265&amp;h=574" del-src="https://static.alili.tech/v-5bbf1b3b/global/img/squares.svg" alt="" title="" style="cursor: pointer;"></span></p>
<p>文章一开始也说了，后台的安全性是很重要的，简简单单的一个账号+密码的方式是很难保证安全性的。所以我司的后台项目都是用了两步验证的方式，之前我们也尝试过使用基于 <a href="https://github.com/google/google-authenticator" rel="nofollow noreferrer" target="_blank">google-authenticator</a> 或者<code>youbikey</code>这样的方式但难度和操作成本都比较大。后来还是准备借助腾讯爸爸，这年代谁不用微信。。。安全性腾讯爸爸也帮我做好了保障。<br><strong>楼主建议</strong>两步验证要支持多个渠道不要只微信或者QQ，前段时间QQ第三方登录就出了bug，官方两三天才修好的，害我背了锅/(ㄒoㄒ)/~~ 。</p>
<p>这里的两部验证有点名不副实，其实就是账号密码验证过之后还需要一个绑定的第三方平台登录验证而已。<br>写起来也很简单，在原有登录得逻辑上改造一下就好。</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="this.$store.dispatch('LoginByEmail', this.loginForm).then(() => {
  //this.$router.push({ path: '/' });
  //不重定向到首页
  this.showDialog = true //弹出选择第三方平台的dialog
}).catch(err => {
  this.$message.error(err); //登录失败提示错误
});" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs kotlin"><code><span class="hljs-keyword">this</span>.$store.dispatch(<span class="hljs-string">'LoginByEmail'</span>, <span class="hljs-keyword">this</span>.loginForm).then(() =&gt; {
  <span class="hljs-comment">//this.$router.push({ path: '/' });</span>
  <span class="hljs-comment">//不重定向到首页</span>
  <span class="hljs-keyword">this</span>.showDialog = <span class="hljs-literal">true</span> <span class="hljs-comment">//弹出选择第三方平台的dialog</span>
}).<span class="hljs-keyword">catch</span>(err =&gt; {
  <span class="hljs-keyword">this</span>.$message.error(err); <span class="hljs-comment">//登录失败提示错误</span>
});</code></pre>
<p>登录成功之后不直接跳到首页而是让用户两步登录，选择登录得平台。<br>接下来就是所有第三方登录一样的地方通过 OAuth2.0 授权。这个各大平台大同小异，大家自行查阅文档，不展开了，就说一个微信授权比较坑的地方。<strong>注意</strong>你连参数的顺序都不能换，不然会验证不通过。<a href="https://github.com/PanJiaChen/vue-element-admin/blob/master/src/views/login/socialsignin.vue" rel="nofollow noreferrer" target="_blank">具体代码</a>，同时我也封装了<a href="https://github.com/PanJiaChen/vue-element-admin/blob/master/src/utils/openWindow.js" rel="nofollow noreferrer" target="_blank">openWindow</a>方法大家自行看吧。<br>当第三方授权成功之后都会跳到一个你之前有一个传入redirect——uri的页面<br><span class="img-wrap"><img src="https://static.alili.tech/img/remote/1460000016613455" del-src="https://static.alili.tech/v-5bbf1b3b/global/img/squares.svg" alt="" title="" style="cursor: pointer;"></span><br>如微信还必须是你授权账号的一级域名。所以你授权的域名是vue-element-admin.com,你就必须重定向到vue-element-admin.com/xxx/下面，所以你需要写一个重定向的服务，如vue-element-admin.com/auth/redirect?a.com 跳到该页面时会再次重定向给a.com。</p>
<p>所以我们后台也需要开一个authredirect页面：<a href="https://github.com/PanJiaChen/vue-element-admin/blob/master/src/views/login/authredirect.vue" rel="nofollow noreferrer" target="_blank">代码</a>。他的作用是第三方登录成功之后会默认跳到授权的页面，授权的页面会再次重定向回我们的后台，由于是spa，改变路由的体验不好，我们通过<code>window.opener.location.href</code>的方式改变hash，在login.js里面再监听hash的变化。当hash变化时，获取之前第三方登录成功返回的code与第一步账号密码登录之后返回的uid一同发送给服务端验证是否正确，如果正确，这时候就是真正的登录成功。</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text=" created() {
     window.addEventListener('hashchange', this.afterQRScan);
   },
   destroyed() {
     window.removeEventListener('hashchange', this.afterQRScan);
   },
   afterQRScan() {
     const hash = window.location.hash.slice(1);
     const hashObj = getQueryObject(hash);
     const originUrl = window.location.origin;
     history.replaceState({}, '', originUrl);
     const codeMap = {
       wechat: 'code',
       tencent: 'code'
     };
     const codeName = hashObj[codeMap[this.auth_type]];
     this.$store.dispatch('LoginByThirdparty', codeName).then(() => {
       this.$router.push({
         path: '/'
       });
     });
   }" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs javascript"><code> created() {
     <span class="hljs-built_in">window</span>.addEventListener(<span class="hljs-string">'hashchange'</span>, <span class="hljs-keyword">this</span>.afterQRScan);
   },
   destroyed() {
     <span class="hljs-built_in">window</span>.removeEventListener(<span class="hljs-string">'hashchange'</span>, <span class="hljs-keyword">this</span>.afterQRScan);
   },
   afterQRScan() {
     <span class="hljs-keyword">const</span> hash = <span class="hljs-built_in">window</span>.location.hash.slice(<span class="hljs-number">1</span>);
     <span class="hljs-keyword">const</span> hashObj = getQueryObject(hash);
     <span class="hljs-keyword">const</span> originUrl = <span class="hljs-built_in">window</span>.location.origin;
     history.replaceState({}, <span class="hljs-string">''</span>, originUrl);
     <span class="hljs-keyword">const</span> codeMap = {
       <span class="hljs-attr">wechat</span>: <span class="hljs-string">'code'</span>,
       <span class="hljs-attr">tencent</span>: <span class="hljs-string">'code'</span>
     };
     <span class="hljs-keyword">const</span> codeName = hashObj[codeMap[<span class="hljs-keyword">this</span>.auth_type]];
     <span class="hljs-keyword">this</span>.$store.dispatch(<span class="hljs-string">'LoginByThirdparty'</span>, codeName).then(<span class="hljs-function"><span class="hljs-params">()</span> =&gt;</span> {
       <span class="hljs-keyword">this</span>.$router.push({
         <span class="hljs-attr">path</span>: <span class="hljs-string">'/'</span>
       });
     });
   }</code></pre>
<p>到这里涉及登录权限的东西也差不多讲完了，这里楼主只是给了大家一个实现的思路(都是楼主不断摸索的血泪史)，每个公司实现的方案都有些出入，请谨慎选择适合自己业务形态的解决方案。如果有什么想法或者建议欢迎去本项目下留言，一同<a href="https://github.com/PanJiaChen/vue-element-admin/issues/new" rel="nofollow noreferrer" target="_blank">讨论</a>。</p>
<hr>
<h2 id="articleHeader14">占坑</h2>
<p>常规占坑，这里是手摸手，带你用vue撸后台系列。   <br>完整项目地址：<a href="https://github.com/PanJiaChen/vue-element-admin" rel="nofollow noreferrer" target="_blank">vue-element-admin</a>  <br>系类文章一：<a href="https://juejin.im/post/59097cd7a22b9d0065fb61d2" rel="nofollow noreferrer" target="_blank">手摸手，带你用vue撸后台 系列一（基础篇）</a>  <br>系类文章二：<a href="https://juejin.im/post/591aa14f570c35006961acac" rel="nofollow noreferrer" target="_blank">手摸手，带你用vue撸后台 系列二(登录权限篇)</a>  <br>系类文章三：<a href="https://juejin.im/post/593121aa0ce4630057f70d35" rel="nofollow noreferrer" target="_blank">手摸手，带你用vue 撸后台 系列三 (实战篇)</a>  <br>系类文章四：<a href="https://juejin.im/post/595b4d776fb9a06bbe7dba56" rel="nofollow noreferrer" target="_blank">手摸手，带你用vue撸后台 系列四(vueAdmin 一个极简的后台基础模板)</a>  <br>系类文章：<a href="https://juejin.im/post/59bb864b5188257e7a427c09" rel="nofollow noreferrer" target="_blank">手摸手，带你优雅的使用 icon</a>  <br>系类文章：<a href="https://segmentfault.com/a/1190000009090836">手摸手，带你封装一个vue component</a>  <br>楼主个人免费<a href="https://jianshiapp.com/circles/1209" rel="nofollow noreferrer" target="_blank">圈子</a></p>

                
{{< /raw >}}

# 版权声明
本文仅用于学习、研究和交流目的。转载请注明出处、完整链接以及原作者。 

本文资源来源互联网，仅供学习研究使用，版权归该资源的合法拥有者所有，

原作者若认为本站侵犯了您的版权，请联系我们，我们会立即删除！

## 原文链接
[https://segmentfault.com/a/1190000009506097](https://segmentfault.com/a/1190000009506097)

## 原文标题
手摸手，带你用vue撸后台 系列二(登录权限篇)
