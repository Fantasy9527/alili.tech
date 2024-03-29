---
title: '在你的网络中使用树莓派和 Pi-hole 拦截广告' 
date: 2019-01-20 2:30:11
hidden: true
slug: 5ozl5ddyvjh
categories: [reprint]
---

{{< raw >}}

            <h1><a href="#在你的网络中使用树莓派和-pi-hole-拦截广告"></a>在你的网络中使用树莓派和 Pi-hole 拦截广告</h1>
<blockquote>
<p>痛恨上网时看到广告？学习这篇教程来设置 Pi-hole。</p>
</blockquote>
<p><a href="https://camo.githubusercontent.com/30f10531220f168ae35746f17e08bc83055d3168/68747470733a2f2f6f70656e736f757263652e636f6d2f73697465732f64656661756c742f66696c65732f7374796c65732f696d6167652d66756c6c2d73697a652f7075626c69632f6c6561642d696d616765732f70692d686f6c652d62616e6e65722e706e673f69746f6b3d315458637033686d"><img src="http://p0.qhimg.com/t01a7478778edf08410.png" alt=""></a></p>
<p>有一个闲置的树莓派？在浏览网页时讨厌广告？<a href="https://pi-hole.net/">Pi-hole</a> 是一个拦截广告的开源软件项目，它可以将你的家庭网络上的所有广告路由到一个不存在的地方，从而实现在你的设备上拦截广告的目的。这么好的方法只需要花几钟的时间来设置，你就可以使用它了。</p>
<p>Pi-hole 拦截了超过 100,000 个提供广告的域名，它可以拦截任何设备（包括移动设备、平板电脑、以及个人电脑）上的广告，并且它是完整的拦截了广告，而不是仅将它们隐藏起来，这样做可以提升总体的网络性能（因为广告不需要下载）。你可以在一个 web 界面上、或者也可以使用一个 API 来监视性能和统计数据。</p>
<h3><a href="#你需要"></a>你需要：</h3>
<ul>
<li>树莓派 + SD 卡</li>
<li>USB 电源线</li>
<li>以太网线</li>
</ul>
<p><a href="https://camo.githubusercontent.com/3e6855916ed43dfa45d1adf384e279c021bb5006/68747470733a2f2f6f70656e736f757263652e636f6d2f73697465732f64656661756c742f66696c65732f753132383635312f7261737062657272792d70692e706e67"><img src="http://p0.qhimg.com/t01eb790855c793a174.png" alt=""></a> <a href="https://camo.githubusercontent.com/a362e80ff86bb6c6264d648722ab10c4e42ad28f/68747470733a2f2f6f70656e736f757263652e636f6d2f73697465732f64656661756c742f66696c65732f753132383635312f6e6f6f62732d636172642e706e67"><img src="http://p0.qhimg.com/t0155c0d71c96cf032a.png" alt=""></a> <a href="https://camo.githubusercontent.com/647901ed0f124a81bbdf6a13d56553c16fe825de/68747470733a2f2f6f70656e736f757263652e636f6d2f73697465732f64656661756c742f66696c65732f753132383635312f706f7765722d737570706c792e706e67"><img src="http://p0.qhimg.com/t0170c96a74c43d13d8.png" alt=""></a> <a href="https://camo.githubusercontent.com/c337dbc770240eb0429bc05c306f8c376e769bc0/68747470733a2f2f6f70656e736f757263652e636f6d2f73697465732f64656661756c742f66696c65732f753132383635312f65746865726e65742d6361626c652e706e67"><img src="http://p0.qhimg.com/t0113991fda4f6271c3.png" alt=""></a></p>
<p>你不需要使用一个最新型号的树莓派 — 一个老款足够完成这项工作，只要它的内存不小于 512MB 就可以 — 因此一个一代树莓派 Model B（rev 2）就足够，一个 Model B+、或者二代的或者三代的树莓派都可以。你可以使用 Pi Zero，但需要一个 USB micro 以太网适配器。你可以使用一个带 WiFi 的 Pi Zero W 而不是以太网。但是，作为你的网络基础设施的一部分，我建议你使用一个性能良好、稳定的有线连接来代替 WiFi 连接。</p>
<h3><a href="#准备-sd-卡"></a>准备 SD 卡</h3>
<p>开始的第一步，你可能需要将 Raspbian Stretch Lite 安装到一个 SD 卡上。SD 卡至少需要 4GB 大小（完整的桌面版 Raspbian 镜像至少要 8GB，但是 Lite 版镜像更小更轻量化，足够完成这项工作）。如果你喜欢，也可以使用完整的 Raspbian 桌面版镜像，但是作为一个去运行简单应用程序的树莓派，你没必要做更多的事情。</p>
<p><a href="https://camo.githubusercontent.com/0051c660b665e4817b1a1ac51b90490f338707c0/68747470733a2f2f6f70656e736f757263652e636f6d2f73697465732f64656661756c742f66696c65732f753132383635312f726173706269616e2d646f776e6c6f6164732e706e67"><img src="http://p0.qhimg.com/t01fc4f7ebcd694b48c.png" alt=""></a></p>
<p>使用你的个人电脑，从树莓派的网站上下载 Raspbian Stretch Lite 镜像。解压它并提取出里面的 <code>.img</code> 文件，然后将这个 <code>.img</code> 文件写入到你的 SD 卡。不论你的 SD 卡是否是空白的，这一步都不会有什么麻烦，因为在写入前它会清空上面的数据。</p>
<p>如果你使用的是 Linux，写入镜像文件更简单的办法是使用命令行工具 <code>dd</code>。或者，你也可以使用跨平台的软件 <a href="https://etcher.io/">Etcher</a> （可以去参考 Les Pounder 写的指南 “<a href="https://opensource.com/article/17/3/how-write-sd-cards-raspberry-pi">如何为树莓派准备 SD 卡</a>“）。</p>
<p><a href="https://camo.githubusercontent.com/abd4b32fd8f7e53cdc11ca694f20691cbdb59ccb/68747470733a2f2f6f70656e736f757263652e636f6d2f73697465732f64656661756c742f66696c65732f6574636865722d77696e2d35323070782e706e67"><img src="http://p0.qhimg.com/t01847306330c99c5d2.png" alt=""></a></p>
<p>SD 卡准备完成之后，你可以将它插入到你的树莓派，连接上键盘、显示器和以太网，然后为树莓派接上电源。在初始化设置之后，这个树莓派就不需要键盘或显示器了。如果你有使用“无末端headless”树莓派工作的经验，你可以 <a href="https://www.raspberrypi.org/blog/a-security-update-for-raspbian-pixel/">启用 SSH</a> 然后去设置它 <a href="https://www.raspberrypi.org/documentation/remote-access/ssh/README.md">启用远程连接</a>。</p>
<h3><a href="#安装-pi-hole"></a>安装 Pi-hole</h3>
<p>在你的树莓派引导完成之后，用缺省用户名（<code>pi</code>）和密码（<code>raspberry</code>）登入。现在你就可以运行命令行了，可以去安装 Pi-hole 了。简单地输入下列命令并回车：</p>
<pre><code class="hljs groovy">curl -sSL <span class="hljs-string">https:</span><span class="hljs-comment">//install.pi-hole.net | bash</span>

</code></pre><p>这个命令下载了 Pi-hole 安装脚本然后去运行它。你可以在你的电脑浏览器中输入 <code>https://install.pi-hole.net</code> 来查看它的内容，你将会看到这个脚本做了些什么。它为你生成了一个<strong>管理员密码</strong>，并和其它安装信息一起显示在你的屏幕上。</p>
<p>就是这么简单，几分钟之后，你的树莓派将准备好为你拦截广告。</p>
<p>在你断开树莓派连接之前，你需要知道它的 IP 地址和你的路由器的 IP 地址。（如果你不知道），在你的终端中输入 <code>hostname -I</code> 来查看你的树莓派的 IP 地址，输入 <code>ip route | grep default</code> 来找到你的路由器的 IP 地址。你看到的将是像 <code>192.168.1.1</code> 这样的地址。</p>
<h3><a href="#配置你的路由器"></a>配置你的路由器</h3>
<p>你的树莓派现在运行着一个 DNS 服务器，接下来你需要告诉你的路由器去使用 Pi-hole 作为它的 DNS 服务器而不是你的 ISP 提供给你的缺省 DNS。进入路由器的管理控制台 web 界面。这个界面一般是输入你的路由器的 IP 地址来进入的。</p>
<p>找到 LAN 设置下面的 DHCP/DNS 设置，然后将你的主 DNS 服务器的 IP 地址设置为你的 Pi-hole 的 IP 地址。设置完成之后，它应该你下图的样子：</p>
<p><a href="https://camo.githubusercontent.com/c2f260a994a36f3432775bfb75fbd0a19e656973/68747470733a2f2f6f70656e736f757263652e636f6d2f73697465732f64656661756c742f66696c65732f753132383635312f70692d686f6c652d646e732e706e67"><img src="http://p0.qhimg.com/t018e952c7625ba15f3.png" alt=""></a></p>
<p>关于这一步的更多信息，可以查看 <a href="https://discourse.pi-hole.net/t/how-do-i-configure-my-devices-to-use-pi-hole-as-their-dns-server/245">Pi-hole discourse</a>。</p>
<p>你还需要确保你的 Pi-hole 始终保持相同的 IP 地址，因此，你需要去查看 DHCP 设置，将你的树莓派的 IP 地址条目添加到保留地址中。</p>
<h3><a href="#外部测试"></a>外部测试</h3>
<p>现在，在命令行下输入 <code>sudo halt</code> 关闭运行的树莓派，并断开它的电源。你可以拔掉显示器连接线和键盘，然后将你的树莓派放置到一个合适的固定的地方 — 或许应该将它放在你的路由器附近。确保连接着以太网线，然后重新连接电源以启动它。</p>
<p>在你的个人电脑上导航到一个网站（强烈建议访问 <a href="https://opensource.com/">Opensource.com</a> 网站），或者用你的 WiFi 中的一个设备去检查你的因特网访问是否正常（如果不能正常访问，可能是你的 DNS 配置错误）。如果在浏览器中看到了预期的结果，说明它的工作正常。现在，你浏览网站时，应该再也看不到广告了！甚至在你的 app 中提供的广告也无法出现在你的移动设备中！祝你“冲浪”愉快！</p>
<p>如果你想去测试一下你的广告拦截的新功能，你可以去这个 <a href="https://pi-hole.net/pages-to-test-ad-blocking-performance/">测试页面</a> 尝试浏览一些内置广告的网站。</p>
<p>现在你可以在你的电脑浏览器上输入 Pi-hole 的 IP 地址来访问它的 web 界面（比如，<code>http://192.168.1.4/admin</code> 或者 <code>http://pi.hole/admin</code> 也可能会工作）。你将看到 Pi-hole 管理面板和一些统计数据（在这时可能数字比较小）。在你输入（在安装时显示在屏幕上的）密码后，你将看到更漂亮的图形界面：</p>
<p><a href="https://camo.githubusercontent.com/6ced8c4446d2d68b2f0282775985f18846e27e9d/68747470733a2f2f6f70656e736f757263652e636f6d2f73697465732f64656661756c742f66696c65732f753132383635312f70692d686f6c652d7765622e706e67"><img src="http://p0.qhimg.com/t01f7fd04056f54458d.png" alt=""></a></p>
<p>你也可以微调你的 Pi-hole 的设置，像域名的白名单和黑名单、永久和临时禁止、访问拦截统计信息等等。</p>
<p>个别情况下，你可能需要去升级你的 Pi-hole 安装。当软件需要更新时，这个 web 界面会出现一个更新提示。如果你启用了 SSH，你可以远程登入，否则，那你只能再次连接键盘和显示器。远程登入之后，输入 <code>pihole -up</code>命令来更新它。</p>
<p><a href="https://camo.githubusercontent.com/1e8db297e50523209cc97a8baa31b415f9bd9cad/68747470733a2f2f6f70656e736f757263652e636f6d2f73697465732f64656661756c742f66696c65732f753132383635312f70692d686f6c652d7570646174652e706e67"><img src="http://p0.qhimg.com/t019eb36a2d91dd300b.png" alt=""></a></p>
<p>如果你使用过 Pi-hole 或者其它的开源广告拦截器，请在下面的评论区把你的经验共享出来。</p>
<hr>
<p>via: <a href="https://opensource.com/article/18/2/block-ads-raspberry-pi">https://opensource.com/article/18/2/block-ads-raspberry-pi</a></p>
<p>作者：<a href="https://opensource.com/users/bennuttall">Ben Nuttall</a> 译者：<a href="https://github.com/qhwdw">qhwdw</a> 校对：<a href="https://github.com/wxy">wxy</a></p>
<p>本文由 <a href="https://github.com/LCTT/TranslateProject">LCTT</a> 原创编译，<a href="https://linux.cn/">Linux中国</a> 荣誉推出</p>

          
{{< /raw >}}

# 版权声明
本文资源来源互联网，仅供学习研究使用，版权归该资源的合法拥有者所有，

本文仅用于学习、研究和交流目的。转载请注明出处、完整链接以及原作者。

原作者若认为本站侵犯了您的版权，请联系我们，我们会立即删除！

## 原文标题
在你的网络中使用树莓派和 Pi-hole 拦截广告

## 原文链接
[https://www.zcfy.cc/article/block-ads-on-your-network-with-raspberry-pi-and-pi-hole](https://www.zcfy.cc/article/block-ads-on-your-network-with-raspberry-pi-and-pi-hole)

