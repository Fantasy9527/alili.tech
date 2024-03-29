---
title: 'Electron，从玩玩具的心态开始，到打造出一款越来越优秀的桌面客户端产品' 
date: 2018-12-28 2:30:10
hidden: true
slug: 3pya4c8duhu
categories: [reprint]
---

{{< raw >}}

                    
<p>首发于<a href="https://webfe.kujiale.com/electron-quick-start/" rel="nofollow noreferrer" target="_blank">酷家乐前端博客</a></p>
<blockquote>
<p>标题是我以第一视角基于 Electron 开发客户端产品的体验，我将在之后分一系列文章向有兴趣的朋友一步一步介绍我是怎么从玩玩具的心态开始接触 Electron 到去开发客户端产品，最后随着业务和功能的复杂度提升再不断地优化客户端。</p>
<p>这是该系列的第一篇，我也是边学边做边反思，欢迎交流，哦，不用担心我会「太监」这个系列文章，因为我的老大握着40米大刀注视着我，不定期出下一篇（间隔最大不超3周）。<br>​</p>
</blockquote>
<p>​<br><span class="img-wrap"><img data-src="/img/remote/1460000011682575?w=2728&amp;h=1772" src="https://static.alili.tech/img/remote/1460000011682575?w=2728&amp;h=1772" alt="Electron" title="Electron" style="cursor: pointer; display: inline;"></span><br>​<br>// 以下是单口相声，从第一视角讲我是怎么接触到 Electron 直到要自己去用它开发应用的，不喜欢可以跳过<br>​<br>Electron，小名 Atom Shell，而 Atom 是 Github 发布的一款「现代化」的文本编辑器，很多人因为看到「Atom 华丽的编辑器截图」而入坑，又纷纷因为「Atom 三分钟憋不出一个P的流畅度」而弃坑，仰天感叹「封面果然都是骗人的」，我就是其中一个，这是第一次我和 Electron 接触，但那时候我还不认识她。<br>​<br>后来听说微软有一款编辑器叫「VS Code」贼好用，我就又去用了下，一样华丽的「封面」，然而却异常地好用和流畅，（这里我并不想挑起两大门派战争，只是个人体验实录），那是我和 Electron 的第二次接触，我依旧不认识她。<br>​<br>之后又在一些博客、专栏看到过诸多 Electron 的相关的文章，大致知道这是一个新的技术框架，可以让 web 前端去开发跨平台的桌面客户端，但也没深入，因为从字面上「用 Web 前端技术去开发一个桌面的客户端产品」去理解，脑中就是六个字「叔叔，我们不约」。<br>​<br>而3个月前，我的老大「尼古拉斯·飞」想让我一个前端去搞 Windows 客户端开发，和我提及技术框架是用 Electron，问我要不要试试。我才第一次去确认了这个单词的读音和意思，意思是「电子」，再去看下她的前世今生，之前叫 Atom Shell「原子的壳」，是 Github 开发 Atom 编辑器时配套开发的技术框架，合理了，「原子」不就是由「电子」和「原子核」组成么，可能哪天 Github 还会出一个「Nucleus」（原子核）的东西吧...<br>​<br>更进一步，我在<a href="https://electron.atom.io/" rel="nofollow noreferrer" target="_blank">Electron的官网</a>发现，很多基于 Electron 的大佬级产品，我都自己体验过，而那时的我并不知道这背后是 Electron。和我每天接触最频繁的当属「VS Code」，也是基于 Electron 的，我在公司 windows 电脑上用的 GitGUI「GitKraken」、我 Mac 上用的邮箱客户端 Nylas 等等都是基于 Electron 开发的。<br>​<br>于是，我和我老大说，“我要试试用 Electron 开发客户端产品”，那时候我确实就是觉得这个东西好玩好看（可以用 Web 前端写界面，一般都比较好看），额外说一句，Electron 本身这个项目也是抱着好玩的心态创造出来的。<br>​<br>我抱着把 Electron 当玩具的态度去浏览它的文档，看一些 Electron 的应用 Demo，再结合我们客户端应用的需求去看，我们可以从这些 Demo 中借鉴哪些思路，慢慢地，我就熟练学会了 Electron ...的拼写。<br>​<br>// 单口相声结束<br>​<br>所以，在这篇以及之后一系列的文章中，我将把自己在 Electron 方面的学习收获和应用心得记录下来，而在这第一篇文字里，我将「庸俗地」介绍如何「Quick Start Electron」，但不是 Hello World。我的「Quick Start」将依次介绍以下内容：<br>​</p>
<ul>
<li>是什么让 Electron 如此迷人（尤其是对于前端）</li>
<li>Electron 入门的几个阶段和对应要完成的事</li>
<li>该怎么去阅读和学习 Electron 的文档</li>
<li>该如何去挑选一些 Electron 的 Demo 源码来学习和实践</li>
<li>之后的该系列文章会涉及到哪些功能或主题（吊个胃口）<br>​</li>
</ul>
<p>本文不会具体说怎么起一个 Demo 去「Quick Start」，这完全可以在官网找到，也有很多文章写了，我只是就自己的经历去给出一些有限的建议，给有兴趣的朋友一个合理的入门参考，<strong>很多时候，当我们实践完「Hello world」后，我们就不知道怎么办了，于是热情就只能冰封在「Hello world」阶段了</strong>，下面这个笑话不是想要的结果。<br>​<br><img alt="四步开发VScode" title="四步开发VScode" src="https://static.alili.techundefined" style="cursor: pointer;"><br>​<br>​</p>
<h2 id="articleHeader0">一、是什么让 Electron 如此迷人？</h2>
<p>​<br>可能主要是因为，猿类里的亚种——前端开发——又有了新的出路了吧，还没找工作的前端开发，又有了新的岗位可以去选择，已经在岗的前端也有了新一年的 KPI/OKR，刚起步的创业公司可以只拉一个前端就能开发跨平台的多个桌面客户端... ...（开个玩笑）。<br>​</p>
<h3 id="articleHeader1">1. 可以用 Web 前端技术开发跨平台的桌面客户端</h3>
<p>​<br>这是 Electron 最迷人的地方，究其根本是因为它是建立在「Chromium」和「Node」之上的，一个负责界面，一个负责背后的逻辑，典型的「你负责貌美如花，我负责赚钱养家」，为什么 Electron 能够开发<strong>跨平台的桌面应用</strong>也就可以理解了。<br>​<br>而对于前端开发来说，「不要和老夫说什么 C++，Java，老夫行走江湖就一把 JS，遇到需求就是干」。前端开发可以用自己熟悉的方式去写应用界面，逻辑部分也还是 JS，如果你精通 Node 后端，那后端也可以插一脚，「鸟枪换大炮」，你开发客户端的能力有一种「忽如一夜春风来」的感觉。<br>​<br>但是，不同系统间还是会有很大的不同，「同一套代码，编译出跨平台的多个客户端」，话是这么说，但你得因为系统的不同做一些额外的处理，以使得打包出的不同系统下的应用都可以正常工作，这可能是一些「if - else」的成本，但相比于那80%都能完全复用的代码，这些成本已经很小了。<br>​<br>综上所述，一个 Web 前端开发者可以花很少的成本去上手 Electron，而相比于以前开发多平台客户端的成本，利用 ELectron 开发多平台客户端的成本是极低的。<br>​</p>
<h3 id="articleHeader2">2. 可以从 Node 的生态获得极大的助力</h3>
<p>​<br>因为 Electron 是基于 Node 的，意味着，Node 这个大生态下的模块，Electron 也都可以用，这减少了很多造轮子的时间，你要写一些逻辑将首先思考有没有成熟的模块可以引入，而不是自己吭哧吭哧闭门造车，自己造时间精力会大量得被消耗，上路还可能翻车。<br>​<br>Electron 从 Node 获益有2个方面，一个方面是如现代的 web 项目一般，开发构建流程可以引入很多成熟的包去打造出适合自己项目的开发工作流，另一个方面就是其应用本身也可以依赖需要的包去完成自己的功能，之后的文章我们会展开详细说。<br>​</p>
<h3 id="articleHeader3">3. 这个东西网站也可以为什么需要客户端？</h3>
<p>​<br>既然 Electron 是用 Web 技术写客户端，那么看上去 Electron 要做的事，可以搬到网站上，为什么还要搬到客户端，这里有3个角度的回答：<br>​</p>
<ul>
<li>
<strong>用户角度：</strong> 客户端是一款独立的软件，其综合体验一般都是比网站高的，尤其是涉及到「工具」范畴的应用，此外，特定的用户群体也会有类似的使用习惯</li>
<li>
<strong>发行方角度：</strong> 客户端是另一种产品形式，是一种产品的分发方式和入口，客户端可以实现很多本地应用独有的需求去触达用户，也能提供更加可靠的服务</li>
<li>
<strong>开发角度：</strong> 终于...不用考虑浏览器兼容了，Chromium 也足够开发使用一些先进的 CSS 或 JS 特性，我们现在还没计划引入 webpack 和 babel，因为现在好像够用，克制才是爱，除了写起来爽，对于开发来说，终于跳出了浏览器的沙盒，你可以自己去控制 Electron 中的「浏览器」，莫名的开心<br>​</li>
</ul>
<p>这些综合起来回答这个小节的问题就是，用 Electron 开发客户端，<strong>用户体验好，开发麻烦小，功能更强大，老板脱发少</strong>。<br>​</p>
<h3 id="articleHeader4">4. 那在 Electron 和 NW.js 之间，为啥选择前者？</h3>
<p>​<br>我没怎么用过 NW.js，但当时在没有时间深入体验的实际情况下，我选择生态好的。<br>​<br>Electron 相关的社区生态很好，之后的开发过程也证明了这一点，遇到的问题基本都能通过 Stackoverflow 找到，如果没有找到，新开一个问题或者去 Github 提 Issue 都可以得到较快的解决，除非是一些已知的问题或一些看文档可以解决的问题，这类问题可能会被忽略过去。<br>​<br>所以，生态更好一些，我选择了 Electron。 <br>​<br>​</p>
<h2 id="articleHeader5">二、Electron 入门可以有哪几个阶段？</h2>
<p>​<br>这一节从笔者目前有限的能力给出学习和使用 Electron 可能需要经历的几个阶段，里面提及的基础的内容可以去搜一下，有很多文章都解释了，比如什么是主进程，什么是渲染进程，而一些重要的实践是之后我会慢慢写的，比如开发打包构建发行工作流、自动更新和升级的实现等。<br>​<br><span class="img-wrap"><img data-src="/img/remote/1460000011682576?w=1024&amp;h=768" src="https://static.alili.tech/img/remote/1460000011682576?w=1024&amp;h=768" alt="Electron 入门阶段" title="Electron 入门阶段" style="cursor: pointer;"></span><br>​<br>​<br>​</p>
<h2 id="articleHeader6">三、该怎么去阅读和学习 Electron 的文档</h2>
<p>​<br>读文档，这回事其实是见仁见智的，如果你看文档有自己的三板斧，可以不用管这一节，不然还是建议看一下一些读和查 Electron 文档的建议，曾经我也「行尸走肉」般地看过 Electron 文档，然后发现这样效率极低，所以都是教训啊。<br>​</p>
<h3 id="articleHeader7">1. 看文档要结合一些别人写的博文或代码去看</h3>
<p>​<br>文档里你不理解的，可能很多开发者已经在自己的博客里展开解释了，所以你如果遇到看文档不理解的，可以搜一些文章，结合着看，这样你才会理解，尤其对于必须理解的基本概念，这非常重要。<br>​<br>甚至，你可能还要结合一些代码去看。<br>​</p>
<h3 id="articleHeader8">2. 第一遍浏览是要理解基本概念和看到 Electron 文档的组织方式</h3>
<p>​<br>第一遍浏览，快的一个下午，慢的2天，检查自己的成果就是搞定以下3个难题，你都能清楚地表达就可以了：<br>​</p>
<ul>
<li>能向不了解 Electron 前端开发解释清楚 Electron 这个技术框架的特性和优势</li>
<li>能清楚地解释什么是「主进程」，什么是「渲染进程」，各自负责什么，怎么通信的</li>
<li>大脑中可以清晰地浮现出 Electron 文档的组织方式（尤其是 API 文档部分，是分主/渲染进程的、模块化的 API 组织方式）<br>​</li>
</ul>
<h3 id="articleHeader9">3. 第二遍看主要是看各个 API 模块大致负责什么</h3>
<p>​<br>在第一遍的基础上，第二遍看 API 文档，脑中基本就有一定的框架了（别小看第一遍的浏览，这会大大提升你第二遍看的效率，也会提高以后你查文档的效率），这个时候看文档的目标就是让每个模块在脑中有一个印象。<br>​<br>这个印象包括，这个模块是在哪类进程中可用的、这个模块负责什么功能，有个印象就好。<br>​<br><strong>因为每个应用的不同，可能对于模块的重要程度排序也会不同，我仅给出我的建议，你可能需要好好看的一些模块，「好好看」的意思是，这些模块可能是很重要的，你要仔细地看</strong>：<br>​<br>只能在主进程使用的：<br>​</p>
<ul>
<li>
<code>app</code>：控制你整个 Electron 应用的生命周期</li>
<li>
<code>BrowserWindow</code>：创建和控制应用的窗口</li>
<li>
<code>ipcMain</code>：用于主进程中，和渲染进程通信的</li>
<li>
<code>webContents</code>：渲染和控制你窗口中的 web 内容的（因为 Electron 中，你是用 web 写的界面）<br>​</li>
</ul>
<p>可以在渲染进程使用的：<br>​</p>
<ul>
<li>
<code>ipcRenderer</code>：用于渲染进程中，和主进程通信的</li>
<li>
<code>remote</code>：可以方便你在渲染进程中直接调用主进程的方法</li>
<li>
<code>&lt;webview&gt; Tag</code>：可以载入外界的网页<br>​</li>
</ul>
<p>以上模块是我觉得是需要仔细学习的第一梯队模块，当然也说了，每个应用不尽相同，所以总体思路还是知道什么对自己重要，就重点看什么，至于其余的，要用到的时候去查就行。<br>​</p>
<h3 id="articleHeader10">4. 查文档</h3>
<p>​<br>只要你已经对 Electron 有了大致的认识，你就会查文档了，不会发生你要在渲染进程中使用的模块，但却直奔主进程中可用的模块那查半天。<br>​<br>有一个 Tip 是，如果你是明确可以定位的，那么你根据你的需求去查对应模块，如果你只有几个关键字，那么 Electron 提供把所有文档在一个页面内展示，这样你就可以用「搜索」了。<br>​<br>​</p>
<h2 id="articleHeader11">四、该如何去挑选一些 Electron 的 Demo 源码来学习和实践</h2>
<p>​<br>每过一段时间，总能看到一些文章「Electron + xxx 开发 what what what」，所以我们可以借鉴和学习的 Demo 是很多的，但是要去挑适合自己的并且友好的 Demo 来看，而看 Demo 代码也需要一定的方式。<br>​</p>
<h3 id="articleHeader12">1. 如何选择第二个你要学习的 Electron Demo</h3>
<p>​<br>怎么没有第一个？「Hello World」：你把我置于何地。<br>​</p>
<ul>
<li>挑一个复杂度不要太高的（package.json简单的一般不复杂）。这一点好理解，不解释。</li>
<li>不要选择一个代码写得很随意的，可能会带坏你。没注释，代码风格清奇，这样的还是算了吧。</li>
<li>不要去跟着写了很复杂的界面，但对 Electron 本身的使用没几行的 Demo，毕竟你不是来学 CSS 的。我真遇到过所谓用 Electron + XXX 开发的项目，写了一个花哨的界面，对 Electron 的使用依旧停在「Hello World」阶段。<br>​</li>
</ul>
<h3 id="articleHeader13">2. 如果找到了这样的 Electron 项目，How to play</h3>
<p>​</p>
<ul>
<li>看懂主要的代码逻辑，看不懂的查文档</li>
<li>自己去给它扩展一个功能，这里会遇到一些很有趣的事，比如你觉得应该简单的，结果你花了很久都没搞定，正常的，所以建议扩展一个和 Demo 中已有功能类似的功能，那样你可以直接参考了。<br>​</li>
</ul>
<h3 id="articleHeader14">3. 等有了自己要开发的应用后，再去看别人的 Demo 是看什么？</h3>
<p>​<br>直到目前，我还是会经常看一些其他的 Electron Demo 或者已可以称为产品的应用，这个时候我不可再事无巨细地去看整个应用的代码，而是明确知道我要去看什么。<br>​<br>举个栗子，在开发实际应用一段时间后，我觉得我们的 Electron 应用，从开发到打包，再到构建安装包，最后到发行，整个开发工作流效率低下，很不流畅，这个时候我就去看很多的项目，借鉴很多的优秀实践，感谢他们。在那个时候，我就会只看他们是怎么组织整个工程的，是怎么划分开发的各个阶段的，又是如何让整个流程流畅地自动化的。<br>​<br>到后来，需要实现「自动更新」功能的时候，我又去看一些实现了自动更新的应用和 Demo，看他们是怎么实现的，最终结合我们自己的需求实现了「分强弱的自动更新」功能。<br>​<br>所以<strong>在有了自己要开发的产品后，看 Electron 应用的代码，就是带着诉求去看的，这一步对优化自己的产品很重要</strong>。<br>​<br>​</p>
<h2 id="articleHeader15">五、之后的该系列文章会涉及到哪些功能或主题（吊个胃口）</h2>
<p>​<br>以上是我对于想入坑的「坑友」们分享的一些「入坑正确姿势」，姿势不对，容易折啊。可以看到这整篇文字没啥具体的实践，但作为一个开发了一段时间产品的前端，这是我认为很重要的一些方式方法。<br>​<br>在之后的一系列文章中，我将介绍以下内容（确定了一定会写的）：<br>​</p>
<ul>
<li>怎么打造自己的开发工作流：从源代码到一个安装程序，我们怎么自动化这个流程</li>
<li>自动更新：我们怎么考虑自动更新，为什么需要分强弱级别的自动更新，有什么区别，怎么实现</li>
<li>如何给自己的应用加入测试版功能，以提供一些方便测试的功能</li>
<li>怎么做客户端能做但浏览器为难的消息推送</li>
<li>... ...<br>​</li>
</ul>
<p>任重道远，欢迎交流，以上内容转载请标明原文链接和作者。</p>

                
{{< /raw >}}

# 版权声明
本文资源来源互联网，仅供学习研究使用，版权归该资源的合法拥有者所有，

本文仅用于学习、研究和交流目的。转载请注明出处、完整链接以及原作者。

原作者若认为本站侵犯了您的版权，请联系我们，我们会立即删除！

## 原文标题
Electron，从玩玩具的心态开始，到打造出一款越来越优秀的桌面客户端产品

## 原文链接
[https://segmentfault.com/a/1190000011699304](https://segmentfault.com/a/1190000011699304)

