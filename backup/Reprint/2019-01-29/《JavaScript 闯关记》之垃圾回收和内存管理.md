---
title: '《JavaScript 闯关记》之垃圾回收和内存管理' 
date: 2019-01-29 2:30:10
hidden: true
slug: c2tpjhydfm
categories: [reprint]
---

{{< raw >}}

                    
<p>JavaScript 具有自动垃圾收集机制（GC：Garbage Collecation），也就是说，执行环境会负责管理代码执行过程中使用的内存。而在 C 和 C++ 之类的语言中，开发人员的一项基本任务就是手工跟踪内存的使用情况，这是造成许多问题的一个根源。</p>
<p>在编写 JavaScript 程序时，开发人员不用再关心内存使用问题，所需内存的分配以及无用内存的回收完全实现了自动管理。这种垃圾收集机制的原理其实很简单：<strong>找出那些不再继续使用的变量，然后释放其占用的内存。</strong>为此，垃圾收集器会按照固定的时间间隔（或代码执行中预定的收集时间），周期性地执行这一操作。</p>
<p>正因为垃圾回收器的存在，许多人认为 JavaScript 不用太关心内存管理的问题，但如果不了解 JavaScript 的内存管理机制，我们同样非常容易成内存泄漏（内存无法被回收）的情况。</p>
<h2 id="articleHeader0">垃圾回收机制</h2>
<h3 id="articleHeader1">内存的分配场景</h3>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="// 1.对象
new Object(); 
new MyConstructor(); 
{ a: 4, b: 5 } 
Object.create(); " title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="javascript hljs"><code class="javascript"><span class="hljs-comment">// 1.对象</span>
<span class="hljs-keyword">new</span> <span class="hljs-built_in">Object</span>(); 
<span class="hljs-keyword">new</span> MyConstructor(); 
{ <span class="hljs-attr">a</span>: <span class="hljs-number">4</span>, <span class="hljs-attr">b</span>: <span class="hljs-number">5</span> } 
<span class="hljs-built_in">Object</span>.create(); </code></pre>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="// 2.数组 
new Array(); 
[ 1, 2, 3, 4 ]; " title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="javascript hljs"><code class="javascript"><span class="hljs-comment">// 2.数组 </span>
<span class="hljs-keyword">new</span> <span class="hljs-built_in">Array</span>(); 
[ <span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span>, <span class="hljs-number">4</span> ]; </code></pre>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="// 3.字符串，JavaScript 的字符串和 .NET 一样，使用资源池和 copy on write 方式管理字符串。
new String(&quot;hello hyddd&quot;); 
&quot;<p>&quot; + e.innerHTML + &quot;</p>&quot; " title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="javascript hljs"><code class="javascript"><span class="hljs-comment">// 3.字符串，JavaScript 的字符串和 .NET 一样，使用资源池和 copy on write 方式管理字符串。</span>
<span class="hljs-keyword">new</span> <span class="hljs-built_in">String</span>(<span class="hljs-string">"hello hyddd"</span>); 
<span class="hljs-string">"&lt;p&gt;"</span> + e.innerHTML + <span class="hljs-string">"&lt;/p&gt;"</span> </code></pre>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="// 4.函数
var x = function () { ... } 
new Function(code); " title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="javascript hljs"><code class="javascript"><span class="hljs-comment">// 4.函数</span>
<span class="hljs-keyword">var</span> x = <span class="hljs-function"><span class="hljs-keyword">function</span> (<span class="hljs-params"></span>) </span>{ ... } 
<span class="hljs-keyword">new</span> <span class="hljs-built_in">Function</span>(code); </code></pre>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="// 5.闭包 
function outer(name) {
     var x = name; 
     return function inner() { 
        return &quot;Hi, &quot; + name; 
     } 
 }" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="javascript hljs"><code class="javascript"><span class="hljs-comment">// 5.闭包 </span>
<span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">outer</span>(<span class="hljs-params">name</span>) </span>{
     <span class="hljs-keyword">var</span> x = name; 
     <span class="hljs-keyword">return</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">inner</span>(<span class="hljs-params"></span>) </span>{ 
        <span class="hljs-keyword">return</span> <span class="hljs-string">"Hi, "</span> + name; 
     } 
 }</code></pre>
<h3 id="articleHeader2">内存的生命周期</h3>
<p>下面我们来分析一下函数中局部变量的正常生命周期。</p>
<ul>
<li><p>内存分配：局部变量只在函数执行的过程中存在。而在这个过程中，会为局部变量在栈（或堆）内存上分配相应的空间，以便存储它们的值。</p></li>
<li><p>内存使用：然后在函数中使用这些变量，直至函数执行结束。</p></li>
<li><p>内存回收：此时，局部变量就没有存在的必要了，因此可以释放它们的内存以供将来使用。</p></li>
</ul>
<p>通常，很容易判断变量是否还有存在的必要，但并非所有情况下都这么容易就能得出结论（例如：使用闭包的时）。垃圾收集器必须跟踪哪个变量有用哪个变量没用，对于不再有用的变量打上标记，以备将来收回其占用的内存。用于标识无用变量的策略可能会因实现而异，但具体到浏览器中的实现，则通常有两个策略：<strong>标记清除</strong> 和 <strong>引用计数</strong>。</p>
<h3 id="articleHeader3">标记清除</h3>
<p>JavaScript 中最常用的垃圾收集方式是 <strong>标记清除</strong>（mark-and-sweep）。当变量进入环境（例如，在函数中声明一个变量）时，就将这个变量标记为“进入环境”。从逻辑上讲，永远不能释放进入环境的变量所占用的内存，因为只要执行流进入相应的环境，就可能会用到它们。而当变量离开环境时，则将其标记为“离开环境”。</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="function test(){ 
    var a = 10 ; // 被标记 ，进入环境 
    var b = 20 ; // 被标记 ，进入环境 
} 
test(); // 执行完毕 之后 a、b又被标离开环境，被回收。" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="javascript hljs"><code class="javascript"><span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">test</span>(<span class="hljs-params"></span>)</span>{ 
    <span class="hljs-keyword">var</span> a = <span class="hljs-number">10</span> ; <span class="hljs-comment">// 被标记 ，进入环境 </span>
    <span class="hljs-keyword">var</span> b = <span class="hljs-number">20</span> ; <span class="hljs-comment">// 被标记 ，进入环境 </span>
} 
test(); <span class="hljs-comment">// 执行完毕 之后 a、b又被标离开环境，被回收。</span></code></pre>
<p>垃圾回收器在运行的时候会给存储在内存中的所有变量都加上标记（当然，可以使用任何标记方式）。然后，它会去掉环境中的变量以及被环境中的变量引用的变量的标记（例如，闭包）。而在此之后再被加上标记的变量将被视为准备删除的变量，原因是环境中的变量已经无法访问到这些变量了。最后，垃圾回收器完成内存清除工作，销毁那些带标记的值并回收它们所占用的内存空间。</p>
<p>这种方式的主要缺点就是如果某些对象被清理后，内存是不连续的，那么就算内存占用率不高，例如只有50%，但是由于内存空隙太多，后来的大对象甚至无法存储到内存之中。一般的处理方式都是在垃圾回收后进行整理操作，这种方法也叫 <strong>标记整理</strong>，整理的过程就是将不连续的内存向一端复制，使不连续的内存连续起来。</p>
<p>目前，IE9+、Firefox、Opera、Chrome 和 Safari 的 JavaScript 实现使用的都是 <strong>标记清除</strong> 式的垃圾收集策略（或类似的策略），只不过垃圾收集的时间间隔互有不同。</p>
<h3 id="articleHeader4">引用计数</h3>
<p>另一种不太常见的垃圾收集策略叫做 <strong>引用计数</strong>（reference counting）。引用计数的含义是跟踪记录每个值被引用的次数。当声明了一个变量并将一个引用类型值赋给该变量时，则这个值的引用次数就是1。如果同一个值又被赋给另一个变量，则该值的引用次数加1。相反，如果包含对这个值引用的变量又取得了另外一个值，则这个值的引用次数减1。当这个值的引用次数变成0时，则说明没有办法再访问这个值了，因而就可以将其占用的内存空间回收回来。这样，当垃圾收集器下次再运行时，它就会释放那些引用次数为零的值所占用的内存。</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="function test(){ 
    var a = {} ; // a的引用次数为0 
    var b = a ; // a的引用次数加1，为1 
    var c = a; // a的引用次数再加1，为2 
    var b = {}; // a的引用次数减1，为1 
}" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="javascript hljs"><code class="javascript"><span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">test</span>(<span class="hljs-params"></span>)</span>{ 
    <span class="hljs-keyword">var</span> a = {} ; <span class="hljs-comment">// a的引用次数为0 </span>
    <span class="hljs-keyword">var</span> b = a ; <span class="hljs-comment">// a的引用次数加1，为1 </span>
    <span class="hljs-keyword">var</span> c = a; <span class="hljs-comment">// a的引用次数再加1，为2 </span>
    <span class="hljs-keyword">var</span> b = {}; <span class="hljs-comment">// a的引用次数减1，为1 </span>
}</code></pre>
<p>早期很多浏览器使用引用计数策略，但很快它就遇到了一个严重的问题：<strong>循环引用</strong>。循环引用指的是对象 A 中包含一个指向对象 B 的指针，而对象 B 中也包含一个指向对象 A 的引用。请看下面这个例子：</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="function problem(){
    var objectA = new Object();
    var objectB = new Object();

    objectA.someOtherObject = objectB;
    objectB.anotherObject = objectA;
}" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="javascript hljs"><code class="javascript"><span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">problem</span>(<span class="hljs-params"></span>)</span>{
    <span class="hljs-keyword">var</span> objectA = <span class="hljs-keyword">new</span> <span class="hljs-built_in">Object</span>();
    <span class="hljs-keyword">var</span> objectB = <span class="hljs-keyword">new</span> <span class="hljs-built_in">Object</span>();

    objectA.someOtherObject = objectB;
    objectB.anotherObject = objectA;
}</code></pre>
<p>在这个例子中，objectA 和 objectB 通过各自的属性相互引用；也就是说，这两个对象的引用次数都是2。在采用 <strong>标记清除</strong> 策略的实现中，由于函数执行之后，这两个对象都离开了作用域，因此这种相互引用不是个问题。但在采用 <strong>引用计数</strong> 策略的实现中，当函数执行完毕后，objectA 和 objectB 还将继续存在，因为它们的引用次数永远不会是0。假如这个函数被重复多次调用，就会导致大量内存得不到回收。为此，新一代浏览器都放弃了引用计数方式，转而采用标记清除来实现其垃圾收集机制。可是，引用计数导致的麻烦并未就此终结。</p>
<p>我们知道，IE 中有一部分对象并不是原生 JavaScript 对象。例如，其 BOM 和 DOM 中的对象就是使用 C++ 以 COM（Component Object Model，组件对象模型）对象的形式实现的，而 COM 对象的垃圾收集机制采用的就是引用计数策略。因此，即使 IE 的 JavaScript 引擎是使用标记清除策略来实现的，但 JavaScript 访问的 COM 对象依然是基于引用计数策略的。换句话说，只要在 IE 中涉及 COM 对象，就会存在循环引用的问题。下面这个简单的例子，展示了使用 COM 对象导致的循环引用问题：</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="var element = document.getElementById(&quot;some_element&quot;);
var myObject = new Object();
myObject.element = element;
element.someObject = myObject;" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="javascript hljs"><code class="javascript"><span class="hljs-keyword">var</span> element = <span class="hljs-built_in">document</span>.getElementById(<span class="hljs-string">"some_element"</span>);
<span class="hljs-keyword">var</span> myObject = <span class="hljs-keyword">new</span> <span class="hljs-built_in">Object</span>();
myObject.element = element;
element.someObject = myObject;</code></pre>
<p>这个例子在一个 DOM 元素（element）与一个原生 JavaScript 对象（myObject）之间创建了循环引用。其中，变量 myObject 有一个名为 element 的属性指向 element 对象；而变量 element 也有一个属性名叫 someObject 回指 myObject。由于存在这个循环引用，即使将例子中的 DOM 从页面中移除，它也永远不会被回收。</p>
<p>为了避免类似这样的循环引用问题，最好是在不使用它们的时候手工断开原生 JavaScript 对象与 DOM 元素之间的连接。例如，可以使用下面的代码消除前面例子创建的循环引用：</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="myObject.element = null;
element.someObject = null;" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="javascript hljs"><code class="javascript">myObject.element = <span class="hljs-literal">null</span>;
element.someObject = <span class="hljs-literal">null</span>;</code></pre>
<p>将变量设置为 <code>null</code> 意味着切断变量与它此前引用的值之间的连接。当垃圾收集器下次运行时，就会删除这些值并回收它们占用的内存。</p>
<p>为了解决上述问题，IE9 把 BOM 和 DOM 对象都转换成了真正的 JavaScript 对象。这样，就避免了两种垃圾收集算法并存导致的问题，也消除了常见的内存泄漏现象。</p>
<h3 id="articleHeader5">IE6 的性能问题</h3>
<p>IE6 的垃圾回收是根据内存分配量运行的，当环境中存在256个变量、4096个对象、64k的字符串任意一种情况的时候就会触发垃圾回收器工作，看起来很科学，不用按一段时间就调用一次，有时候会没必要，这样按需调用不是很好吗？但是如果环境中就是有这么多变量等一直存在，现在脚本如此复杂，那么垃圾回收器会一直工作，这样浏览器就没法儿玩儿了。</p>
<p>微软在 IE7 中做了调整，触发条件不再是固定的，而是动态修改的，初始值和 IE6 相同，如果垃圾回收器回收的内存分配量低于程序占用内存的15%，说明大部分内存不可被回收，设的垃圾回收触发条件过于敏感，这时候把临界条件翻倍，如果回收的内存高于85%，说明大部分内存早就该清理了，这时候则将各种临界值重置回默认值。这一看似简单的调整，极大地提升了 IE7 在运行包含大量 JavaScript 的页面时的性能。</p>
<h3 id="articleHeader6">编码注意 - 解除引用</h3>
<p>使用具备垃圾收集机制的语言编写程序，开发人员一般不必操心内存管理的问题。但是，JavaScript 在进行内存管理及垃圾收集时面临的问题还是有点与众不同。其中最主要的一个问题，就是分配给 Web 浏览器的可用内存数量通常要比分配给桌面应用程序的少。这样做的目的主要是出于安全方面的考虑，目的是防止运行 JavaScript 的网页耗尽全部系统内存而导致系统崩溃。内存限制问题不仅会影响给变量分配内存，同时还会影响调用栈以及在一个线程中能够同时执行的语句数量。</p>
<p>因此，确保占用最少的内存可以让页面获得更好的性能。而优化内存占用的最佳方式，就是为执行中的代码只保存必要的数据。一旦数据不再有用，最好通过将其值设置为 <code>null</code> 来释放其引用——这个做法叫做 <strong>解除引用</strong>（dereferencing）。这一做法适用于大多数全局变量和全局对象的属性。局部变量会在它们离开执行环境时自动被解除引用，如下面这个例子所示：</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="function createPerson(name){
    var localPerson = new Object();
    localPerson.name = name;
    return localPerson;
}

var globalPerson = createPerson(&quot;Nicholas&quot;);

// 手工解除globalPerson的引用
globalPerson = null;" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="javascript hljs"><code class="javascript"><span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">createPerson</span>(<span class="hljs-params">name</span>)</span>{
    <span class="hljs-keyword">var</span> localPerson = <span class="hljs-keyword">new</span> <span class="hljs-built_in">Object</span>();
    localPerson.name = name;
    <span class="hljs-keyword">return</span> localPerson;
}

<span class="hljs-keyword">var</span> globalPerson = createPerson(<span class="hljs-string">"Nicholas"</span>);

<span class="hljs-comment">// 手工解除globalPerson的引用</span>
globalPerson = <span class="hljs-literal">null</span>;</code></pre>
<p>由于局部变量 <code>localPerson</code> 在 <code>createPerson()</code> 函数执行完毕后就离开了其执行环境，因此无需我们显式地去为它解除引用。但是对于全局变量 <code>globalPerson</code> 而言，则需要我们在不使用它的时候手工为它解除引用，这也正是上面例子中最后一行代码的目的。</p>
<p>不过，解除一个值的引用并不意味着自动回收该值所占用的内存。解除引用的真正作用是让值脱离执行环境，以便垃圾收集器下次运行时将其回收。</p>
<h2 id="articleHeader7">垃圾回收的优化策略</h2>
<p>和其他语言一样，JavaScript 的垃圾回收策略也无法避免一个问题：垃圾回收时，会停止响应其他操作，这是为了安全考虑。而 JavaScript 的垃圾回收在 100ms 甚至以上，对一般的应用还好，但对于 JavaScript 游戏和动画，这种对连贯性要求比较高的应用，就麻烦了。这就是新引擎需要优化的点：避免垃圾回收造成的长时间停止响应。</p>
<p>David 大叔主要介绍了2个优化方案，而这也是最主要的2个优化方案了：</p>
<h3 id="articleHeader8">分代回收（Generation GC）</h3>
<p>这个和 Java 回收策略思想是一致的。目的是通过区分「临时」与「持久」对象；多回收「临时对象区」（young generation），少回收「持久对象区」（tenured generation），减少每次需遍历的对象，从而减少每次GC的耗时。<strong>Chrome 浏览器所使用的 V8 引擎就是采用的分代回收策略。</strong>如图： </p>
<p><span class="img-wrap"><img data-src="/img/remote/1460000007962112?w=2716&amp;h=1486" src="https://static.alili.tech/img/remote/1460000007962112?w=2716&amp;h=1486" alt="" title="" style="cursor: pointer; display: inline;"></span></p>
<h3 id="articleHeader9">增量回收（Incremental GC）</h3>
<p>这个方案的思想很简单，就是「每次处理一点，下次再处理一点，如此类推」。这种方案，虽然耗时短，但中断较多，带来了上下文切换频繁的问题。<strong>Firefox 浏览器所使用的  JavaScript 引擎就是采用的增量回收策略。</strong>如图： </p>
<p><span class="img-wrap"><img data-src="/img/remote/1460000007962113?w=2716&amp;h=1484" src="https://static.alili.tech/img/remote/1460000007962113?w=2716&amp;h=1484" alt="" title="" style="cursor: pointer; display: inline;"></span></p>
<p>因为每种方案都其适用场景和缺点，因此在实际应用中，会根据实际情况选择方案。例如：如果大量对象都是长期「存活」，则分代处理优势也不大。</p>
<blockquote><p>原文链接：Know Your Engines: How to Make Your JavaScript Fast  <br><a href="http://conferences.oreilly.com/velocity/velocity2011/public/schedule/detail/18087" rel="nofollow noreferrer" target="_blank">http://t.cn/RIROY1W</a></p></blockquote>
<h3 id="articleHeader10">查看 Chrome 浏览器下的 CG 过程</h3>
<ol>
<li><p>使用快捷键 <code>F12</code> 或者 <code>Ctrl+Shift+J</code> 打开 Chrome 浏览器的「开发者工具」。</p></li>
<li><p>选择 <code>Timeline</code> 选项卡，在 <code>Capture</code> 选项中，只勾选 <code>Memory</code>。</p></li>
<li><p>设置完成后，点击最左边的 <code>Record</code> 按钮，然后就可以访问网页了。</p></li>
<li><p>打开一个网站，例如：<a href="http://www.taobao.com" rel="nofollow noreferrer" target="_blank">http://www.taobao.com</a>，当网页加载完成后，点击 <code>Stop</code>，等待分析结果。</p></li>
<li><p>然后在 <code>Chart View</code> 上寻找内存急速下降的部分，查看对应的 <code>Event Log</code>，可以从中找到 GC 的日志。</p></li>
</ol>
<p>具体过程如下图所示：<br><span class="img-wrap"><img data-src="/img/remote/1460000007962114?w=755&amp;h=691" src="https://static.alili.tech/img/remote/1460000007962114?w=755&amp;h=691" alt="" title="" style="cursor: pointer;"></span></p>
<h2 id="articleHeader11">关卡</h2>
<ul>
<li><p>挑战一，尝试写一段小程序，触发 IE6 的无限 CG。</p></li>
<li><p>挑战二，参考「查看 Chrome 浏览器下的 CG 过程」，尝试查看 Firefox  浏览器下的 CG 过程。</p></li>
</ul>
<h2 id="articleHeader12">更多</h2>
<blockquote><p>关注微信公众号「劼哥舍」回复「答案」，获取关卡详解。  <br>关注 <a href="https://github.com/stone0090/javascript-lessons" rel="nofollow noreferrer" target="_blank">https://github.com/stone0090/javascript-lessons</a>，获取最新动态。</p></blockquote>

                
{{< /raw >}}

# 版权声明
本文资源来源互联网，仅供学习研究使用，版权归该资源的合法拥有者所有，

本文仅用于学习、研究和交流目的。转载请注明出处、完整链接以及原作者。

原作者若认为本站侵犯了您的版权，请联系我们，我们会立即删除！

## 原文标题
《JavaScript 闯关记》之垃圾回收和内存管理

## 原文链接
[https://segmentfault.com/a/1190000007962109](https://segmentfault.com/a/1190000007962109)

