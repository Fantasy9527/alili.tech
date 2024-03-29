---
title: '前端每周清单第 44 期: 2017 JS 调查报告、REST 接口实时化、ESM 的过去与未来' 
date: 2018-12-21 2:30:11
hidden: true
slug: rvrickj12u9
categories: [reprint]
---

{{< raw >}}

                    
<p><span class="img-wrap"><img data-src="/img/remote/1460000012476504?w=1240&amp;h=826" src="https://static.alili.tech/img/remote/1460000012476504?w=1240&amp;h=826" alt="" title="" style="cursor: pointer; display: inline;"></span></p>
<p><a href="http://www.infoq.com/cn/FE-Weekly" rel="nofollow noreferrer" target="_blank">前端每周清单</a>专注前端领域内容，以对外文资料的搜集为主，帮助开发者了解一周前端热点；分为新闻热点、开发教程、工程实践、深度阅读、开源项目、巅峰人生等栏目。欢迎关注【前端之巅】微信公众号（ID ： frontshow），及时获取前端每周清单。</p>
<h2 id="articleHeader0">新闻热点</h2>
<p><code>国内国外，前端最新动态</code></p>
<ul>
<li>
<a href="https://parg.co/UO2" rel="nofollow noreferrer" target="_blank">FCC 废除网络中立法案</a>: 所谓的网络中立性，要求网络服务供应商不能优待或者故意限制来自某些网站的流量。前总统奥巴马在 2015 年签署命令，规定以 1934 年通过的《联邦通讯法》“ 第二类 ” 业务来规管网络服务供应商，让联邦通讯委员会执法，禁止网络服务供应商优待任何公司。美国联邦通讯委员会（Federal Communications Commission, FCC ）早前通过废除网络中立性的规定，不再明文禁止网络服务供应商优待来自特定网站的流量。</li>
<li>
<a href="https://parg.co/Us5" rel="nofollow noreferrer" target="_blank">Expo SDK v24.0 发布</a>: 本周 Expo SDK 24.0 正式发布，其基于 React Native 0.51 版本；同时 Expo 的官方网站也迎来了极大的更新，搜索、项目创建、发现等界面都焕然一新。该版本中添加了离线图片支持功能，避免了每次应用初始化时都需要进行网络图片加载；同时添加了 iOS 权限对话框的配置，优化了 ImageManipulator, ImagePicker 等接口的功能。</li>
<li>
<a href="https://parg.co/Usm" rel="nofollow noreferrer" target="_blank">React Studio 1.3 发布</a>: React Studio 是图形化可交互地 React 应用开发工作台，本文即是介绍最新的 1.3 版本中包含的系列特性。首先是整体性能与交互体验的提升，并且增加了对多语言的支持，同时优化了 Mock 数据的创建方式；此外，该版本还引入了新的卡片、选择器等等一系列新的组件。</li>
</ul>
<h2 id="articleHeader1">开发教程</h2>
<p><code>步步为营，掌握基础技能</code></p>
<ul>
<li>
<a href="https://github.com/Chalarangelo/30-seconds-of-code" rel="nofollow noreferrer" target="_blank">JavaScript 基础代码片</a>: 本文整理了许多简明精巧的 JavaScript 开发中用到的代码片，既适合于初学者学习语法，也能帮助开发者温故知新。本文包含了数组的常见处理、浏览器元素与位置、时间与日期、函数与函数式编程、数学公式与计算、Node.js、Object、字符串以及很多的其他工具类；更多 JS 学习资料参考<a href="https://parg.co/U5v" rel="nofollow noreferrer" target="_blank">现代 JavaScript 开发：语法基础与工程实践</a>。</li>
<li>
<a href="https://parg.co/UsZ" rel="nofollow noreferrer" target="_blank">清除浏览器中的资源缓存</a>: 浏览器缓存是最常见的，也是最显著的提升前端性能的手段之一；不过在如果我们错误地将某些资源设置为了长期缓存，那么就要寻求方法强制刷新这些资源。本文即是介绍讨论如何强制刷新浏览器的某些资源缓存，作者依次讨论了 location.reload, vary + fetch, fetch + cache:reload, fetch + POST, iframe 中进行 POST, Clear-Site-Data 等方法；更多浏览器的存储操作参考<a href="https://parg.co/UHU" rel="nofollow noreferrer" target="_blank">现代 Web 开发基础</a>。</li>
<li>
<a href="https://parg.co/Usb" rel="nofollow noreferrer" target="_blank">基于 Vue.js 的 RSA 加密通信应用</a>: 加密是现代互联网的基石之一，本文即是希望通过构建简单的加密聊天应用，来引导读者了解加解密算法的基本概念。本文首先介绍了 2048 位的 RSA 加密算法的概念与实现方式，然后使用 Vue.js 来编写前端界面，并且使用 Node.js 以及 Socket.io 来编写服务端来协调各个客户端的消息。更多 Vue.js 相关资料参考 <a href="https://parg.co/Usp" rel="nofollow noreferrer" target="_blank">Vue.js Reference</a>。</li>
</ul>
<h2 id="articleHeader2">工程实践</h2>
<p><code>立足实践，提示实际水平</code></p>
<ul>
<li>
<a href="https://parg.co/Usl" rel="nofollow noreferrer" target="_blank">ESM 的目前实现与未来规划</a>: ES 模块化标准最早是 2015 年在 ECMAScript 6 中发布，现在我们已经可以在三个主流浏览器中使用 ES Modules。而 Node.js 目前是采用了 Common.js 模块化方案，我们可以在应用中通过 require 方法来引入其他模块。两种模块机制的巨大差异使得同时兼容 Common.js 与 ESModule 并非易事；而自 Node.js 8.9.0 以来，开发者可以实验性地使用 ESModules，本文即是对于社区的反馈以及 ESM 的未来规划进行介绍。更多相关资料参考 <a href="https://parg.co/UHE" rel="nofollow noreferrer" target="_blank">JS Reference</a>。</li>
<li>
<a href="https://parg.co/UsY" rel="nofollow noreferrer" target="_blank">基于 Apollo 的组件数据交互</a>: 本文是 Werkspot 的工程师分享的他们协同使用 Apollo Client 与 React Native 来开发应用的实践经验，着重讨论了 GraphQL 带来的易用性与灵活性。作者首先讨论了查询组件的构成，然后分析了如何结合查询组件与 Mutations，最后讨论了如何测试查询组件；更多 GraphQL 相关资料参考<a href="https://parg.co/Us3" rel="nofollow noreferrer" target="_blank">这里</a>。</li>
<li>
<a href="https://parg.co/Usq" rel="nofollow noreferrer" target="_blank">将 REST APIs 转化为实时 APIs</a>: 实时交互式现代技术栈中的重要组成，从而满足用户与企业的高速频繁地数据需求；本文即是介绍如何利用开源的 Pushpin 来将 REST API 转化为实时 API。本文首先介绍了请求-响应架构与事件驱动架构地区别，然后对比了现有的事件接口的解决方案，最后介绍了 Pushpin 的特性与部署方式。更多服务端架构讨论参考<a href="https://parg.co/UdT" rel="nofollow noreferrer" target="_blank">服务端应用程序开发基础</a>。</li>
<li>
<a href="https://mp.weixin.qq.com/s/Yv1ss1X1K-QG9fEXGjZ_zw" rel="nofollow noreferrer" target="_blank">Electron 开发跨平台构建流程设计</a>: 这是 Electron 系列文章的第二篇，本文将和大家分享我是怎么去构建自动化的 Electron 开发构建工程的，说白了，就是怎么把敲的代码变成一个用户可以下载安装的包。当然随着之后应用复杂度的提升和技术再选型，工程体系可能随时会重构或演进，但至少可以给大家一些参考，欢迎留言交流。工程自动化，应该是所有开发者的一种基础追求，当你搭建建好工程体系，以后你将专注于产品功能的开发，而不会花大量不必要的时间去手动构建。更多 Electron 相关资料参考<a href="https://parg.co/Us3" rel="nofollow noreferrer" target="_blank">这里</a>。</li>
</ul>
<h2 id="articleHeader3">深度阅读</h2>
<p><code>深度思考，升华开发智慧</code></p>
<ul>
<li>
<a href="https://stateofjs.com/2017/" rel="nofollow noreferrer" target="_blank">The State of JavaScript 2017</a>: 经过漫长的调研与数据整理之后，2017 年的 JavaScript 使用报告正式发布；本报告汇聚了来自数万名开发者对于语法、前端框架、状态管理、服务端框架、测试、CSS 、构建工具、移动端框架等等大前端相关技术栈的看法与使用体验。本报告仍然采取了乐于使用、正在使用、准备使用、不感兴趣、没听过等几个层次来描述开发者对于某个框架或者工具的看法；此外，本报告还提供了所谓 Connections 图解，即衡量使用者不同技术之间的关联度，譬如有多少 React 的使用者仍然使用了 Redux 等等。更多 JS 教程参考<a href="https://parg.co/U5v" rel="nofollow noreferrer" target="_blank">现代 JavaScript 开发：语法基础与工程实践</a>。</li>
<li>
<a href="https://parg.co/UOB" rel="nofollow noreferrer" target="_blank">NectarJS: 将 JavaScript 编译为平台相关的二进制代码</a>: 本文作者 Adrien Thierry 近年来致力于，打造将 JavaScript 编译为平台相关的二进制代码的途径，其在本文中介绍了开源的 NectarJS 的设计理念与运行机制。作者将 NectarJS 定位为编译即服务，即能够在优化 JavaScript 本身性能的同时，支持将其编译为 WebAssembly、IoT、Windows、OSX、Linux 等等各个平台或者目标的格式。更多 JS 教程参考<a href="https://parg.co/U5v" rel="nofollow noreferrer" target="_blank">现代 JavaScript 开发：语法基础与工程实践</a>。</li>
<li>
<a href="https://parg.co/UsU" rel="nofollow noreferrer" target="_blank">REST 就是新时代的 SOAP</a>: 本文作者分享了其对于 REST 的看法，不可避免地带有主观色彩，可以辩证地去看待。作者首先讨论了 RESTful API 的不足，其抽象简练的原则往往不能满足真实业务场景中的问题；然后作者又讨论了 REST 动词、错误处理乃至于基础概念上的不足，作者并未在本文中讨论他理想的解决方案，只是抛出了很多问题留待读者去思考。更多服务端架构讨论参考<a href="https://parg.co/UdT" rel="nofollow noreferrer" target="_blank">服务端应用程序开发基础</a>。</li>
<li>
<a href="https://parg.co/UsP" rel="nofollow noreferrer" target="_blank">利用机器学习突破图片验证码</a>: 图片验证码是现代网页中常见的安全防火墙之一，能够用于人机识别，避免爬虫等恶意抓取行为；本文则是以著名的 WordPress 图片验证码插件为例，介绍如何使用机器学习来突破验证码的限制。本文作者主要使用了 Python 3, OpenCV, Keras, TensorFlow 这些常见的机器学习库与工具，首先介绍了如何使用 WordPress 的插件创造训练数据集，然后介绍了深度卷积神经网络的基本原理以及如何进行神经网络的训练，最后介绍了如何使用训练好的模型进行图片识别。更多 Web 安全相关资料参考<a href="https://parg.co/U6E" rel="nofollow noreferrer" target="_blank">这里</a>。</li>
</ul>
<h2 id="articleHeader4">开源项目</h2>
<p><code>乐于分享，共推前端发展</code></p>
<ul>
<li>
<a href="https://github.com/exercism/exercism.io" rel="nofollow noreferrer" target="_blank">exercism.io</a>: Exercism 提供了超过三十种编程语言的数百个实践问题，以帮助开发者在实践中学习并且掌握某个编程语言。Exercism 还提供了便捷的客户端工具，帮助开发者快速搭建实验环境，并且允许开发者分享自己的见解与解决方案。</li>
<li>
<a href="https://github.com/developit/microbundle" rel="nofollow noreferrer" target="_blank">Microbundle</a>: Microbundle 是基于 Rollup 构建的零配置小模块打包工具，开发者只需要安装，并且在 package.json 内配置基础命令即可使用。Microbundle 会自动检测 index.js 或者 cli.js 这样的文件，作为入口文件，自动编译为 CommonJS、UMD 、 ESM 等多种格式。</li>
<li>
<a href="https://github.com/ballercat/walt" rel="nofollow noreferrer" target="_blank">WAlt</a>: WAlt 可以作为 WebAssembly 文本格式的中介，其尝试使开发者利用 JavaScript 的语法来直接编写 .walt 代码，然后直接编译为 WebAssembly。WALt 的优势在于，其并不需要 C/C++ 或者 Rust 环境，而只需要了解 JavaScript；并且编译的过程也不需要 LLVM 等二进制工具，还能够集成于 Webpack 等工具。</li>
</ul>
<h2 id="articleHeader5">巅峰人生</h2>
<ul><li>
<a href="https://parg.co/Us6" rel="nofollow noreferrer" target="_blank">10 年 IT 老兵：思路上的转变，远比单纯提升技术更有价值</a>: 本文节选自赵成教授在极客时间 App 开设的“赵成的运维体系管理课”，是其对自己十年技术生涯的回顾与总结。赵成教授来自美丽联合集团，集团旗下两大主力产品是蘑菇街和美丽说，目前负责管理集团的技术服务团队。作者在本文中依次分享了为什么我选择了踏上运维之路？、运维思路上的转变，远比单纯提升运维技术更有价值、专栏的构成等内容。</li></ul>
<h2 id="articleHeader6">前端之巅</h2>
<p>「前端之巅」是 InfoQ 旗下关注前端技术的垂直社群，加入前端之巅学习群请关注「前端之巅」公众号后回复 “ 加群 ”。投稿请发邮件到 editors@cn.infoq.com，注明 “ 前端之巅投稿 ”。</p>
<p><span class="img-wrap"><img data-src="/img/remote/1460000008850035" src="https://static.alili.tech/img/remote/1460000008850035" alt="前端之巅微信底图－5.jpg" title="前端之巅微信底图－5.jpg" style="cursor: pointer; display: inline;"></span></p>

                
{{< /raw >}}

# 版权声明
本文资源来源互联网，仅供学习研究使用，版权归该资源的合法拥有者所有，

本文仅用于学习、研究和交流目的。转载请注明出处、完整链接以及原作者。

原作者若认为本站侵犯了您的版权，请联系我们，我们会立即删除！

## 原文标题
前端每周清单第 44 期: 2017 JS 调查报告、REST 接口实时化、ESM 的过去与未来

## 原文链接
[https://segmentfault.com/a/1190000012476499](https://segmentfault.com/a/1190000012476499)

