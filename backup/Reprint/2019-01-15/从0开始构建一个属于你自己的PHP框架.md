---
title: '从0开始构建一个属于你自己的PHP框架' 
date: 2019-01-15 2:30:12
hidden: true
slug: iq6nuw5ngo
categories: [reprint]
---

{{< raw >}}

                    
<p><span class="img-wrap"><img data-src="/img/bVNg9F?w=500&amp;h=500" src="https://static.alili.tech/img/bVNg9F?w=500&amp;h=500" alt="图片描述" title="图片描述" style="cursor: pointer; display: inline;"></span></p>
<h1 id="articleHeader0">如何构建一个自己的PHP框架</h1>
<p>为什么我们要去构建一个自己的PHP框架？可能绝大多数的人都会说“市面上已经那么多的框架了，还造什么轮子？”。我的观点“造轮子不是目的，造轮子的过程中汲取到知识才是目的”。</p>
<p>那怎样才能构建一个自己的PHP框架呢？大致流程如下：</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="　　　　
入口文件　----> 注册自加载函数
        ----> 注册错误(和异常)处理函数
        ----> 加载配置文件
        ----> 请求
        ----> 路由　
        ---->（控制器 <----> 数据模型）
        ----> 响应
        ----> json
        ----> 视图渲染数据" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs haml"><code>　　　　
入口文件　----&gt; 注册自加载函数
        -<span class="ruby">---&gt; 注册错误(和异常)处理函数
</span>        -<span class="ruby">---&gt; 加载配置文件
</span>        -<span class="ruby">---&gt; 请求
</span>        -<span class="ruby">---&gt; 路由　
</span>        -<span class="ruby">---&gt;（控制器 &lt;----&gt; 数据模型）
</span>        -<span class="ruby">---&gt; 响应
</span>        -<span class="ruby">---&gt; json
</span>        -<span class="ruby">---&gt; 视图渲染数据</span></code></pre>
<p>除此之外我们还需要单元测试、nosql支持、接口文档支持、一些辅助脚本等。最终我的框架目录如下：</p>
<h1 id="articleHeader1">框架目录一览</h1>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="app                             [PHP应用目录]
├── demo                        [模块目录]
│   ├── controllers             [控制器目录]
│   │      └── Index.php        [默认控制器文件，输出json数据]
│   ├── logics                  [逻辑层，主要写业务逻辑的地方]
│   │   ├── exceptions          [异常目录]
│   │   ├── gateway          　　[一个逻辑层实现的gateway演示]
│   │   ├── tools               [工具类目录]
│   │   └── UserDefinedCase.php [注册框架加载到路由前的处理用例]
│   └── models                  [数据模型目录]
│       └── TestTable.php       [演示模型文件，定义一一对应的数据模型]
├── config                      [配置目录]
│    ├── demo                   [模块配置目录]
│    │   ├── config.php         [模块自定义配置]
│    │   └── route.php          [模块自定义路由]
│    ├── common.php             [公共配置]
│    ├── database.php           [数据库配置]
│    └── nosql.php              [nosql配置]
docs                            [接口文档目录]
├── apib                        [Api Blueprint]
│    └── demo.apib              [接口文档示例文件]
├── swagger                     [swagger]
framework                       [Easy PHP核心框架目录]
├── exceptions                  [异常目录]
│      ├── CoreHttpException.php[核心http异常]
├── handles                     [框架运行时挂载处理机制类目录]
│      ├── Handle.php           [处理机制接口]
│      ├── ErrorHandle.php      [错误处理机制类]
│      ├── ExceptionHandle.php  [未捕获异常处理机制类]
│      ├── ConfigHandle.php     [配置文件处理机制类]
│      ├── NosqlHandle.php      [nosql处理机制类]
│      ├── LogHandle.php        [log机制类]
│      ├── UserDefinedHandle.php[用户自定义处理机制类]
│      └── RouterHandle.php     [路由处理机制类]
├── orm                         [对象关系模型]
│      ├── Interpreter.php      [sql解析器]
│      ├── DB.php               [数据库操作类]
│      ├── Model.php            [数据模型基类]
│      └── db                   [数据库类目录]
│          └── Mysql.php        [mysql实体类]
├── nosql                       [nosql类目录]
│    ├── Memcahed.php           [Memcahed类文件]
│    ├── MongoDB.php            [MongoDB类文件]
│    └── Redis.php              [Redis类文件]
├── App.php                     [框架类]
├── Container.php               [服务容器]
├── Helper.php                  [框架助手类]
├── Load.php                    [自加载类]
├── Request.php                 [请求类]
├── Response.php                [响应类]
├── run.php                     [框架应用启用脚本]
frontend                        [前端源码和资源目录]
├── src                         [资源目录]
│    ├── components             [vue组件目录]
│    ├── views                  [vue视图目录]
│    ├── images                 [图片]
│    ├── ...
├── app.js                      [根js]
├── app.vue                     [根组件]
├── index.template.html         [前端入口文件模板]
├── store.js                    [vuex store文件]
public                          [公共资源目录，暴露到万维网]
├── dist                        [前端build之后的资源目录，build生成的目录，不是发布分支忽略该目录]
│    └── ...
├── index.html                  [前端入口文件,build生成的文件，不是发布分支忽略该文件]
├── index.php                   [后端入口文件]
runtime                         [临时目录]
├── logs                        [日志目录]
├── build                       [php打包生成phar文件目录]
tests                           [单元测试目录]
├── demo                        [模块名称]
│      └── DemoTest.php         [测试演示]
├── TestCase.php                [测试用例]
vendor                          [composer目录]
.git-hooks                      [git钩子目录]
├── pre-commit                  [git pre-commit预commit钩子示例文件]
├── commit-msg                  [git commit-msg示例文件]
.babelrc                        [babel配置文件]
.env                            [环境变量文件]
.gitignore                      [git忽略文件配置]
build                           [php打包脚本]
cli                             [框架cli模式运行脚本]
LICENSE                         [lincese文件]
logo.png                        [框架logo图片]
composer.json                   [composer配置文件]
composer.lock                   [composer lock文件]
package.json                    [前端依赖配置文件]
phpunit.xml                     [phpunit配置文件]
README-CN.md                    [中文版readme文件]
README.md                       [readme文件]
webpack.config.js               [webpack配置文件]
yarn.lock                       [yarn　lock文件]
" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs css"><code><span class="hljs-selector-tag">app</span>                             <span class="hljs-selector-attr">[PHP应用目录]</span>
├── <span class="hljs-selector-tag">demo</span>                        <span class="hljs-selector-attr">[模块目录]</span>
│   ├── <span class="hljs-selector-tag">controllers</span>             <span class="hljs-selector-attr">[控制器目录]</span>
│   │      └── <span class="hljs-selector-tag">Index</span><span class="hljs-selector-class">.php</span>        <span class="hljs-selector-attr">[默认控制器文件，输出json数据]</span>
│   ├── <span class="hljs-selector-tag">logics</span>                  <span class="hljs-selector-attr">[逻辑层，主要写业务逻辑的地方]</span>
│   │   ├── <span class="hljs-selector-tag">exceptions</span>          <span class="hljs-selector-attr">[异常目录]</span>
│   │   ├── <span class="hljs-selector-tag">gateway</span>          　　<span class="hljs-selector-attr">[一个逻辑层实现的gateway演示]</span>
│   │   ├── <span class="hljs-selector-tag">tools</span>               <span class="hljs-selector-attr">[工具类目录]</span>
│   │   └── <span class="hljs-selector-tag">UserDefinedCase</span><span class="hljs-selector-class">.php</span> <span class="hljs-selector-attr">[注册框架加载到路由前的处理用例]</span>
│   └── <span class="hljs-selector-tag">models</span>                  <span class="hljs-selector-attr">[数据模型目录]</span>
│       └── <span class="hljs-selector-tag">TestTable</span><span class="hljs-selector-class">.php</span>       <span class="hljs-selector-attr">[演示模型文件，定义一一对应的数据模型]</span>
├── <span class="hljs-selector-tag">config</span>                      <span class="hljs-selector-attr">[配置目录]</span>
│    ├── <span class="hljs-selector-tag">demo</span>                   <span class="hljs-selector-attr">[模块配置目录]</span>
│    │   ├── <span class="hljs-selector-tag">config</span><span class="hljs-selector-class">.php</span>         <span class="hljs-selector-attr">[模块自定义配置]</span>
│    │   └── <span class="hljs-selector-tag">route</span><span class="hljs-selector-class">.php</span>          <span class="hljs-selector-attr">[模块自定义路由]</span>
│    ├── <span class="hljs-selector-tag">common</span><span class="hljs-selector-class">.php</span>             <span class="hljs-selector-attr">[公共配置]</span>
│    ├── <span class="hljs-selector-tag">database</span><span class="hljs-selector-class">.php</span>           <span class="hljs-selector-attr">[数据库配置]</span>
│    └── <span class="hljs-selector-tag">nosql</span><span class="hljs-selector-class">.php</span>              <span class="hljs-selector-attr">[nosql配置]</span>
<span class="hljs-selector-tag">docs</span>                            <span class="hljs-selector-attr">[接口文档目录]</span>
├── <span class="hljs-selector-tag">apib</span>                        <span class="hljs-selector-attr">[Api Blueprint]</span>
│    └── <span class="hljs-selector-tag">demo</span><span class="hljs-selector-class">.apib</span>              <span class="hljs-selector-attr">[接口文档示例文件]</span>
├── <span class="hljs-selector-tag">swagger</span>                     <span class="hljs-selector-attr">[swagger]</span>
<span class="hljs-selector-tag">framework</span>                       <span class="hljs-selector-attr">[Easy PHP核心框架目录]</span>
├── <span class="hljs-selector-tag">exceptions</span>                  <span class="hljs-selector-attr">[异常目录]</span>
│      ├── <span class="hljs-selector-tag">CoreHttpException</span><span class="hljs-selector-class">.php</span><span class="hljs-selector-attr">[核心http异常]</span>
├── <span class="hljs-selector-tag">handles</span>                     <span class="hljs-selector-attr">[框架运行时挂载处理机制类目录]</span>
│      ├── <span class="hljs-selector-tag">Handle</span><span class="hljs-selector-class">.php</span>           <span class="hljs-selector-attr">[处理机制接口]</span>
│      ├── <span class="hljs-selector-tag">ErrorHandle</span><span class="hljs-selector-class">.php</span>      <span class="hljs-selector-attr">[错误处理机制类]</span>
│      ├── <span class="hljs-selector-tag">ExceptionHandle</span><span class="hljs-selector-class">.php</span>  <span class="hljs-selector-attr">[未捕获异常处理机制类]</span>
│      ├── <span class="hljs-selector-tag">ConfigHandle</span><span class="hljs-selector-class">.php</span>     <span class="hljs-selector-attr">[配置文件处理机制类]</span>
│      ├── <span class="hljs-selector-tag">NosqlHandle</span><span class="hljs-selector-class">.php</span>      <span class="hljs-selector-attr">[nosql处理机制类]</span>
│      ├── <span class="hljs-selector-tag">LogHandle</span><span class="hljs-selector-class">.php</span>        <span class="hljs-selector-attr">[log机制类]</span>
│      ├── <span class="hljs-selector-tag">UserDefinedHandle</span><span class="hljs-selector-class">.php</span><span class="hljs-selector-attr">[用户自定义处理机制类]</span>
│      └── <span class="hljs-selector-tag">RouterHandle</span><span class="hljs-selector-class">.php</span>     <span class="hljs-selector-attr">[路由处理机制类]</span>
├── <span class="hljs-selector-tag">orm</span>                         <span class="hljs-selector-attr">[对象关系模型]</span>
│      ├── <span class="hljs-selector-tag">Interpreter</span><span class="hljs-selector-class">.php</span>      <span class="hljs-selector-attr">[sql解析器]</span>
│      ├── <span class="hljs-selector-tag">DB</span><span class="hljs-selector-class">.php</span>               <span class="hljs-selector-attr">[数据库操作类]</span>
│      ├── <span class="hljs-selector-tag">Model</span><span class="hljs-selector-class">.php</span>            <span class="hljs-selector-attr">[数据模型基类]</span>
│      └── <span class="hljs-selector-tag">db</span>                   <span class="hljs-selector-attr">[数据库类目录]</span>
│          └── <span class="hljs-selector-tag">Mysql</span><span class="hljs-selector-class">.php</span>        <span class="hljs-selector-attr">[mysql实体类]</span>
├── <span class="hljs-selector-tag">nosql</span>                       <span class="hljs-selector-attr">[nosql类目录]</span>
│    ├── <span class="hljs-selector-tag">Memcahed</span><span class="hljs-selector-class">.php</span>           <span class="hljs-selector-attr">[Memcahed类文件]</span>
│    ├── <span class="hljs-selector-tag">MongoDB</span><span class="hljs-selector-class">.php</span>            <span class="hljs-selector-attr">[MongoDB类文件]</span>
│    └── <span class="hljs-selector-tag">Redis</span><span class="hljs-selector-class">.php</span>              <span class="hljs-selector-attr">[Redis类文件]</span>
├── <span class="hljs-selector-tag">App</span><span class="hljs-selector-class">.php</span>                     <span class="hljs-selector-attr">[框架类]</span>
├── <span class="hljs-selector-tag">Container</span><span class="hljs-selector-class">.php</span>               <span class="hljs-selector-attr">[服务容器]</span>
├── <span class="hljs-selector-tag">Helper</span><span class="hljs-selector-class">.php</span>                  <span class="hljs-selector-attr">[框架助手类]</span>
├── <span class="hljs-selector-tag">Load</span><span class="hljs-selector-class">.php</span>                    <span class="hljs-selector-attr">[自加载类]</span>
├── <span class="hljs-selector-tag">Request</span><span class="hljs-selector-class">.php</span>                 <span class="hljs-selector-attr">[请求类]</span>
├── <span class="hljs-selector-tag">Response</span><span class="hljs-selector-class">.php</span>                <span class="hljs-selector-attr">[响应类]</span>
├── <span class="hljs-selector-tag">run</span><span class="hljs-selector-class">.php</span>                     <span class="hljs-selector-attr">[框架应用启用脚本]</span>
<span class="hljs-selector-tag">frontend</span>                        <span class="hljs-selector-attr">[前端源码和资源目录]</span>
├── <span class="hljs-selector-tag">src</span>                         <span class="hljs-selector-attr">[资源目录]</span>
│    ├── <span class="hljs-selector-tag">components</span>             <span class="hljs-selector-attr">[vue组件目录]</span>
│    ├── <span class="hljs-selector-tag">views</span>                  <span class="hljs-selector-attr">[vue视图目录]</span>
│    ├── <span class="hljs-selector-tag">images</span>                 <span class="hljs-selector-attr">[图片]</span>
│    ├── ...
├── <span class="hljs-selector-tag">app</span><span class="hljs-selector-class">.js</span>                      <span class="hljs-selector-attr">[根js]</span>
├── <span class="hljs-selector-tag">app</span><span class="hljs-selector-class">.vue</span>                     <span class="hljs-selector-attr">[根组件]</span>
├── <span class="hljs-selector-tag">index</span><span class="hljs-selector-class">.template</span><span class="hljs-selector-class">.html</span>         <span class="hljs-selector-attr">[前端入口文件模板]</span>
├── <span class="hljs-selector-tag">store</span><span class="hljs-selector-class">.js</span>                    <span class="hljs-selector-attr">[vuex store文件]</span>
<span class="hljs-selector-tag">public</span>                          <span class="hljs-selector-attr">[公共资源目录，暴露到万维网]</span>
├── <span class="hljs-selector-tag">dist</span>                        <span class="hljs-selector-attr">[前端build之后的资源目录，build生成的目录，不是发布分支忽略该目录]</span>
│    └── ...
├── <span class="hljs-selector-tag">index</span><span class="hljs-selector-class">.html</span>                  <span class="hljs-selector-attr">[前端入口文件,build生成的文件，不是发布分支忽略该文件]</span>
├── <span class="hljs-selector-tag">index</span><span class="hljs-selector-class">.php</span>                   <span class="hljs-selector-attr">[后端入口文件]</span>
<span class="hljs-selector-tag">runtime</span>                         <span class="hljs-selector-attr">[临时目录]</span>
├── <span class="hljs-selector-tag">logs</span>                        <span class="hljs-selector-attr">[日志目录]</span>
├── <span class="hljs-selector-tag">build</span>                       <span class="hljs-selector-attr">[php打包生成phar文件目录]</span>
<span class="hljs-selector-tag">tests</span>                           <span class="hljs-selector-attr">[单元测试目录]</span>
├── <span class="hljs-selector-tag">demo</span>                        <span class="hljs-selector-attr">[模块名称]</span>
│      └── <span class="hljs-selector-tag">DemoTest</span><span class="hljs-selector-class">.php</span>         <span class="hljs-selector-attr">[测试演示]</span>
├── <span class="hljs-selector-tag">TestCase</span><span class="hljs-selector-class">.php</span>                <span class="hljs-selector-attr">[测试用例]</span>
<span class="hljs-selector-tag">vendor</span>                          <span class="hljs-selector-attr">[composer目录]</span>
<span class="hljs-selector-class">.git-hooks</span>                      <span class="hljs-selector-attr">[git钩子目录]</span>
├── <span class="hljs-selector-tag">pre-commit</span>                  <span class="hljs-selector-attr">[git pre-commit预commit钩子示例文件]</span>
├── <span class="hljs-selector-tag">commit-msg</span>                  <span class="hljs-selector-attr">[git commit-msg示例文件]</span>
<span class="hljs-selector-class">.babelrc</span>                        <span class="hljs-selector-attr">[babel配置文件]</span>
<span class="hljs-selector-class">.env</span>                            <span class="hljs-selector-attr">[环境变量文件]</span>
<span class="hljs-selector-class">.gitignore</span>                      <span class="hljs-selector-attr">[git忽略文件配置]</span>
<span class="hljs-selector-tag">build</span>                           <span class="hljs-selector-attr">[php打包脚本]</span>
<span class="hljs-selector-tag">cli</span>                             <span class="hljs-selector-attr">[框架cli模式运行脚本]</span>
<span class="hljs-selector-tag">LICENSE</span>                         <span class="hljs-selector-attr">[lincese文件]</span>
<span class="hljs-selector-tag">logo</span><span class="hljs-selector-class">.png</span>                        <span class="hljs-selector-attr">[框架logo图片]</span>
<span class="hljs-selector-tag">composer</span><span class="hljs-selector-class">.json</span>                   <span class="hljs-selector-attr">[composer配置文件]</span>
<span class="hljs-selector-tag">composer</span><span class="hljs-selector-class">.lock</span>                   <span class="hljs-selector-attr">[composer lock文件]</span>
<span class="hljs-selector-tag">package</span><span class="hljs-selector-class">.json</span>                    <span class="hljs-selector-attr">[前端依赖配置文件]</span>
<span class="hljs-selector-tag">phpunit</span><span class="hljs-selector-class">.xml</span>                     <span class="hljs-selector-attr">[phpunit配置文件]</span>
<span class="hljs-selector-tag">README-CN</span><span class="hljs-selector-class">.md</span>                    <span class="hljs-selector-attr">[中文版readme文件]</span>
<span class="hljs-selector-tag">README</span><span class="hljs-selector-class">.md</span>                       <span class="hljs-selector-attr">[readme文件]</span>
<span class="hljs-selector-tag">webpack</span><span class="hljs-selector-class">.config</span><span class="hljs-selector-class">.js</span>               <span class="hljs-selector-attr">[webpack配置文件]</span>
<span class="hljs-selector-tag">yarn</span><span class="hljs-selector-class">.lock</span>                       <span class="hljs-selector-attr">[yarn　lock文件]</span>
</code></pre>
<h1 id="articleHeader2">框架模块说明：</h1>
<h2 id="articleHeader3">入口文件</h2>
<p>定义一个统一的入口文件，对外提供统一的访问文件。对外隐藏了内部的复杂性，类似企业服务总线的思想。</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="// 载入框架运行文件
require('../framework/run.php');" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs javascript"><code><span class="hljs-comment">// 载入框架运行文件</span>
<span class="hljs-built_in">require</span>(<span class="hljs-string">'../framework/run.php'</span>);</code></pre>
<p><a href="https://github.com/TIGERB/easy-php/blob/master/public/index.php" rel="nofollow noreferrer" target="_blank">[file: public/index.php</a>]</p>
<h2 id="articleHeader4">自加载模块</h2>
<p>使用spl_autoload_register函数注册自加载函数到__autoload队列中，配合使用命名空间，当使用一个类的时候可以自动载入(require)类文件。注册完成自加载逻辑后，我们就可以使用use和配合命名空间申明对某个类文件的依赖。</p>
<p><a href="https://github.com/TIGERB/easy-php/blob/master/framework/Load.php" rel="nofollow noreferrer" target="_blank">[file: framework/Load.php</a>]</p>
<h2 id="articleHeader5">错误和异常模块</h2>
<p>脚本运行期间：</p>
<ul><li>错误:</li></ul>
<p>通过函数set_error_handler注册用户自定义错误处理方法，但是set_error_handler不能处理以下级别错误，E_ERROR、 E_PARSE、 E_CORE_ERROR、 E_CORE_WARNING、 E_COMPILE_ERROR、 E_COMPILE_WARNING，和在 调用 set_error_handler() 函数所在文件中产生的大多数 E_STRICT。所以我们需要使用register_shutdown_function配合error_get_last获取脚本终止执行的最后错误，目的是对于不同错误级别和致命错误进行自定义处理，例如返回友好的提示的错误信息。</p>
<p><a href="https://github.com/TIGERB/easy-php/blob/master/framework/handles/ErrorHandle.php" rel="nofollow noreferrer" target="_blank">[file: framework/hanles/ErrorHandle.php</a>]</p>
<ul><li>异常:</li></ul>
<p>通过函数set_exception_handler注册未捕获异常处理方法，目的捕获未捕获的异常，例如返回友好的提示和异常信息。</p>
<p><a href="https://github.com/TIGERB/easy-php/blob/master/framework/handles/ExceptionHandle.php" rel="nofollow noreferrer" target="_blank">[file: framework/hanles/ExceptionHandle.php</a>]</p>
<h2 id="articleHeader6">配置文件模块</h2>
<p>加载框架自定义和用户自定义的配置文件。</p>
<p><a href="https://github.com/TIGERB/easy-php/blob/master/framework/handles/ConfigHandle.php" rel="nofollow noreferrer" target="_blank">[file: framework/hanles/ConfigHandle.php</a>]</p>
<h2 id="articleHeader7">输入和输出</h2>
<ul>
<li>定义请求对象：包含所有的请求信息</li>
<li>定义响应对象：申明响应相关信息</li>
</ul>
<p>框架中所有的异常输出和控制器输出都是json格式，因为我认为在前后端完全分离的今天，这是很友善的，目前我们不需要再去考虑别的东西。</p>
<p><a href="https://github.com/TIGERB/easy-php/blob/master/framework/Request.php" rel="nofollow noreferrer" target="_blank">[file: framework/Request.php</a>]</p>
<p><a href="https://github.com/TIGERB/easy-php/blob/master/framework/Response.php" rel="nofollow noreferrer" target="_blank">[file: framework/Response.php</a>]</p>
<h2 id="articleHeader8">路由模块</h2>
<p>通过用户访问的url信息，通过路由规则执行目标控制器类的的成员方法。我在这里把路由大致分成了四类：</p>
<p><strong>传统路由</strong></p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="domain/index.php?module=Demo&amp;contoller=Index&amp;action=test&amp;username=test" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs fortran"><code style="word-break: break-word; white-space: initial;">domain/<span class="hljs-built_in">index</span>.php?<span class="hljs-keyword">module</span>=Demo&amp;contoller=<span class="hljs-built_in">Index</span>&amp;<span class="hljs-keyword">action</span>=test&amp;username=test</code></pre>
<p><strong>pathinfo路由</strong></p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="domain/demo/index/modelExample" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs maxima"><code style="word-break: break-word; white-space: initial;"><span class="hljs-built_in">domain</span>/<span class="hljs-built_in">demo</span>/index/modelExample</code></pre>
<p><strong>用户自定义路由</strong></p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="// 定义在config/moduleName/route.php文件中，这个的this指向RouterHandle实例
$this->get('v1/user/info', function (Framework\App $app) {
    return 'Hello Get Router';
});" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs php"><code><span class="hljs-comment">// 定义在config/moduleName/route.php文件中，这个的this指向RouterHandle实例</span>
<span class="hljs-keyword">$this</span>-&gt;get(<span class="hljs-string">'v1/user/info'</span>, <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-params">(Framework\App $app)</span> </span>{
    <span class="hljs-keyword">return</span> <span class="hljs-string">'Hello Get Router'</span>;
});</code></pre>
<p><strong>微单体路由</strong></p>
<p>我在这里详细说下这里所谓的微单体路由，面向SOA和微服务架构大行其道的今天，有很多的团队都在向服务化迈进，但是服务化过程中很多问题的复杂度都是指数级的增长，例如分布式的事务，服务部署，跨服务问题追踪等等。这导致对于小的团队从单体架构走向服务架构难免困难重重，所以有人提出来了微单体架构，按照我的理解就是在一个单体架构的SOA过程，我们把微服务中的的各个服务还是以模块的方式放在同一个单体中，比如：</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="app
├── UserService     [用户服务模块]
├── ContentService  [内容服务模块]
├── OrderService    [订单服务模块]
├── CartService     [购物车服务模块]
├── PayService      [支付服务模块]
├── GoodsService    [商品服务模块]
└── CustomService   [客服服务模块]" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs accesslog"><code>app
├── UserService     <span class="hljs-string">[用户服务模块]</span>
├── ContentService  <span class="hljs-string">[内容服务模块]</span>
├── OrderService    <span class="hljs-string">[订单服务模块]</span>
├── CartService     <span class="hljs-string">[购物车服务模块]</span>
├── PayService      <span class="hljs-string">[支付服务模块]</span>
├── GoodsService    <span class="hljs-string">[商品服务模块]</span>
└── CustomService   <span class="hljs-string">[客服服务模块]</span></code></pre>
<p>如上，我们简单的在一个单体里构建了各个服务模块，但是这些模块怎么通信呢？如下：</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="App::$app->get('demo/index/hello', [
    'user' => 'TIGERB'
]);" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs php"><code>App::$app-&gt;get(<span class="hljs-string">'demo/index/hello'</span>, [
    <span class="hljs-string">'user'</span> =&gt; <span class="hljs-string">'TIGERB'</span>
]);</code></pre>
<p>通过上面的方式我们就可以松耦合的方式进行单体下各个模块的通信和依赖了。与此同时，业务的发展是难以预估的，未来当我们向SOA的架构迁移时，很简单，我们只需要把以往的模块独立成各个项目，然后把App实例get方法的实现转变为RPC或者REST的策略即可，我们可以通过配置文件去调整对应的策略或者把自己的，第三方的实现注册进去即可。</p>
<p><a href="https://github.com/TIGERB/easy-php/blob/master/framework/handles/RouterHandle.php" rel="nofollow noreferrer" target="_blank">[file: framework/hanles/RouterHandle.php</a>]</p>
<h2 id="articleHeader9">传统的MVC模式提倡为MCL模式</h2>
<p>传统的MVC模式包含model-view-controller层，绝大多时候我们会把业务逻辑写到controller层或model层，但是慢慢的我们会发现代码难以阅读、维护、扩展，所以我在这里强制增加了一个logics层。至于，逻辑层里怎么写代码怎么，完全由你自己定义，你可以在里面实现一个工具类，你也可以在里面再新建子文件夹并在里面构建你的业务逻辑代码，你甚至可以实现一个基于责任连模式的网关(我会提供具体的示例)。这样看来，我们的最终结构是这样的:</p>
<ul>
<li>M: models, 职责只涉及数据模型相关操作</li>
<li>C: controllers, 职责对外暴露资源，前后端分离架构下controllers其实就相当于json格式的视图</li>
<li>L: logics, 职责灵活实现所有业务逻辑的地方</li>
</ul>
<p><strong>logics逻辑层</strong></p>
<p>逻辑层实现网关示例：</p>
<p>我们在logics层目录下增加了一个gateway目录，然后我们就可以灵活的在这个目录下编写逻辑了。gateway的结构如下：</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="gateway                     [Logics层目录下gateway逻辑目录]
  ├── Check.php             [接口]
  ├── CheckAppkey.php       [检验app key]
  ├── CheckArguments.php    [校验必传参数]
  ├── CheckAuthority.php    [校验访问权限]
  ├── CheckFrequent.php     [校验访问频率]
  ├── CheckRouter.php       [网关路由]
  ├── CheckSign.php         [校验签名]
  └── Entrance.php          [网关入口文件]" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs css"><code><span class="hljs-selector-tag">gateway</span>                     <span class="hljs-selector-attr">[Logics层目录下gateway逻辑目录]</span>
  ├── <span class="hljs-selector-tag">Check</span><span class="hljs-selector-class">.php</span>             <span class="hljs-selector-attr">[接口]</span>
  ├── <span class="hljs-selector-tag">CheckAppkey</span><span class="hljs-selector-class">.php</span>       <span class="hljs-selector-attr">[检验app key]</span>
  ├── <span class="hljs-selector-tag">CheckArguments</span><span class="hljs-selector-class">.php</span>    <span class="hljs-selector-attr">[校验必传参数]</span>
  ├── <span class="hljs-selector-tag">CheckAuthority</span><span class="hljs-selector-class">.php</span>    <span class="hljs-selector-attr">[校验访问权限]</span>
  ├── <span class="hljs-selector-tag">CheckFrequent</span><span class="hljs-selector-class">.php</span>     <span class="hljs-selector-attr">[校验访问频率]</span>
  ├── <span class="hljs-selector-tag">CheckRouter</span><span class="hljs-selector-class">.php</span>       <span class="hljs-selector-attr">[网关路由]</span>
  ├── <span class="hljs-selector-tag">CheckSign</span><span class="hljs-selector-class">.php</span>         <span class="hljs-selector-attr">[校验签名]</span>
  └── <span class="hljs-selector-tag">Entrance</span><span class="hljs-selector-class">.php</span>          <span class="hljs-selector-attr">[网关入口文件]</span></code></pre>
<p>网关入口类主要负责网关的初始化，代码如下：</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="// 初始化一个：必传参数校验的check
$checkArguments   =  new CheckArguments();
// 初始化一个：app key check
$checkAppkey      =  new CheckAppkey();
// 初始化一个：访问频次校验的check
$checkFrequent    =  new CheckFrequent();
// 初始化一个：签名校验的check
$checkSign        =  new CheckSign();
// 初始化一个：访问权限校验的check
$checkAuthority   =  new CheckAuthority();
// 初始化一个：网关路由规则
$checkRouter      =  new CheckRouter();

// 构成对象链
$checkArguments->setNext($checkAppkey)
               ->setNext($checkFrequent)
               ->setNext($checkSign)
               ->setNext($checkAuthority)
               ->setNext($checkRouter);

// 启动网关
$checkArguments->start(
    APP::$container->getSingle('request')
);" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs php"><code><span class="hljs-comment">// 初始化一个：必传参数校验的check</span>
$checkArguments   =  <span class="hljs-keyword">new</span> CheckArguments();
<span class="hljs-comment">// 初始化一个：app key check</span>
$checkAppkey      =  <span class="hljs-keyword">new</span> CheckAppkey();
<span class="hljs-comment">// 初始化一个：访问频次校验的check</span>
$checkFrequent    =  <span class="hljs-keyword">new</span> CheckFrequent();
<span class="hljs-comment">// 初始化一个：签名校验的check</span>
$checkSign        =  <span class="hljs-keyword">new</span> CheckSign();
<span class="hljs-comment">// 初始化一个：访问权限校验的check</span>
$checkAuthority   =  <span class="hljs-keyword">new</span> CheckAuthority();
<span class="hljs-comment">// 初始化一个：网关路由规则</span>
$checkRouter      =  <span class="hljs-keyword">new</span> CheckRouter();

<span class="hljs-comment">// 构成对象链</span>
$checkArguments-&gt;setNext($checkAppkey)
               -&gt;setNext($checkFrequent)
               -&gt;setNext($checkSign)
               -&gt;setNext($checkAuthority)
               -&gt;setNext($checkRouter);

<span class="hljs-comment">// 启动网关</span>
$checkArguments-&gt;start(
    APP::$container-&gt;getSingle(<span class="hljs-string">'request'</span>)
);</code></pre>
<p>实现完成这个gateway之后，我们如何在框架中去使用呢？在logic层目录中我提供了一个user-defined的实体类，我们把gateway的入口类注册到UserDefinedCase这个类中，示例如下：</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="/**
 * 注册用户自定义执行的类
 *
 * @var array
 */
private $map = [
    //　演示 加载自定义网关
    'App\Demo\Logics\Gateway\Entrance'
];" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs smali"><code>/**
 * 注册用户自定义执行的类
 *
 * @var<span class="hljs-built_in"> array
</span> */<span class="hljs-keyword">
private</span> $map = [
    //　演示 加载自定义网关
    'App\Demo\Logics\Gateway\Entrance'
];</code></pre>
<p>这样这个gateway就可以工作了。接着说说这个UserDefinedCase类，UserDefinedCase会在框架加载到路由机制之前被执行，这样我们就可以灵活的实现一些自定义的处理了。这个gateway只是个演示，你完全可以天马行空的组织你的逻辑～</p>
<p>视图View去哪了？由于选择了完全的前后端分离和SPA(单页应用), 所以传统的视图层也因此去掉了，详细的介绍看下面。</p>
<p><a href="https://github.com/TIGERB/easy-php/tree/master/app/demo" rel="nofollow noreferrer" target="_blank">[file: app/*</a>]</p>
<h2 id="articleHeader10">使用Vue作为视图</h2>
<p><strong>源码目录</strong></p>
<p>完全的前后端分离，数据双向绑定，模块化等等的大势所趋。这里我把我自己开源的vue前端项目结构<a href="http://vue.tigerb.cn/" rel="nofollow noreferrer" target="_blank">easy-vue</a>移植到了这个项目里，作为视图层。我们把前端的源码文件都放在frontend目录里，详细如下，你也可以自己定义：</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="frontend                        [前端源码和资源目录，这里存放我们整个前端的源码文件]
├── src                         [资源目录]
│    ├── components             [编写我们的前端组件]
│    ├── views                  [组装我们的视图]
│    ├── images                 [图片]
│    ├── ...
├── app.js                      [根js]
├── app.vue                     [根组件]
├── index.template.html         [前端入口文件模板]
└── store.js                    [状态管理，这里只是个演示，你可以很灵活的编写文件和目录]" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs css"><code><span class="hljs-selector-tag">frontend</span>                        <span class="hljs-selector-attr">[前端源码和资源目录，这里存放我们整个前端的源码文件]</span>
├── <span class="hljs-selector-tag">src</span>                         <span class="hljs-selector-attr">[资源目录]</span>
│    ├── <span class="hljs-selector-tag">components</span>             <span class="hljs-selector-attr">[编写我们的前端组件]</span>
│    ├── <span class="hljs-selector-tag">views</span>                  <span class="hljs-selector-attr">[组装我们的视图]</span>
│    ├── <span class="hljs-selector-tag">images</span>                 <span class="hljs-selector-attr">[图片]</span>
│    ├── ...
├── <span class="hljs-selector-tag">app</span><span class="hljs-selector-class">.js</span>                      <span class="hljs-selector-attr">[根js]</span>
├── <span class="hljs-selector-tag">app</span><span class="hljs-selector-class">.vue</span>                     <span class="hljs-selector-attr">[根组件]</span>
├── <span class="hljs-selector-tag">index</span><span class="hljs-selector-class">.template</span><span class="hljs-selector-class">.html</span>         <span class="hljs-selector-attr">[前端入口文件模板]</span>
└── <span class="hljs-selector-tag">store</span><span class="hljs-selector-class">.js</span>                    <span class="hljs-selector-attr">[状态管理，这里只是个演示，你可以很灵活的编写文件和目录]</span></code></pre>
<p><strong>build步骤</strong></p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="yarn install

DOMAIN=http://你的域名 npm run dev" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs sql"><code>yarn <span class="hljs-keyword">install</span>

<span class="hljs-keyword">DOMAIN</span>=<span class="hljs-keyword">http</span>://你的域名 npm run dev</code></pre>
<p><strong>编译后</strong></p>
<p>build成功之后会生成dist目录和入口文件index.html在public目录中。非发布分支.gitignore文件会忽略这些文件，发布分支去除忽略即可。</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="public                          [公共资源目录，暴露到万维网]
├── dist                        [前端build之后的资源目录，build生成的目录，不是发布分支忽略该目录]
│    └── ...
├── index.html                  [前端入口文件,build生成的文件，不是发布分支忽略该文件]" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs css"><code><span class="hljs-selector-tag">public</span>                          <span class="hljs-selector-attr">[公共资源目录，暴露到万维网]</span>
├── <span class="hljs-selector-tag">dist</span>                        <span class="hljs-selector-attr">[前端build之后的资源目录，build生成的目录，不是发布分支忽略该目录]</span>
│    └── ...
├── <span class="hljs-selector-tag">index</span><span class="hljs-selector-class">.html</span>                  <span class="hljs-selector-attr">[前端入口文件,build生成的文件，不是发布分支忽略该文件]</span></code></pre>
<p><a href="https://github.com/TIGERB/easy-php/tree/master/frontend" rel="nofollow noreferrer" target="_blank">[file: frontend/*</a>]</p>
<h2 id="articleHeader11">数据库对象关系映射</h2>
<p>数据库对象关系映射ORM(Object Relation Map)是什么？按照我目前的理解：顾名思义是建立对象和抽象事物的关联关系，在数据库建模中model实体类其实就是具体的表，对表的操作其实就是对model实例的操作。可能绝大多数的人都要问“为什么要这样做，直接sql语句操作不好吗？搞得这么麻烦！”，我的答案：直接sql语句当然可以，一切都是灵活的，但是从一个项目的<strong>可复用，可维护, 可扩展</strong>出发，采用ORM思想处理数据操作是理所当然的，想想如果若干一段时间你看见代码里大段的难以阅读且无从复用的sql语句，你是什么样的心情。</p>
<p>市面上对于ORM的具体实现有thinkphp系列框架的Active Record,yii系列框架的Active Record,laravel系列框架的Eloquent(据说是最优雅的)，那我们这里言简意赅就叫ORM了。接着为ORM建模，首先是ORM客户端实体DB：通过配置文件初始化不同的db策略，并封装了操作数据库的所有行为，最终我们通过DB实体就可以直接操作数据库了，这里的db策略目前我只实现了mysql(负责建立连接和db的底层操作)。接着我们把DB实体的sql解析功能独立成一个可复用的sql解析器的trait，具体作用：把对象的链式操作解析成具体的sql语句。最后，建立我们的模型基类model,model直接继承DB即可。最后的结构如下：</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="├── orm                         [对象关系模型]
│      ├── Interpreter.php      [sql解析器]
│      ├── DB.php               [数据库操作类]
│      ├── Model.php            [数据模型基类]
│      └── db                   [数据库类目录]
│          └── Mysql.php        [mysql实体类]" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs css"><code>├── <span class="hljs-selector-tag">orm</span>                         <span class="hljs-selector-attr">[对象关系模型]</span>
│      ├── <span class="hljs-selector-tag">Interpreter</span><span class="hljs-selector-class">.php</span>      <span class="hljs-selector-attr">[sql解析器]</span>
│      ├── <span class="hljs-selector-tag">DB</span><span class="hljs-selector-class">.php</span>               <span class="hljs-selector-attr">[数据库操作类]</span>
│      ├── <span class="hljs-selector-tag">Model</span><span class="hljs-selector-class">.php</span>            <span class="hljs-selector-attr">[数据模型基类]</span>
│      └── <span class="hljs-selector-tag">db</span>                   <span class="hljs-selector-attr">[数据库类目录]</span>
│          └── <span class="hljs-selector-tag">Mysql</span><span class="hljs-selector-class">.php</span>        <span class="hljs-selector-attr">[mysql实体类]</span></code></pre>
<p><strong>DB类使用示例</strong></p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="/**
 * DB操作示例
 *
 * findAll
 *
 * @return void
 */
public function dbFindAllDemo()
{
    $where = [
        'id'   => ['>=', 2],
    ];
    $instance = DB::table('user');
    $res      = $instance->where($where)
                         ->orderBy('id asc')
                         ->limit(5)
                         ->findAll(['id','create_at']);
    $sql      = $instance->sql;

    return $res;
}" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs php"><code><span class="hljs-comment">/**
 * DB操作示例
 *
 * findAll
 *
 * <span class="hljs-doctag">@return</span> void
 */</span>
<span class="hljs-keyword">public</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">dbFindAllDemo</span><span class="hljs-params">()</span>
</span>{
    $where = [
        <span class="hljs-string">'id'</span>   =&gt; [<span class="hljs-string">'&gt;='</span>, <span class="hljs-number">2</span>],
    ];
    $instance = DB::table(<span class="hljs-string">'user'</span>);
    $res      = $instance-&gt;where($where)
                         -&gt;orderBy(<span class="hljs-string">'id asc'</span>)
                         -&gt;limit(<span class="hljs-number">5</span>)
                         -&gt;findAll([<span class="hljs-string">'id'</span>,<span class="hljs-string">'create_at'</span>]);
    $sql      = $instance-&gt;sql;

    <span class="hljs-keyword">return</span> $res;
}</code></pre>
<p><strong>Model类使用示例</strong></p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="// controller 代码
/**
 * model example
 *
 * @return mixed
 */
public function modelExample()
{
    try {

        DB::beginTransaction();
        $testTableModel = new TestTable();

        // find one data
        $testTableModel->modelFindOneDemo();
        // find all data
        $testTableModel->modelFindAllDemo();
        // save data
        $testTableModel->modelSaveDemo();
        // delete data
        $testTableModel->modelDeleteDemo();
        // update data
        $testTableModel->modelUpdateDemo([
               'nickname' => 'easy-php'
            ]);
        // count data
        $testTableModel->modelCountDemo();

        DB::commit();
        return 'success';

    } catch (Exception $e) {
        DB::rollBack();
        return 'fail';
    }
}

//TestTable model
/**
 * Model操作示例
 *
 * findAll
 *
 * @return void
 */
public function modelFindAllDemo()
{
    $where = [
        'id'   => ['>=', 2],
    ];
    $res = $this->where($where)
                ->orderBy('id asc')
                ->limit(5)
                ->findAll(['id','create_at']);
    $sql = $this->sql;

    return $res;
}" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs php"><code><span class="hljs-comment">// controller 代码</span>
<span class="hljs-comment">/**
 * model example
 *
 * <span class="hljs-doctag">@return</span> mixed
 */</span>
<span class="hljs-keyword">public</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">modelExample</span><span class="hljs-params">()</span>
</span>{
    <span class="hljs-keyword">try</span> {

        DB::beginTransaction();
        $testTableModel = <span class="hljs-keyword">new</span> TestTable();

        <span class="hljs-comment">// find one data</span>
        $testTableModel-&gt;modelFindOneDemo();
        <span class="hljs-comment">// find all data</span>
        $testTableModel-&gt;modelFindAllDemo();
        <span class="hljs-comment">// save data</span>
        $testTableModel-&gt;modelSaveDemo();
        <span class="hljs-comment">// delete data</span>
        $testTableModel-&gt;modelDeleteDemo();
        <span class="hljs-comment">// update data</span>
        $testTableModel-&gt;modelUpdateDemo([
               <span class="hljs-string">'nickname'</span> =&gt; <span class="hljs-string">'easy-php'</span>
            ]);
        <span class="hljs-comment">// count data</span>
        $testTableModel-&gt;modelCountDemo();

        DB::commit();
        <span class="hljs-keyword">return</span> <span class="hljs-string">'success'</span>;

    } <span class="hljs-keyword">catch</span> (<span class="hljs-keyword">Exception</span> $e) {
        DB::rollBack();
        <span class="hljs-keyword">return</span> <span class="hljs-string">'fail'</span>;
    }
}

<span class="hljs-comment">//TestTable model</span>
<span class="hljs-comment">/**
 * Model操作示例
 *
 * findAll
 *
 * <span class="hljs-doctag">@return</span> void
 */</span>
<span class="hljs-keyword">public</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">modelFindAllDemo</span><span class="hljs-params">()</span>
</span>{
    $where = [
        <span class="hljs-string">'id'</span>   =&gt; [<span class="hljs-string">'&gt;='</span>, <span class="hljs-number">2</span>],
    ];
    $res = <span class="hljs-keyword">$this</span>-&gt;where($where)
                -&gt;orderBy(<span class="hljs-string">'id asc'</span>)
                -&gt;limit(<span class="hljs-number">5</span>)
                -&gt;findAll([<span class="hljs-string">'id'</span>,<span class="hljs-string">'create_at'</span>]);
    $sql = <span class="hljs-keyword">$this</span>-&gt;sql;

    <span class="hljs-keyword">return</span> $res;
}</code></pre>
<p><a href="https://github.com/TIGERB/easy-php/tree/master/framework/orm" rel="nofollow noreferrer" target="_blank">[file: framework/orm/*</a>]</p>
<h2 id="articleHeader12">服务容器模块</h2>
<p>什么是服务容器？</p>
<p>服务容器听起来很浮，按我的理解简单来说就是提供一个第三方的实体，我们把业务逻辑需要使用的类或实例注入到这个第三方实体类中，当需要获取类的实例时我们直接通过这个第三方实体类获取。</p>
<p>服务容器的意义？</p>
<p>用设计模式来讲：其实不管设计模式还是实际编程的经验中，我们都是强调“高内聚，松耦合”，我们做到高内聚的结果就是每个实体的作用都是极度专一，所以就产生了各个作用不同的实体类。在组织一个逻辑功能时，这些细化的实体之间就会不同程度的产生依赖关系，对于这些依赖我们通常的做法如下：</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="class Demo
{
    public function __construct()
    {
        // 类demo直接依赖RelyClassName
        $instance = new RelyClassName();
    }
}" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs php"><code><span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">Demo</span>
</span>{
    <span class="hljs-keyword">public</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">__construct</span><span class="hljs-params">()</span>
    </span>{
        <span class="hljs-comment">// 类demo直接依赖RelyClassName</span>
        $instance = <span class="hljs-keyword">new</span> RelyClassName();
    }
}</code></pre>
<p>这样的写法没有什么逻辑上的问题，但是不符合设计模式的“最少知道原则”，因为之间产生了直接依赖，整个代码结构不够灵活是紧耦合的。所以我们就提供了一个第三方的实体，把直接依赖转变为依赖于第三方，我们获取依赖的实例直接通过第三方去完成以达到松耦合的目的，这里这个第三方充当的角色就类似系统架构中的“中间件”，都是协调依赖关系和去耦合的角色。最后，这里的第三方就是所谓的服务容器。</p>
<p>在实现了一个服务容器之后，我把Request,Config等实例都以单例的方式注入到了服务容器中，当我们需要使用的时候从容器中获取即可，十分方便。使用如下：</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="// 注入单例
App::$container->setSingle('别名，方便获取', '对象/闭包/类名');

// 例，注入Request实例
App::$container->setSingle('request', function () {
    // 匿名函数懒加载
    return new Request();
});
// 获取Request对象
App::$container->getSingle('request');" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs php"><code><span class="hljs-comment">// 注入单例</span>
App::$container-&gt;setSingle(<span class="hljs-string">'别名，方便获取'</span>, <span class="hljs-string">'对象/闭包/类名'</span>);

<span class="hljs-comment">// 例，注入Request实例</span>
App::$container-&gt;setSingle(<span class="hljs-string">'request'</span>, <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-params">()</span> </span>{
    <span class="hljs-comment">// 匿名函数懒加载</span>
    <span class="hljs-keyword">return</span> <span class="hljs-keyword">new</span> Request();
});
<span class="hljs-comment">// 获取Request对象</span>
App::$container-&gt;getSingle(<span class="hljs-string">'request'</span>);</code></pre>
<p><a href="https://github.com/TIGERB/easy-php/blob/master/framework/Container.php" rel="nofollow noreferrer" target="_blank">[file: framework/Container</a>]</p>
<h2 id="articleHeader13">Nosql模块</h2>
<p>提供对nosql的支持，提供全局单例对象，借助我们的服务容器我们在框架启动的时候，通过配置文件的配置把需要的nosql实例注入到服务容器中。目前我们支持redis/memcahed/mongodb。</p>
<p>如何使用？如下，</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="// 获取redis对象
App::$container->getSingle('redis');
// 获取memcahed对象
App::$container->getSingle('memcahed');
// 获取mongodb对象
App::$container->getSingle('mongodb');" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs clean"><code><span class="hljs-comment">// 获取redis对象</span>
App::$container-&gt;getSingle(<span class="hljs-string">'redis'</span>);
<span class="hljs-comment">// 获取memcahed对象</span>
App::$container-&gt;getSingle(<span class="hljs-string">'memcahed'</span>);
<span class="hljs-comment">// 获取mongodb对象</span>
App::$container-&gt;getSingle(<span class="hljs-string">'mongodb'</span>);</code></pre>
<p><a href="https://github.com/TIGERB/easy-php/tree/master/framework/nosql" rel="nofollow noreferrer" target="_blank">[file: framework/nosql/*</a>]</p>
<h2 id="articleHeader14">接口文档生成和接口模拟模块</h2>
<p>通常我们写完一个接口后，接口文档是一个问题，我们这里使用Api Blueprint协议完成对接口文档的书写和mock(可用)，同时我们配合使用Swagger通过接口文档实现对接口的实时访问(目前未实现)。</p>
<p>Api Blueprint接口描述协议选取的工具是snowboard,具体使用说明如下：</p>
<p><strong>接口文档生成说明</strong></p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="cd docs/apib

./snowboard html -i demo.apib -o demo.html -s

open the website, http://localhost:8088/" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs stylus"><code>cd docs/apib

./snowboard <span class="hljs-selector-tag">html</span> -<span class="hljs-selector-tag">i</span> demo<span class="hljs-selector-class">.apib</span> -o demo<span class="hljs-selector-class">.html</span> -s

open the website, http:<span class="hljs-comment">//localhost:8088/</span></code></pre>
<p><strong>接口mock使用说明</strong></p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="cd docs/apib

./snowboard mock -i demo.apib

open the website, http://localhost:8087/demo/index/hello" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs stylus"><code>cd docs/apib

./snowboard mock -<span class="hljs-selector-tag">i</span> demo<span class="hljs-selector-class">.apib</span>

open the website, http:<span class="hljs-comment">//localhost:8087/demo/index/hello</span></code></pre>
<p><a href="https://github.com/TIGERB/easy-php/tree/master/docs" rel="nofollow noreferrer" target="_blank">[file: docs/*</a>]</p>
<h2 id="articleHeader15">单元测试模块</h2>
<p>基于phpunit的单元测试，写单元测试是个好的习惯。</p>
<p>如何使用？</p>
<p>tests目录下编写测试文件，具体参考tests/demo目录下的DemoTest文件,然后运行：</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text=" vendor/bin/phpunit" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs awk"><code style="word-break: break-word; white-space: initial;"> vendor<span class="hljs-regexp">/bin/</span>phpunit</code></pre>
<p>测试断言示例：</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="/**
 *　演示测试
 */
public function testDemo()
{
    $this->assertEquals(
        'Hello Easy PHP',
        // 执行demo模块index控制器hello操作，断言结果是不是等于Hello Easy PHP　
        App::$app->get('demo/index/hello')
    );
}" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs php"><code><span class="hljs-comment">/**
 *　演示测试
 */</span>
<span class="hljs-keyword">public</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">testDemo</span><span class="hljs-params">()</span>
</span>{
    <span class="hljs-keyword">$this</span>-&gt;assertEquals(
        <span class="hljs-string">'Hello Easy PHP'</span>,
        <span class="hljs-comment">// 执行demo模块index控制器hello操作，断言结果是不是等于Hello Easy PHP　</span>
        App::$app-&gt;get(<span class="hljs-string">'demo/index/hello'</span>)
    );
}</code></pre>
<p><a href="https://phpunit.de/manual/current/zh_cn/appendixes.assertions.html" rel="nofollow noreferrer" target="_blank">phpunit断言文档语法参考</a></p>
<p><a href="https://github.com/TIGERB/easy-php/tree/master/tests" rel="nofollow noreferrer" target="_blank">[file: tests/*</a>]</p>
<h2 id="articleHeader16">Git钩子配置</h2>
<p>目的规范化我们的项目代码和commit记录。</p>
<ul>
<li>代码规范：配合使用php_codesniffer，在代码提交前对代码的编码格式进行强制验证。</li>
<li>commit-msg规范：采用ruanyifeng的commit msg规范，对commit msg进行格式验证，增强git log可读性和便于后期查错和统计log等, 这里使用了<a href="https://github.com/Treri" rel="nofollow noreferrer" target="_blank">Treri</a>的commit-msg脚本，Thx~。</li>
</ul>
<p><a href="https://github.com/TIGERB/easy-php/tree/master/.git-hooks" rel="nofollow noreferrer" target="_blank">[file: ./git-hooks/*</a>]</p>
<h2 id="articleHeader17">辅助脚本</h2>
<p><strong>cli脚本</strong></p>
<p>以命令行的方式运行框架，具体见使用说明。</p>
<p><strong>build脚本</strong></p>
<p>打包PHP项目脚本，打包整个项目到runtime/build目录，例如：</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="runtime/build/App.20170505085503.phar

<?php
// 入口文件引入包文件即可
require('runtime/build/App.20170505085503.phar');" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs xml"><code>runtime/build/App.20170505085503.phar

<span class="php"><span class="hljs-meta">&lt;?php</span>
<span class="hljs-comment">// 入口文件引入包文件即可</span>
<span class="hljs-keyword">require</span>(<span class="hljs-string">'runtime/build/App.20170505085503.phar'</span>);</span></code></pre>
<p><a href="https://github.com/TIGERB/easy-php/tree/master/build" rel="nofollow noreferrer" target="_blank">[file: ./build</a>]</p>
<h1 id="articleHeader18">如何使用?</h1>
<p>执行：</p>
<blockquote>composer create-project tigerb/easy-php easy  --prefer-dist &amp;&amp; cd easy</blockquote>
<p><strong>网站服务模式:</strong></p>
<p>快速开始一个demo:</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="cd bin &amp;&amp; php cli --run" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs stata"><code style="word-break: break-word; white-space: initial;"><span class="hljs-keyword">cd</span> bin &amp;&amp; php <span class="hljs-keyword">cli</span> --<span class="hljs-keyword">run</span></code></pre>
<p>demo如下：</p>
<p><span class="img-wrap"><img data-src="/img/remote/1460000009321795" src="https://static.alili.tech/img/remote/1460000009321795" alt="https://raw.githubusercontent.com/TIGERB/easy-php/master/demo.gif" title="https://raw.githubusercontent.com/TIGERB/easy-php/master/demo.gif" style="cursor: pointer;"></span></p>
<p><strong>客户端脚本模式:</strong></p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="php cli --method=<module.controller.action> --<arguments>=<value> ...

例如, php cli --method=demo.index.get --username=easy-php" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs oxygene"><code>php cli --<span class="hljs-function"><span class="hljs-keyword">method</span>=&lt;<span class="hljs-title">module</span>.<span class="hljs-title">controller</span>.<span class="hljs-title">action</span>&gt; --&lt;<span class="hljs-title">arguments</span>&gt;=&lt;<span class="hljs-title">value</span>&gt; ...

例如, <span class="hljs-title">php</span> <span class="hljs-title">cli</span> --<span class="hljs-title">method</span>=<span class="hljs-title">demo</span>.<span class="hljs-title">index</span>.<span class="hljs-title">get</span> --<span class="hljs-title">username</span>=<span class="hljs-title">easy</span>-<span class="hljs-title">php</span></span></code></pre>
<p>获取帮助:</p>
<p>使用命令 php cli 或者 php cli --help</p>
<p><strong>Swoole模式:</strong></p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="cd public &amp;&amp; php server.php" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs axapta"><code style="word-break: break-word; white-space: initial;">cd <span class="hljs-keyword">public</span> &amp;&amp; php <span class="hljs-keyword">server</span>.php</code></pre>
<p>获取帮助:</p>
<p>使用命令 php cli 或者 php cli --help</p>
<h1 id="articleHeader19">docker环境</h1>
<p>本框架提供docker开发环境，一行命令几分钟构建你的开发环境，详细请点击<a href="https://easy-framework.github.io/easy-env/" rel="nofollow noreferrer" target="_blank">easy-env</a>查看。</p>
<h1 id="articleHeader20">性能-fpm</h1>
<blockquote>ab -c 100 -n 10000 "http://easy-php.local/Demo/Index/hello"</blockquote>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="Document Path:          /
Document Length:        53 bytes

Concurrency Level:      100
Time taken for tests:   3.259 seconds
Complete requests:      10000
Failed requests:        0
Total transferred:      1970000 bytes
HTML transferred:       530000 bytes
Requests per second:    3068.87 [#/sec] (mean)
Time per request:       32.585 [ms] (mean)
Time per request:       0.326 [ms] (mean, across all concurrent requests)
Transfer rate:          590.40 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.3      0       4
Processing:     6   32   4.0     31      68
Waiting:        6   32   4.0     31      68
Total:          8   32   4.0     31      68

Percentage of the requests served within a certain time (ms)
  50%     31
  66%     32
  75%     33
  80%     34
  90%     39
  95%     41
  98%     43
  99%     46
 100%     68 (longest request)" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs tap"><code>Document Path:          /
Document Length:       <span class="hljs-number"> 53 </span>bytes

Concurrency Level:      100
Time taken for tests:   3.259 seconds
Complete requests:      10000
Failed requests:        0
Total transferred:     <span class="hljs-number"> 1970000 </span>bytes
HTML transferred:      <span class="hljs-number"> 530000 </span>bytes
Requests per second:    3068.87 [<span class="hljs-comment">#/sec] (mean)</span>
Time per request:       32.585 [ms] (mean)
Time per request:       0.326 [ms] (mean, across all concurrent requests)
Transfer rate:          590.40 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:       <span class="hljs-number"> 0 </span>  <span class="hljs-number"> 0 </span>  0.3     <span class="hljs-number"> 0 </span>      4
Processing:    <span class="hljs-number"> 6 </span> <span class="hljs-number"> 32 </span>  4.0    <span class="hljs-number"> 31 </span>     68
Waiting:       <span class="hljs-number"> 6 </span> <span class="hljs-number"> 32 </span>  4.0    <span class="hljs-number"> 31 </span>     68
Total:         <span class="hljs-number"> 8 </span> <span class="hljs-number"> 32 </span>  4.0    <span class="hljs-number"> 31 </span>     68

Percentage of the requests served within a certain time (ms)
  50%     31
  66%     32
  75%     33
  80%     34
  90%     39
  95%     41
  98%     43
  99%     46
 100%    <span class="hljs-number"> 68 </span>(longest request)</code></pre>
<h1 id="articleHeader21">性能-Swoole</h1>
<blockquote>ab -c 100 -n 10000 "http://easy-php.local/Demo/Index/hello"</blockquote>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="Concurrency Level:      100
Time taken for tests:   1.319 seconds
Complete requests:      10000
Failed requests:        0
Total transferred:      1870000 bytes
HTML transferred:       160000 bytes
Requests per second:    7580.84 [#/sec] (mean)
Time per request:       13.191 [ms] (mean)
Time per request:       0.132 [ms] (mean, across all concurrent requests)
Transfer rate:          1384.39 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    5  10.6      3     172
Processing:     1    9  13.4      7     177
Waiting:        0    7  11.7      6     173
Total:          3   13  16.9     11     179

Percentage of the requests served within a certain time (ms)
  50%     11
  66%     12
  75%     13
  80%     14
  90%     15
  95%     17
  98%     28
  99%     39
 100%    179 (longest request)" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs tap"><code>Concurrency Level:      100
Time taken for tests:   1.319 seconds
Complete requests:      10000
Failed requests:        0
Total transferred:     <span class="hljs-number"> 1870000 </span>bytes
HTML transferred:      <span class="hljs-number"> 160000 </span>bytes
Requests per second:    7580.84 [<span class="hljs-comment">#/sec] (mean)</span>
Time per request:       13.191 [ms] (mean)
Time per request:       0.132 [ms] (mean, across all concurrent requests)
Transfer rate:          1384.39 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:       <span class="hljs-number"> 0 </span>  <span class="hljs-number"> 5 </span> 10.6     <span class="hljs-number"> 3 </span>    172
Processing:    <span class="hljs-number"> 1 </span>  <span class="hljs-number"> 9 </span> 13.4     <span class="hljs-number"> 7 </span>    177
Waiting:       <span class="hljs-number"> 0 </span>  <span class="hljs-number"> 7 </span> 11.7     <span class="hljs-number"> 6 </span>    173
Total:         <span class="hljs-number"> 3 </span> <span class="hljs-number"> 13 </span> 16.9    <span class="hljs-number"> 11 </span>    179

Percentage of the requests served within a certain time (ms)
  50%     11
  66%     12
  75%     13
  80%     14
  90%     15
  95%     17
  98%     28
  99%     39
 100%   <span class="hljs-number"> 179 </span>(longest request)</code></pre>
<h1 id="articleHeader22">问题和贡献</h1>
<p>不足的地方还有很多，如果大家发现了什么问题，可以给我提<a href="https://github.com/TIGERB/easy-php/issues" rel="nofollow noreferrer" target="_blank">issue</a>或者PR。</p>
<p>或者你觉着在这个框架实现的细节你想了解的，一样可以给我提<a href="https://github.com/TIGERB/easy-php/issues" rel="nofollow noreferrer" target="_blank">issue</a>，后面我会总结成相应的文章分享给大家。</p>
<p>如何贡献？</p>
<div class="widget-codetool" style="display:none;">
      <div class="widget-codetool--inner">
      <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
      <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="cp ./.git-hooks/* ./git/hooks" title="" data-original-title="复制"></span>
      <span type="button" class="saveToNote code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="放进笔记"></span>
      </div>
      </div><pre class="hljs awk"><code style="word-break: break-word; white-space: initial;">cp .<span class="hljs-regexp">/.git-hooks/</span>* .<span class="hljs-regexp">/git/</span>hooks</code></pre>
<p>然后正常发起PR即可, 所有的commit我都会进行代码格式(psr)验证和commit-msg验证，如果发生错误，请按照提示纠正即可。</p>
<p>项目地址：<a href="https://github.com/TIGERB/easy-php" rel="nofollow noreferrer" target="_blank">https://github.com/TIGERB/easy-php</a></p>
<h1 id="articleHeader23">TODO</h1>
<ul>
<li>懒加载优化框架加载流程</li>
<li>变更Helper助手类的成员方法为框架函数，简化使用提高生产效率</li>
<li>提供更友善的开发api帮助</li>
<li>模块支持数据库nosql自定义配置</li>
<li>支持mysql主从配置</li>
<li>ORM提供更多链式操作api</li>
<li>框架log行为进行级别分类</li>
<li>想办法解决上线部署是配置文件问题</li>
<li>性能测试和优化</li>
<li>...</li>
</ul>
<blockquote><a href="http://easy-php.tigerb.cn" rel="nofollow noreferrer" target="_blank">Easy PHP：一个极速轻量级的PHP全栈框架</a></blockquote>
<p><strong>扫面下方二维码关注我的技术公众号，及时为大家推送我的原创技术分享</strong></p>
<p><span class="img-wrap"><img data-src="/img/bVbb7Dw?w=258&amp;h=258" src="https://static.alili.tech/img/bVbb7Dw?w=258&amp;h=258" alt="图片描述" title="图片描述" style="cursor: pointer;"></span></p>

                
{{< /raw >}}

# 版权声明
本文资源来源互联网，仅供学习研究使用，版权归该资源的合法拥有者所有，

本文仅用于学习、研究和交流目的。转载请注明出处、完整链接以及原作者。

原作者若认为本站侵犯了您的版权，请联系我们，我们会立即删除！

## 原文标题
从0开始构建一个属于你自己的PHP框架

## 原文链接
[https://segmentfault.com/a/1190000009321792](https://segmentfault.com/a/1190000009321792)

