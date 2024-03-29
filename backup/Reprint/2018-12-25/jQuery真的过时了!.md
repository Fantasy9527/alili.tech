---
title: 'jQuery真的过时了!' 
date: 2018-12-25 2:30:11
hidden: true
slug: gn279t4jyn7
categories: [reprint]
---

{{< raw >}}

                    
<h4>前言</h4>
<p>前几天知乎上有一个问题：jQuery真的过时了吗？我的答案是：jQuery确实过时了！感觉这个话题挺有趣，那咱们展开了聊聊。详细地说一下jQuery为什么过时了。</p>
<h4>一、jQuery解决了哪些痛点</h4>
<p>讨论一个库或者框架是否过时，应该先看看它解决了哪些问题，这些问题现在是否有更好的解决方案。</p>
<p>jQuery解决了哪些问题呢？</p>
<p><strong>浏览器兼容问题</strong></p>
<p>IE678横行的年代，浏览器兼容问题是前端小伙伴们必须掌握的技能，IE6有哪些bug，这得倒背如流。IE不识别哪些标准的JavaScript方法和对象，这也得记住，写个ajax，别人都用XMLHttpRequest,到IE就得用ActiveXObject。苦逼的前端程序员不能专心研究技术，只能天天为浏览器厂商擦屁股。直到jQuery的出现，大家解脱了，DOM操作也好,事件绑定也好，ajax也好，jQuery为我们封装了兼容各个浏览器的方法，感觉整个世界都和平了。</p>
<p><strong>选择器</strong></p>
<p>没有jQuery，我们要用getElementById、getElementsByTagName这些方法获取DOM对象。为列表所有元素绑定事件，要么事件委托，要么遍历所有元素。为了搞定操蛋的DOM接口，我们掌握了各种奇淫技巧，其实毛用没有。有了jQuery,我们可以用css选择器获取元素，绑定事件也不在需要遍历元素列表了，整个人都清爽了。</p>
<p><strong>动画效果</strong></p>
<p>我清晰地记得第一次用计时器写动画，如何让一个元素动起来，再如何让它停止运动。调试完，无bug，这就用了一堆代码了，更不用说做一个完整的页面效果，想想就让人崩溃。在看看jQuery为我们提供的动画效果，简直不敢想象没有jQuery，我们如何在IE678里面实现我们想要的效果。</p>
<p><strong>总结</strong></p>
<p>jQuery解决的痛点还远不止这些：它的DOM操作，样式操作，属性操作，事件绑定，还有遍历，表单序列化，ajax封装，这些功能给前端开发带入了一个崭新的世界。一直到今天，jQuery仍然是被前端开发者使用最多的库，没有之一！</p>
<h4>二、替代jQuery的解决方案</h4>
<p>jQuery给前端带来的影响是空前的，但是随着前端的发展，jQuery也进入了一个过时的状态，它为我们解决的痛点都已经有了替代方案。</p>
<p><strong>浏览器兼容</strong></p>
<p>浏览器的兼容问题越来越不是问题，IE6可以说已经被淘汰了（目前只有大型国企、事业单位、机关部门还有医院中的XP系统还保留着IE6，但是也开始逐步淘汰中）。天猫去年已经宣布不再支持IE8。虽然浏览器兼容问题仍然存在，但是已经不是当年那个坑翻天的时代了。</p>
<p><strong>选择器</strong></p>
<p>CSS3新增了大量选择器，操作样式，想怎么找就怎么找，不必麻烦jQuery。</p>
<p>原生的JavaScript也新增了querySelector和querySelectorAll方法，可以直接通过css选择器获取元素。</p>
<p><strong>动画效果</strong></p>
<p>css3提供了丰富的过渡和动画效果，让我们不再依赖jQuery。</p>
<p><strong>ajax</strong></p>
<p>fetch和axios这些第三方模块已经将ajax封装得相当出色了，我们再也不用为了一个$.ajax就引入jQuery。</p>
<p><strong>DOM操作和事件绑定</strong></p>
<p>抛开动画的问题，剩下主要就是数据的增删改，这种操作用jQuery，不管从性能的角度，还是易于开发和维护的角度来看，mvvm框架都要超jQuery很多。</p>
<p><strong>组件化和模块化</strong></p>
<p>组件化和模块化的开发是现在前端开发的主流，优点简单的说就是易于开发、易于维护、易于团队分工。</p>
<p>jQuery是可以组件化开发的，但是用jQuery写组件，就用两个字形容：蛋疼，谁用谁知道。</p>
<p>模块化不管是用ES2015也好（import,export），用webpack也好（require,module.exports），再不行，咱们复古有点用require.js或者sea.js,这显然和jQuery都没什么关系</p>
<p><strong>综上所述</strong></p>
<p>jQuery所有的优点都有更优秀的解决方案，可以肯定的说，jQuery已经过时了！</p>
<p>（如果贵公司要求兼容IE678,那就另当别论了。）</p>
<h4>三、jQuery并没有被淘汰</h4>
<p>jQuery虽然已经过时了，但是并没有被淘汰，而且近几年也不会。</p>
<p>现在市面上的大部分网站和应用还是基于jQuery，在此后的几年，他们仍然需要用jQuery维护。<br>很多公司没有专职的前端开发，他们的前端工作由后台负责，这些人更喜欢用jQuery加后台模板的工作模式。<br>部分两三年以上工作经验的前端开发，他们安于现状，排斥css3新特性，也没耐心学习mvvm框架，jQuery仍是他们的主要工具。<br>IE678并没有消失，所以jQuery仍有用武之地。<br>jQuery易于上手，仍然很适合做一些简单的网站。</p>
<p>综上所述，jQuery并没有被淘汰，仍然是新人必须技能之一。</p>
<h4>四、总结</h4>
<p>jQuery虽然过时了，但是jQuery仍然是前端开发的必备技能，对于一个前端新人，这个坑一定得踩。</p>
<h4>五、尾声</h4>
<p>如果您觉得有收获，请不要吝惜一个小小的【赞】，如果喜欢类似的文章，可以关注微信公众号：【晓舟报告】，第一时间获取文章。</p>

                
{{< /raw >}}

# 版权声明
本文资源来源互联网，仅供学习研究使用，版权归该资源的合法拥有者所有，

本文仅用于学习、研究和交流目的。转载请注明出处、完整链接以及原作者。

原作者若认为本站侵犯了您的版权，请联系我们，我们会立即删除！

## 原文标题
jQuery真的过时了!

## 原文链接
[https://segmentfault.com/a/1190000012110582](https://segmentfault.com/a/1190000012110582)

