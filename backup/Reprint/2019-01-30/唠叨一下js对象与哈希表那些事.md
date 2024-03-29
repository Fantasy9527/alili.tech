---
title: '唠叨一下js对象与哈希表那些事' 
date: 2019-01-30 2:30:23
hidden: true
slug: pmkr8f26r6
categories: [reprint]
---

{{< raw >}}

                    
<p><strong>最近在整理数据结构和算法相关的知识，小茄专门在github上开了个repo <a href="https://github.com/qieguo2016/algorithm" rel="nofollow noreferrer" target="_blank">https://github.com/qieguo2016...</a>，后续内容也会更新到这里，欢迎围观加星星！</strong></p>
<h2 id="articleHeader0">js对象</h2>
<p>js中的对象是基于哈希表结构的，而哈希表的查找时间复杂度为O(1)，所以很多人喜欢用对象来做映射，减少遍历循环。</p>
<p>比如常见的数组去重：</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="function arrayUnique(target) {
  var result = [target[0]];
  var temp = {};
  temp[target[0]] = true;
  for (var i = 1, targetLen = target.length; i < targetLen; i++) {
    if (typeof temp[target[i]] === 'undefined') {
      result.push(target[i]);
      temp[target[i]] = true;
    }
  }
  return result;
}" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="javascript hljs"><code class="javascript"><span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">arrayUnique</span>(<span class="hljs-params">target</span>) </span>{
  <span class="hljs-keyword">var</span> result = [target[<span class="hljs-number">0</span>]];
  <span class="hljs-keyword">var</span> temp = {};
  temp[target[<span class="hljs-number">0</span>]] = <span class="hljs-literal">true</span>;
  <span class="hljs-keyword">for</span> (<span class="hljs-keyword">var</span> i = <span class="hljs-number">1</span>, targetLen = target.length; i &lt; targetLen; i++) {
    <span class="hljs-keyword">if</span> (<span class="hljs-keyword">typeof</span> temp[target[i]] === <span class="hljs-string">'undefined'</span>) {
      result.push(target[i]);
      temp[target[i]] = <span class="hljs-literal">true</span>;
    }
  }
  <span class="hljs-keyword">return</span> result;
}</code></pre>
<p>这里使用了一个temp对象来保存出现过的元素，在循环中每次只需要判断当前元素是否在temp对象内即可判断出该元素是否已经出现过。</p>
<p>上面的代码看起来没有问题，但有点经验的同学可能会说了，假如目标数组是[1,'1'], 这是2个不同类型元素，所以我们的期望值应该是原样输出的。但结果却是[1]。<br>同理的还有true、null等，也就是说对象中的key在obj[key]时都被自动转成了字符串类型。<br>所以，如果要区分出不同的类型的话，temp里面的属性值就不能是一个简单的true了，而是要包含几种数据类型。比如可以是：</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="    temp[target[0]]={};
    temp[target[0]][(typeof temp[target[i]])] = 1;" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="javascript hljs"><code class="javascript">    temp[target[<span class="hljs-number">0</span>]]={};
    temp[target[<span class="hljs-number">0</span>]][(<span class="hljs-keyword">typeof</span> temp[target[i]])] = <span class="hljs-number">1</span>;</code></pre>
<p>在判断的时候除了要判断键是否存在之外，也要判断对应的数据类型计数是否大于1，以此来判断元素是否重复。</p>
<p>另外，上面的代码语法也有点问题，不知道你发现了没？<br>我们造的这个temp对象并不是完全空白，他是基于Object原型链继承而来的，所以自带了一个__proto__属性，如果你的目标数组里面恰好有"__proto__"这个值，返回的结果就有问题了，具体结果可以自己测试确认。解决方法有两种：</p>
<p>1） 想办法去掉这个磨人的__proto__。显然，我们需要去掉原型链，那么可以使用<code>Object.create(null)</code>的方式来创建一个完全空白、无原型的空对象。</p>
<p><del>2） 使用<code>!temp.hasOwnProperty(target[i])</code>代替<code>typeof temp[target[i]] === 'undefined'</code>，这时候代表原型链的__proto__属性就不能干扰到我们的结果判断了。</del>  感谢@天生爱走神的指正，<code>obj.hasOwnProperty(__proto__)</code>会得到false，但是假如我们的目标数组里面包含<code>__proto__</code>的话，就不能对<code>__proto__</code>进行去重了。</p>
<p>上面说了js中使用对象的一点小窍门，核心在于对象的<code>hashmap</code>结构，那<code>hashmap</code>是怎样的一个结构呢？且听小茄细细道来。</p>
<h2 id="articleHeader1">Hash Map</h2>
<p>在真实世界中，我们描述一个事物最常用的方式是使用<code>属性</code>-<code>值</code>（<code>key</code>-<code>value</code>）这样的键值对数据，面向对象编程中对象的定义和js中的对象都是这种模式。比如我们描述一个人是这样的：</p>
<p><span class="img-wrap"><img data-src="/img/bVGp74?w=262&amp;h=342" src="https://static.alili.tech/img/bVGp74?w=262&amp;h=342" alt="一个对象" title="一个对象" style="cursor: pointer; display: inline;"></span></p>
<h4>那在计算机中怎么保存这样的数据呢？</h4>
<p>计算机存储空间有两个属性：<code>存储地址</code>和所存储的<code>值</code>，机器可以根据给定的<code>存储地址</code>去读写该地址下的<code>值</code>。根据这种结构，假如我们将一块存储空间分成一个一个的格子，然后将这些数据依次塞到每个格子里，接下来我们就可以根据格子编号直接访问格子的内容了。这种方式就是数组（也叫线性连续表）：数组头保存整个数组储存空间的起始地址，不同下标代表不同的储存地址的偏移量，访问不同下标所对应的地址就能实现数组元素的读写。所以，很自然就会想到将上述的键值对数据的<code>key</code>映射成数组下标，接着读写数组就变成了读写<code>value</code>值。将<code>key</code>的字符串转换成代表下标数值比较简单，可以用特定的码表（如ASCII）进行转换。</p>
<p>上述小明的属性名（<code>key</code>）经过变换，可能就变成了这样：</p>
<p><span class="img-wrap"><img data-src="/img/bVGqaW?w=630&amp;h=402" src="https://static.alili.tech/img/bVGqaW?w=630&amp;h=402" alt="属性名转换" title="属性名转换" style="cursor: pointer;"></span></p>
<p>由于key的值不同长度不一，所以转换后的下标的值也相差巨大，另外<code>key</code>的个数不确定，也就意味着下标的个数也有很大的范围，甚至无穷多，就有了下面的问题：</p>
<h4>怎么将一组值相差范围巨大，个数波动范围很大的下标放入特定的数组空间呢？</h4>
<p>如果我们直接取下标值作为存储数组的下标，虽然简单，但是你会发现这个长度为10010的数组只存了8个值，太浪费！如果我们想要缩短数组的长度，比如缩为10，最简单的方式可以使用取模的方式来确定下标：<code>69 % 10 = 9，7 % 10 = 7, 198 % 10 = 8……</code>。这个取模就是<code>哈希算法</code>、也叫<code>散列算法</code>。</p>
<p>通过这样的方式得到的下标分别为<code>9、7、8、3、6、2、0</code>，可以得到保存小明数据的数组：</p>
<p><span class="img-wrap"><img data-src="/img/bVGqbI?w=586&amp;h=147" src="https://static.alili.tech/img/bVGqbI?w=586&amp;h=147" alt="图片描述" title="图片描述" style="cursor: pointer; display: inline;"></span></p>
<p>但是这种方式很容易出现重复，假如我们增加一个属性<code>phone</code>，对应的映射值是29，那么29跟69算出来的下标就重复了。也就是哈希算法中的<code>冲突</code>，也叫<code>碰撞</code>。好的哈希算法能极大减少<code>冲突</code>，但由于输入几乎是无穷的，而输出却要求在有限的空间内，所以<code>冲突</code>是不可避免的。</p>
<h4>那如何处理冲突呢？</h4>
<p>还是上面这个例子，29和69发生了冲突，但是我们可以将他们组成一个链表，链表的头部放在数组中，得到。链表结构中，每个元素（除单向链表的尾部）都包含了相连元素的内存地址和本身的值，上文中的冲突放入一个链表中，可以得到这样的结构：</p>
<p><span class="img-wrap"><img data-src="/img/bVGqdH?w=737&amp;h=469" src="https://static.alili.tech/img/bVGqdH?w=737&amp;h=469" alt="图片描述" title="图片描述" style="cursor: pointer; display: inline;"></span></p>
<p>最终得到的这个数据结构，也就是我们常说<code>哈希表</code>了。这种将数组与链表结合生成哈希表的方法，叫<code>拉链法</code>。</p>
<h4>哈希表数据的查找</h4>
<p>比如想知道小明的<code>name</code>属性，即<code>小明.name</code>。流程是这样的：</p>
<p>1）根据字符映射关系得到映射值为69<br>2）使用哈希算法得到下标 index=hash(69)=9<br>3）遍历数组中下标为9的链表，链表的第一个元素的<code>key</code>刚好就是我们要找的<code>name</code>，所以返回<code>value</code>值<code>小明</code></p>
<p>哈希表中增删一个元素并不会影响到其他的元素，不像数组一样需要改变后面所有的元素下标。在拉链式的哈希表中，属性的增删就是链表的增删，非常方便。而在纯数组形式的哈希表中，对属性的删并不是真的删除，而是做一个空标志而已，所以不影响其他元素。</p>
<h2 id="articleHeader2">Hash Map的扩展知识</h2>
<p>对于哈希表来说，最重要的莫过于生成哈希串的哈希算法和处理冲突的策略了。下面进行简单的介绍。</p>
<h3 id="articleHeader3">哈希算法（散列算法）</h3>
<p>根据上面的例子得知，哈希算法的目的就是将不定的输入转换成特定范围的输出，并且要求输出尽量均匀分布。由于散列算法是应用在每一次数据定位中的，它的使用频率非常的高，这意味着我们必须要选择简单的算法。散列算法有很多，这里简单介绍几种。</p>
<p>1，除法散列法<br>最直观的一种，小茄上文使用的就是这种散列法，公式：<br>index = key % 16</p>
<p>2，平方散列法<br>求<code>index</code>是非常频繁的操作，而乘法的运算要比除法来得省时（对现在的CPU来说，估计我们感觉不出来），所以我们考虑把除法换成乘法和一个位移操作。公式：<br>index = (key * key) &gt;&gt; 28<br>如果数值分配比较均匀的话这种方法能得到不错的结果，另外<code>key</code>如果很大，<code>key</code> * <code>key</code>会发生溢出。但我们这个乘法不关心溢出，因为我们根本不是为了获取相乘结果，而是为了获取<code>index</code>。</p>
<p>3，斐波那契（Fibonacci）散列法<br>平方散列法的缺点是显而易见的，所以我们能不能找出一个理想的乘数，而不是拿value本身当作乘数呢？答案是肯定的。</p>
<p>1，对于16位整数而言，这个乘数是40503<br>2，对于32位整数而言，这个乘数是2654435769<br>3，对于64位整数而言，这个乘数是11400714819323198485</p>
<p>这几个“理想乘数”是如何得出来的呢？这跟一个法则有关，叫黄金分割法则，而描述黄金分割法则的最经典表达式无疑就是著名的斐波那契数列，即如此形式的序列：0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144,233, 377, 610， 987, 1597, 2584, 4181, 6765, 10946，…。</p>
<p>对我们常见的32位整数而言，公式：<br>index = (key* 2654435769) &gt;&gt; 28</p>
<h3 id="articleHeader4">处理冲突的策略</h3>
<p>上文介绍了拉链法来处理冲突，处理冲突的方法其实也有很多，下面简单介绍一下另外几种：</p>
<p>1）拉链法变种。由于链表的查找需要遍历，如果我们将链表换成树或者哈希表结构，那么就能大幅提高冲突元素的查找效率。不过这样做则会加大哈希表构造的复杂度，也就是构建哈希表时的效率会变差。</p>
<p>2）开放寻址： 当关键字<code>key</code>的哈希地址<code>p=H（key）</code>出现冲突时，以<code>p</code>为基础，产生另一个哈希地址<code>p1</code>，如果<code>p1</code>仍然冲突，再以<code>p</code>为基础，产生另一个哈希地址<code>p2</code>，…，直到找出一个不冲突的哈希地址<code>pi</code>，将相应元素存入其中。这种方法有一个通用的函数形式：</p>
<p>Hi=（H（key）+di）% m   i=1，2，…，n</p>
<p>根据<code>di</code>的不同，又可以分为线性的、平方的、随机数之类的。。。这里不再展开。</p>
<p>开发寻址的好处是存储空间更加紧凑，利用率高。但是这种方式让冲突元素之间产生了联系，在删除元素的时候，不能直接删除，否则就打乱了冲突元素的寻址链。</p>
<p>3）再哈希法</p>
<p>这种方法会预先定义一组哈希算法，发生冲突的时候，调用下一个哈希算法计算一直计算到不发生冲突的时候则插入元素，这种方法跟开放寻址的方法优缺点类似。函数表达式：</p>
<p>index=Hi（key）  ,  i=1，2，…，n</p>
<h3 id="articleHeader5">哈希相关的应用实践</h3>
<p>哈希算法常用的场景除了上文所说的快速查找之外，还有一个非常重要的应用就是加密算法，这个加密更准确的说法是加签，也即是“消息摘要”。</p>
<p>根据上文的基础介绍可知，哈希算法就是将任意数据转换成一定范围数据的算法，这种算法的副作用就是会产生冲突。但是呢，在快速查找中出现的副作用，却是加密功能中的核心，因为有冲突，所以从结果就无法逆推出输入值，这样就实现了数据的单向加密。而输入数据的变化却又会影响到哈希串的值，所以我们可以用哈希串来进行数据的校验。</p>
<p>关于js对象与哈希相关的东西就说到这里了，用文字总结一下之后发现很多知识点都明确了很多，尤其是要用最平白的语言组织出来，就必须有自己的理解才行。任何一个细节都可以看出很多东西，谨以此文与君共勉！</p>

                
{{< /raw >}}

# 版权声明
本文资源来源互联网，仅供学习研究使用，版权归该资源的合法拥有者所有，

本文仅用于学习、研究和交流目的。转载请注明出处、完整链接以及原作者。

原作者若认为本站侵犯了您的版权，请联系我们，我们会立即删除！

## 原文标题
唠叨一下js对象与哈希表那些事

## 原文链接
[https://segmentfault.com/a/1190000007692754](https://segmentfault.com/a/1190000007692754)

