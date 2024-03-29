---
title: '前端每周清单第 23 期：React 内部原理与实现，自定义基于 JavaScript 的 16 位虚拟机' 
date: 2019-01-07 2:30:11
hidden: true
slug: c73zfhr0dgu
categories: [reprint]
---

{{< raw >}}

                    
<blockquote><p><a href="https://zhuanlan.zhihu.com/p/28076241" rel="nofollow noreferrer" target="_blank">前端每周清单第 23 期：React 内部原理与实现，自定义基于 JavaScript 的 16 位虚拟机，使用 Apollo Server 快速搭建 GraphQL 服务端</a> 为InfoQ中文站特供稿件，首发地址为<a href="https://parg.co/bhI" rel="nofollow noreferrer" target="_blank">前端之巅公众号</a>；如需转载，请与InfoQ中文站联系。从属于笔者的<a href="https://github.com/wxyyxc1992/Web-Development-And-Engineering-Practices" rel="nofollow noreferrer" target="_blank">  Web 前端入门与工程实践</a>的<a href="https://parg.co/bh1" rel="nofollow noreferrer" target="_blank">前端每周清单系列</a>系列；部分文章需要自备梯子。</p></blockquote>
<h1 id="articleHeader0">前端每周清单第 23 期：React 内部原理与实现，自定义基于 JavaScript 的 16 位虚拟机，使用 Apollo Server 快速搭建 GraphQL 服务端</h1>
<p><code>前端</code> <code>前端每周清单</code></p>
<p><a href="http://www.infoq.com/cn/FE-Weekly" rel="nofollow noreferrer" target="_blank">前端每周清单</a>专注前端领域内容，分为新闻热点、开发教程、工程实践、深度阅读、开源项目、巅峰人生等栏目。关注【前端之巅】微信公众号（ID：frontshow），及时获取前端每周清单。</p>
<h2 id="articleHeader1">新闻热点</h2>
<p><code>国内国外，前端最新动态</code></p>
<ul>
<li><p><a href="https://parg.co/baN" rel="nofollow noreferrer" target="_blank">React 开源许可证风波</a>：近日，Apache 基金会宣布禁止使用包括 React 在内的， Facebook 带附加条款的 BSD Licence的开源软件，引发了部分使用者的担忧；社区已经有很多人<a href="https://parg.co/bax" rel="nofollow noreferrer" target="_blank">请愿</a>修改 React 开源许可证，同时 Facebook 另一开源项目 RocksDB 已经表示会在截止日期前修改许可证；React 项目维护者表示 Facebook 内部正在讨论此事，我们也会持续跟踪。</p></li>
<li><p><a href="http://blog.npmjs.org/post/162986246605/v530-2017-07-13" rel="nofollow noreferrer" target="_blank">npm 5.3.0 发布</a>：自 5.x 版本发布之后，npm 一直致力于提升版本迭代速度，尽可能地通过小的更新来修复 npm 存在的问题。而本周发布的 5.3.0 版本为 npm ls 命令添加了 --link 参数，并且为 npx 添加了更多的支持语言版本；此外该版本还修复了一系列版本控制上的问题。</p></li>
<li><p><a href="https://blog.expo.io/expo-sdk-v19-0-0-is-now-available-821a62b58d3d" rel="nofollow noreferrer" target="_blank">Expo SDK v19.0.0 发布</a>：近日基于 React Native 0.46 版本的 Expo SDK v19.0.0 正式发布，其大幅度更新了 Android 中 JavaScriptCore 的特性支持与性能表现；除此之外该版本还优化了开发者工具与文档，并且引入了语音识别、地理位置编码、文件系统、异步加载资源以及 Android 应用的权限管理等多个新的特性。</p></li>
<li><p><a href="https://parg.co/bWH" rel="nofollow noreferrer" target="_blank">Angular 5.0.0-beta.0 以及 Angular 4.3.1 发布</a>：本周 Angular 5.0.0-beta.0 版本发布， 其对于动画以及懒加载模块进行了大幅优化；同时本周还发布了 Angular 4.3.1 以及 angular-cli 1.2.2 版本。</p></li>
</ul>
<h2 id="articleHeader2">开发教程</h2>
<p><code>步步为营，掌握基础技能</code></p>
<ul>
<li><p><a href="https://deepstreamhub.com/tutorials/protocols/webrtc-intro/" rel="nofollow noreferrer" target="_blank">WebRTC 实战教程</a>：本系列教程会带你回顾，从构建点对点的基于 WebRTC 的数据、音频与视频传输信道开始，到搭建适用于真实场景的多人聊天室中的文件共享、视频操作以及屏幕共享等实践。本系列教程旨在让读者构建清晰、直观地 WebRTC 知识体系，能够独立地构建基于 WebRTC 的应用。本系列教程分为七个部分，依次是基础概念讲解、数据信道搭建、网络连通性诊断、音视频处理、视频操作、屏幕分享、文件传输、发布到生产环境等。</p></li>
<li><p><a href="https://parg.co/bay" rel="nofollow noreferrer" target="_blank">ES6 中的 JavaScript 工厂函数实现</a>：本文归属于 Eric Elliott 发布的 Composing Software 系列，介绍在 JavaScript ES6 语法背景下如何实现工厂函数。所谓工厂函数即是非类或者构造函数的，能干会某个新创建对象的函数；工厂函数能够简化我们创建新对象的过程，本文即是详细地介绍了如何实现工厂函数，也是一篇不错的 ES6 函数语法讲解；更多 JavaScript 相关资料参考<a href="https://parg.co/bMI" rel="nofollow noreferrer" target="_blank"> https://parg.co/bMI  </a>。</p></li>
<li><p><a href="https://toddmotto.com/lazy-loading-angular-code-splitting-webpack" rel="nofollow noreferrer" target="_blank">基于 NGModules 与 Webpack 的 Angular 应用模块分割与懒加载</a>：本文主要讨论如何在 Angular 应用开发中利用 Webpack 与 NGModules 实现对于代码库的模块分割，并且利用懒加载来加载非首屏内容，从而提升整体的应用响应性能。本文首先介绍了代码分割与懒加载相关的概念知识，然后介绍了如何搭建 Webpack 基础环境，然后介绍了使用 NgModules 以及性能对比；更多 Webpack 相关资料参考<a href="https://parg.co/bVs" rel="nofollow noreferrer" target="_blank"> https://parg.co/bVs  </a>。</p></li>
<li><p><a href="https://codeburst.io/simple-data-visualization-with-react-js-svg-line-chart-tutorial-df12e5843ce" rel="nofollow noreferrer" target="_blank">基于 React.js 的简单数据可视化</a>：本文旨在介绍如何利用 React.js ，并且不借助任何第三方库的帮助，来实现简单的 SVG 线型图。本文首先讨论如何利用 Create React App 搭建简单 React 项目，然后介绍了 SVG 的基础语法以及如何创建模拟数据，最后介绍了如何进行逻辑代码分割并且编写单独的 LineChart 组件；更多 React 相关资料参考<a href="https://parg.co/bM1" rel="nofollow noreferrer" target="_blank"> https://parg.co/bM1 </a>。</p></li>
<li><p><a href="https://parg.co/bWY" rel="nofollow noreferrer" target="_blank">使用 Apollo Server 快速开发基于 Node.js 的 GraphQL 服务端</a>：Apollo Server 是由社区维护的开源 GraphQL 服务端，它支持目前主流的 Node.js HTTP 服务端框架：Express、Connect、Hapi、Koa、AWS Lambda、Restify 以及 Micro。本文首先介绍 Apollo Server 遵循着开放、简单、高性能的原则，然后介绍了基于 Express 的基础用法以及性能监控等内容；更多 GraphQL 相关资料参考 <a href="https://parg.co/b1e" rel="nofollow noreferrer" target="_blank"> https://parg.co/b1e </a>。</p></li>
<li><p><a href="https://parg.co/bW6" rel="nofollow noreferrer" target="_blank">Vue.js 组件的实践分享</a>：本文是来自 Morningstar 的工程师，分享的他们利用 Vue.js 进行前端组件化开发时的实践经验；主要是它们对于 Vue.js 组件编写的心得。作者首先介绍了组件不同生命周期回调的含义，然后介绍了从简单到复杂组件的状态与数据管理，接下来介绍了 Slot 与 Minxin 在组件重用上的具体用法等内容；更多 Vue.js 相关资料参考<a href="https://parg.co/byL" rel="nofollow noreferrer" target="_blank"> https://parg.co/byL </a>。</p></li>
</ul>
<h2 id="articleHeader3">工程实践</h2>
<p><code>立足实践，提示实际水平</code></p>
<ul>
<li><p><a href="https://parg.co/bWo" rel="nofollow noreferrer" target="_blank">Redux 优先的路由模型</a>：路由库是构建任何复杂单页应用的核心组件之一，如果你已经使用过 React 与 Redux 开发过 Web 应用，那么对于 React Router 这个著名的路由库并不陌生。不过 React Router 还是与界面层强耦合，而本文希望介绍以 Redux 为核心的路由解决方案，作者会深入浅出地介绍概念设计、核心代码编写以及如何用不到一百行代码来编写与组件框架相关的接口；更多 Redux 相关资料参考 <a href="https://parg.co/bVQ" rel="nofollow noreferrer" target="_blank"> https://parg.co/bVQ  </a>。</p></li>
<li><p><a href="https://codeburst.io/angular-best-practices-4bed7ae1d0b7" rel="nofollow noreferrer" target="_blank">Angular 最佳实践分享</a>：作者在本文中分享自己在工作中总结出的 Angular 应用实践，本文尽可能地避免官方的 Angular 样式指南中提及的约定，而是着眼于呈现个人的经验总结。本文依次介绍了类型定义、组件实践、服务定义、模板使用等方面。</p></li>
<li><p><a href="https://mo.github.io/2017/07/20/javascript-e2e-integration-testing.html" rel="nofollow noreferrer" target="_blank">基于 JavaScript 的 Web 应用的端到端测试工具对比</a>：本文回顾了常见的基于 JavaScript 的，用于对 Web 应用进行端到端测试的工具，并且对它们进行了简单对比。本文首先探讨了项目中应用端到端测试的意义，然后列举了当前可用的基于 JavaScript 的界面自动化测试框架，然后比较了不同的端到端测试框架的流行程度与基本的代码片风格；更多 Web 测试相关资料参考<a href="https://parg.co/bWd" rel="nofollow noreferrer" target="_blank"> https://parg.co/bWd  </a>。</p></li>
<li><p><a href="https://about.gitlab.com/2017/07/17/redesigning-gitlabs-navigation/" rel="nofollow noreferrer" target="_blank">重新设计的 Gitlab 导航</a>：本文记述了 Gitlab 在重新设计应用导航栏过程中的讨论与思索过程，是一篇不错的了解用户交互与产品设计的分享文章。本文首先介绍了对于导航栏应该包含的内容的分割，譬如分为全局内容与上下文内容；然后介绍了如何针对两个原型的设计理念，以及如何进行 A/B 测试以最终决定应用方案的过程。</p></li>
<li><p><a href="https://www.robinwieruch.de/learn-react-before-using-redux/" rel="nofollow noreferrer" target="_blank">学习 Redux 之前你应该了解的关于 React 的 8 件事</a>：状态管理是前端的难点之一，而 React 或类似的前端界面库往往只是提供了简单的组件内状态管理，因此很多开发者还是会逐步转向于 Redux 等专用的状态管理工具；不过也有很多开发者是在并没有真实碰到需要解决大规模可扩展的状态管理问题时，或者没有仔细了解过 React 内置的状态管理范式时就盲目地去学习了 Redux。而本文则依次介绍了本地状态管理基础、函数式本地状态、状态提升、高阶组件、Context、状态组件与无状态组件等内容；更多 React 相关资料参考<a href="https://parg.co/bM1" rel="nofollow noreferrer" target="_blank"> https://parg.co/bM1 </a>。</p></li>
</ul>
<h2 id="articleHeader4">深度阅读</h2>
<p><code>深度思考，升华开发智慧</code></p>
<ul>
<li><p><a href="https://parg.co/ba2" rel="nofollow noreferrer" target="_blank">美团点评收银台前端可用性保障实践</a>：本文主要讨论的是前端可用性相关话题，以在美团点评移动端网页收银台的实践为例，讲解收银台前端是如何保障可用性的。本文依次讨论了如何定义前端服务可用性、如何衡量前端服务可用性、哪里容易出问题、怎样保障才能被信服等内容。</p></li>
<li><p><a href="https://medium.com/envato/our-top-10-free-tools-for-frontend-web-development-15d8a6052652" rel="nofollow noreferrer" target="_blank">Web 开发中的十大常用工具</a>：每年都会涌现出很多优秀的 Web 开发辅助工具，而本文是来自于 Envato 的工程师分享的他们开发中常用的十个工具。本文依次介绍了可用于生成网格的 Grid.Guide、类似于 BootStrap 的样式库 Foundation、在线代码编辑与共享工具 CodePen、jQuery 插件聚集地 Unheap、自动界面刷新工具 LivePage、整页抓取工具 FullPage Screen Capture、字体辅助 WhatFont、Node/Npm、移动端速度测试、响应式速度测试工具等。</p></li>
<li><p><a href="https://github.com/syg/ecmascript-binary-ast/" rel="nofollow noreferrer" target="_blank">JavaScript Binary AST 提案</a>：随着 Web 应用体积的不断增加，页面启动时间逐渐成为了应用性能的主要瓶颈之一；我们可以通过多种方式来缓存代码，但是对于大型代码库的解析却难以直观解决。譬如在现代的笔记本上，Chrome 在加载 Facebook.com 的时候需要花费 10% 到 15% 的时间来解析 JavaScript 代码。本文介绍了由多位工程师提出的旨在提升 JavaScript 解析速度的 Binary AST 方案，本文介绍了当前解析中的瓶颈所在，并且给出了相应的解决建议。</p></li>
<li><p><a href="http://www.mattgreer.org/articles/react-internals-part-one-basic-rendering/" rel="nofollow noreferrer" target="_blank">React 内部原理与实现</a>：作者会在本系列五部分的文章中介绍 React 内部原理，并且带着读者一步一步重造出简单的类 React 框架；通过阅读这几篇文章，我们可以了解 React 的工作机制，譬如组件的完整生命周期等等。本系列文章依次介绍了基础渲染过程、componentWillMount 与 componentDidMount、基础更新、setState 与事务等内容；更多 React 相关资料参考<a href="https://parg.co/bM1" rel="nofollow noreferrer" target="_blank"> https://parg.co/bM1 </a>。</p></li>
<li><p><a href="https://francisstokes.wordpress.com/2017/07/20/16-bit-vm-in-javascript/" rel="nofollow noreferrer" target="_blank">自定义基于 JavaScript 的 16 位虚拟机</a>：本文介绍了如何利用 JavaScript 自定义 16 位虚拟机，主要包括如何设计某个简单的汇编语言、如何构建某个编译器能够将 <code>*.asm</code> 文件编译为可执行格式、如何构建某个能够模拟内存、CPU 以及部分 I/O 操作的虚拟机。文章内容依次介绍了虚拟硬件的基础、限制、汇编语言、编译器、调试器、编码与解码等内容；更多 JavaScript 相关资料参考<a href="https://parg.co/bMI" rel="nofollow noreferrer" target="_blank"> https://parg.co/bMI  </a>。</p></li>
</ul>
<h2 id="articleHeader5">开源项目</h2>
<p><code>乐于分享，共推前端发展</code></p>
<ul>
<li><p><a href="http://aurora.mljr.com/install.html" rel="nofollow noreferrer" target="_blank">Aurora</a>：Aurora  是基于 Vue 2.2.0 版的组件库, 提供了常见的网格布局、按钮、表单域、分页、模态窗口等组件。</p></li>
<li><p><a href="https://ecomfe.github.io/san/" rel="nofollow noreferrer" target="_blank">San</a>：San 是百度 EFE 团队出品的 Web MVVM 组件库，其具备体积小巧、引用方便、兼容性良好的特性，提供了高性能视图、组件反解、自由管理模块等功能。</p></li>
<li><p><a href="https://github.com/KingPixil/wade" rel="nofollow noreferrer" target="_blank">Wade</a>：Wade 是轻量级、高性能的 JavaScript 搜索库，Wade 会在构建阶段自动地为输入数组中的每个字符串的字符构建反向索引，然后在搜索时候快速返回用户输入关键字对应地下标；Wade 优势在于对于相同的数据集进行多次搜索时能够避免冗余的遍历。</p></li>
<li><p><a href="https://github.com/lord/slate" rel="nofollow noreferrer" target="_blank">Slate</a>：Slate 是基于 MarkDown 的接口文档生成工具，它能够从 MarkDown 文件配置中读取接口信息并且生成漂亮、智能、响应式的在线单页接口文档页面。</p></li>
<li><p><a href="https://parg.co/bWb" rel="nofollow noreferrer" target="_blank">swagger-decorator</a>：swagger-decorator 是旨在一处注解多处使用的 JavaScript &amp; Node.js 应用中实体类与方法注解库，其能够用于实体类生成与校验、Sequelize ORM 实体类生成、面向 Koa 的路由注解与 Swagger 文档自动生成的场景。</p></li>
</ul>
<h2 id="articleHeader6">巅峰人生</h2>
<h2 id="articleHeader7">前端之巅</h2>
<p>「前端之巅」是InfoQ旗下关注前端技术的垂直社群，加入前端之巅学习群请关注「前端之巅」公众号后回复“加群”。投稿请发邮件到editors@cn.infoq.com，注明“前端之巅投稿”。</p>
<p><span class="img-wrap"><img data-src="/img/remote/1460000008850035" src="https://static.alili.tech/img/remote/1460000008850035" alt="前端之巅微信底图－5.jpg" title="前端之巅微信底图－5.jpg" style="cursor: pointer; display: inline;"></span></p>

                
{{< /raw >}}

# 版权声明
本文资源来源互联网，仅供学习研究使用，版权归该资源的合法拥有者所有，

本文仅用于学习、研究和交流目的。转载请注明出处、完整链接以及原作者。

原作者若认为本站侵犯了您的版权，请联系我们，我们会立即删除！

## 原文标题
前端每周清单第 23 期：React 内部原理与实现，自定义基于 JavaScript 的 16 位虚拟机

## 原文链接
[https://segmentfault.com/a/1190000010324585](https://segmentfault.com/a/1190000010324585)

