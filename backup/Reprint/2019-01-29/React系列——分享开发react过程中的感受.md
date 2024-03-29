---
title: 'React系列——分享开发react过程中的感受' 
date: 2019-01-29 2:30:10
hidden: true
slug: qd89oacc06
categories: [reprint]
---

{{< raw >}}

                    
<p>react在中国大地火热已经有2年多了，积累了一大批粉丝，还有很多后来者或许在犹豫要不要学react，网上到处都是react、vue、angular的对比，还有某些大公司开源的组件化前端框架。</p>
<p>然而，最近阿当大师（曾经的前端大牛），是的，曾经，我没有活在他那个权威的时代，相信你们之中很多人也是，如果不是因为前端界的撕逼大战，新人前端几乎都不认识他，我特意问了下公司的其他技术同事，个个都懵逼，这人是谁啊？</p>
<p>阿当的观点中，是明确反对前端学习者从框架入手学习，尤其是react这么“笨重”的框架，当然，还有很多工程化的框架都被他损了一遍。大师提了一个观点：要从基础入手，学习html、css、js，把这些基础打牢固。</p>
<p>其实他的说法无可厚非，在他那个前端地位没那么高的年代，前端没有站在历史的前台，没有那么多开源框架可以用，所以靠自己的双手造了很多轮子，这样就需要很扎实的基本功。</p>
<p>那么，为什么react闲谈会说到阿当呢？如果你看过他的2016年度前端总结的文章，就知道react被喷的有多惨。</p>
<p>好了，撕逼的事情交给高层去解决，当你看了这些反对react的观点之后还有学习react的激情的话，那么，欢迎继续往下看。</p>
<p><strong>学习react需要什么基础？如何学习react？自学多久才能上手项目？</strong></p>
<p>新手一般都有这样的3个疑问，包括我自己也是，在没开始学习react之前，总是看到社区的人说react好难啊，要配置这个，要配置那个，github上找的demo又看不懂，这是Facebook大神开发出来给大神用的框架吧？</p>
<p><strong>1、关于基础：</strong><br>   新手一般都是基础很差，可能你现在只会写几个html和css，做一些简单的静态页面，可能你有jQuery基础，会用基本的jq函数，再好一点基础的就是js的基本常识懂一些，这应该是大部分react入门者的水平。</p>
<p>这其实说的就是我自己，在我知道有react这个单词之前，我就是只懂上面列举的一些前端技术，你要是那时候问我ajax怎么写，我会百度搜了告诉你。对，那时候我只会用百度。。</p>
<p>除非你的基础比那时候的我还差很多，可以考虑先学学基本功，否则，就可以直接学习react了。你可能会激动的反驳我：“还有一堆react的技术栈要学，没有基本功我学毛线啊！”</p>
<p>别急，下面会告诉你react开发是多么容易。</p>
<p><strong>2、关于学习方式：</strong></p>
<p>有的同学会看网上的一些react培训视频，或者去看看w3c的react教程，大部分的人会去react社区看教程，下载demo来研究。这些做法都很好，不过我都没有去认真看过。视频是绝对没兴趣看的了，在遇到bug的时候可能会去社区找找答案。</p>
<p>反对者：“你不看教程写毛线react代码啊！你会写吗？不是只会吹水吧？”</p>
<p>我学习react的方式可能跟绝大部分前端不一样，那是由我架构的第一个react项目，完全零基础，接手项目的第一天，连react都不会拼写（这个一点也不好笑）。第一天逛了下react官网，没看懂一句话，就知道有个jsx的鬼东西。到了研究的第二天，再次发现不对劲啊，怎么多了那么多与react相关的技术，flux、redux（单向数据流的概念）、对了，还需要啥打包工具。。。还有很多。</p>
<p>第三天，我思考了很多，然后找了一个切入点，从redux入手，为什么选择redux，而不是react本身呢？这里主要考虑的是前端工程化的问题，网上都在吹redux是react最好用的脚手架，那么我干脆先用redux搭建好框架，然后再去添加react组件。一个项目做出了最终是要进行维护和迭代更新的，如果一开始不架构好前端工程，那么后期重构的工作量就太大了，我虽然基础不好，但是这点常识还是了解一些，当然也离不开CTO的指点。</p>
<p>redux框架从哪里来呢？redux教程网站并没有告诉大家该如何搭建redux-react开发框架，我们有万能的github，上去找找吧，但不是star越多越好，因为对于新手来说，并不需要太强大的功能，只需要能够提供redux数据流的管理方案即可。这个框架也不是我去找的，而是CTO找好给我的，所以可以看出，又一个大牛在身边指点是多么快速学习。</p>
<p>redux并不比react简单啊，单向数据流的概念什么鬼，乱七八糟的，多了这么多概念，action、reducer、store、component等。即使网上有现成的redux-react框架，自己下载下来仍旧是不知道从哪个文件开始入手啊，可是设计师已经把一堆图丢过来了。</p>
<p>含着泪开始动手：“老大，这个。。该怎么写，你能写几行给我参考吗？”</p>
<p>老大：“框架不是有几个demo吗？自己看看去。”</p>
<p>之后，我学啊，学啊，硬是模仿着demo写出来了一个在现在看来乱七八糟的组件，还有action、reducer，虽然看不懂action是什么、reducer又是什么，但是只知道照着其他组件的数据管理方式照抄就对了。说到这里，又一个痛点就是，通篇都是用ES6的语法来写，然后我又懵逼了。</p>
<p>“老大，这里一个箭头表示啥意思，是C语言里面的指针吗？map函数怎么用啊？react生命周期又要怎么写啊？。。。”</p>
<p>然后老大就给我发了几条网址，其中就有著名的阮老师的ES6教程，还有react的官网教程。</p>
<p>转眼就过了2个月，view层组件竟然被我写出来了，怎么写出来的？2个月就能把这么多技术栈学完然后应用到项目上？扯淡吧。</p>
<p>然而我并没有学懂这些所谓的技术栈，比如最基本的箭头函数，我只会用，但是不理解为什么。</p>
<p>好了，关于从这个项目学习react的步骤就跳过了，后面的交互都是老大实现的，写的比我溜多了。</p>
<p>这个时候我算是半吊子的架构师了吧，比初级还初级的那种，转眼就毕业了，感谢老大的指导，理想和现实，我选择了理想，去了远方。</p>
<p>有句话说的好，一入react深似海，我发现除了react，我其他啥都不会了，咋办，还是找家用react技术的公司去锻炼吧。。故事讲完了，也就到了现在的我，抽时间写写react学习心得。</p>
<p>我的个人建议是，react是前端工程化思想的结晶，学习react，不能只从简单的demo学起，而是一开始就要培养工程化构建项目的思维。有的学习者缺少工程化的思维，这也就是为什么很多学习react的人会觉得react好难学啊，一个简单的弹窗组件都要搞那么复杂的数据流，jQuery一个show(),hide()不就搞定了。</p>
<p>现在很多公司都在推崇敏捷开发，因为市场竞争压力很大，开发一款产品的周期时间越来越短，迭代更新的速度也在加快，这也就是为什么现今的前端界那么多框架盛行的原因，因为框架可以帮助开发者节省很多时间。</p>
<p><strong>3、自学react多久才能上手项目？</strong></p>
<p>我觉得直接上手项目，这个过程中去自学，会有更大的体验和收获，首先一个项目可以给你带来压力，就会逼迫自己去主动学习，当然过程中放弃的人也很多。其次实践能让react概念更加刻骨铭心。</p>
<p>学渣们出来喷：“我就一个学渣，什么js概念都不会，react更看不懂了，不让我先学好基础，直接看react不是浪费时间吗？”</p>
<p>对此，分享几碗鸡汤<br>“没有尝试过，怎么知道自己不行”<br>“人总要有点梦想，万一实现了呢”</p>
<p><strong>4、总结：</strong></p>
<p>来，大家干了这碗鸡汤，要相信未来的路可以走的更远。</p>

                
{{< /raw >}}

# 版权声明
本文资源来源互联网，仅供学习研究使用，版权归该资源的合法拥有者所有，

本文仅用于学习、研究和交流目的。转载请注明出处、完整链接以及原作者。

原作者若认为本站侵犯了您的版权，请联系我们，我们会立即删除！

## 原文标题
React系列——分享开发react过程中的感受

## 原文链接
[https://segmentfault.com/a/1190000007926749](https://segmentfault.com/a/1190000007926749)

