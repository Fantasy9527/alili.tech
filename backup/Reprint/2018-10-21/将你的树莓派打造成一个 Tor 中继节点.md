---
title: 将你的树莓派打造成一个 Tor 中继节点
hidden: true
categories: [reprint]
slug: 7e43fcce
date: 2018-10-21 00:00:00
---

{{< raw >}}

            <h1><a href="#将你的树莓派打造成一个-tor-中继节点"></a>将你的树莓派打造成一个 Tor 中继节点</h1>
<blockquote>
<p>在此教程中学习如何将你的旧树莓派打造成一个完美的 Tor 中继节点。</p>
</blockquote>
<p><a href="https://camo.githubusercontent.com/87c7dab3e0caa528b4ce1cc01b86a061372577a6/68747470733a2f2f7777772e6c696e75782e636f6d2f73697465732f6c636f6d2f66696c65732f7374796c65732f72656e64657265645f66696c652f7075626c69632f746f722d6f6e696f6e2d726f757465722e6a70673f69746f6b3d3657556c30456c48"><img src="https://p0.ssl.qhimg.com/t01b08ea3068153da47.jpg" alt=""></a></p>
<p>你是否和我一样，在第一代或者第二代树莓派发布时买了一个，玩了一段时间就把它搁置“吃灰”了。毕竟，除非你是机器人爱好者，否则一般不太可能去长时间使用一个处理器很慢的、并且内存只有 256 MB 的计算机。这并不是说你不能用它去做一件很酷的东西，但是在工作和其它任务之间，我还没有看到用一些旧的物件发挥新作用的机会。</p>
<p>然而，如果你想去好好利用它并且不想花费你太多的时间和资源的话，可以将你的旧树莓派打造成一个完美的 Tor 中继节点。</p>
<h3><a href="#tor-中继节点是什么"></a>Tor 中继节点是什么</h3>
<p>在此之前你或许听说过 <a href="https://www.torproject.org/">Tor 项目</a>，如果恰好你没有听说过，我简单给你介绍一下，“Tor” 是 “The Onion Router（洋葱路由器）” 的缩写，它是用来对付在线追踪和其它违反隐私行为的技术。</p>
<p>不论你在互联网上做什么事情，都会在你的 IP 包通过的设备上留下一些数字“脚印”：所有的交换机、路由器、负载均衡，以及目标网络记录的来自你的原始会话的 IP 地址，以及你访问的互联网资源（通常是它的主机名，<a href="https://en.wikipedia.org/wiki/Server_Name_Indication#Security_implications">即使是在使用 HTTPS 时</a>）的 IP 地址。如过你是在家中上互联网，那么你的 IP 地址可以直接映射到你的家庭所在地。如果你使用了 VPN 服务（<a href="https://www.linux.com/blog/2017/10/tips-secure-your-network-wake-krack">你应该使用</a>），那么你的 IP 地址映射到你的 VPN 提供商那里，而 VPN 提供商是可以映射到你的家庭所在地的。无论如何，有可能在某个地方的某个人正在根据你访问的网络和在网站上呆了多长时间来为你建立一个个人的在线资料。然后将这个资料进行出售，并与从其它服务上收集的资料进行聚合，然后利用广告网络进行赚钱。至少，这是乐观主义者对如何利用这些数据的一些看法 —— 我相信你还可以找到更多的更恶意地使用这些数据的例子。</p>
<p>Tor 项目尝试去提供一个解决这种问题的方案，使它们不可能（或者至少是更加困难）追踪到你的终端 IP 地址。Tor 是通过让你的连接在一个由匿名的入口节点、中继节点和出口节点组成的匿名中继链上反复跳转的方式来实现防止追踪的目的：</p>
<ol>
<li><strong>入口节点</strong> 只知道你的 IP 地址和中继节点的 IP 地址，但是不知道你最终要访问的目标 IP 地址</li>
<li><strong>中继节点</strong> 只知道入口节点和出口节点的 IP 地址，以及既不是源也不是最终目标的 IP 地址</li>
<li><strong>出口节点</strong> 仅知道中继节点和最终目标地址，它是在到达最终目标地址之前解密流量的节点</li>
</ol>
<p>中继节点在这个交换过程中扮演一个关键的角色，因为它在源请求和目标地址之间创建了一个加密的障碍。甚至在意图偷窥你数据的对手控制了出口节点的情况下，在他们没有完全控制整个 Tor 中继链的情况下仍然无法知道请求源在哪里。</p>
<p>只要存在大量的中继节点，你的隐私被会得到保护 —— 这就是我为什么真诚地建议你，如果你的家庭宽带有空闲的时候去配置和运行一个中继节点。</p>
<h4><a href="#考虑去做-tor-中继时要记住的一些事情"></a>考虑去做 Tor 中继时要记住的一些事情</h4>
<p>一个 Tor 中继节点仅发送和接收加密流量 —— 它从不访问任何其它站点或者在线资源，因此你不用担心有人会利用你的家庭 IP 地址去直接浏览一些令人担心的站点。话虽如此，但是如果你居住在一个提供匿名增强服务anonymity-enhancing services是违法行为的司法管辖区的话，那么你还是不要运营你的 Tor 中继节点了。你还需要去查看你的互联网服务提供商的服务条款是否允许你去运营一个 Tor 中继。</p>
<h3><a href="#需要哪些东西"></a>需要哪些东西</h3>
<ul>
<li>一个带完整外围附件的树莓派（任何型号/代次都行）</li>
<li>一张有 <a href="https://www.raspberrypi.org/downloads/raspbian/">Raspbian Stretch Lite</a> 的 SD 卡</li>
<li>一根以太网线缆</li>
<li>一根用于供电的 micro-USB 线缆</li>
<li>一个键盘和带 HDMI 接口的显示器（在配置期间使用）</li>
</ul>
<p>本指南假设你已经配置好了你的家庭网络连接的线缆或者 ADSL 路由器，它用于运行 NAT 转换（它几乎是必需的）。大多数型号的树莓派都有一个可用于为树莓派供电的 USB 端口，如果你只是使用路由器的 WiFi 功能，那么路由器应该有空闲的以太网口。但是在我们将树莓派设置为一个“配置完不管”的 Tor 中继之前，我们还需要一个键盘和显示器。</p>
<h3><a href="#引导脚本"></a>引导脚本</h3>
<p>我改编了一个很流行的 Tor 中继节点引导脚本以适配树莓派上使用 —— 你可以在我的 GitHub 仓库 <a href="https://github.com/mricon/tor-relay-bootstrap-rpi">https://github.com/mricon/tor-relay-bootstrap-rpi</a> 上找到它。你用它引导树莓派并使用缺省的用户 <code>pi</code> 登入之后，做如下的工作：</p>
<pre><code class="hljs stata">sudo apt-get install -y git
git clone https:<span class="hljs-comment">//github.com/mricon/tor-relay-bootstrap-rpi</span>
<span class="hljs-keyword">cd</span> tor-relay-<span class="hljs-keyword">bootstrap</span>-rpi
sudo ./<span class="hljs-keyword">bootstrap</span>.<span class="hljs-keyword">sh</span>

</code></pre><p>这个脚本将做如下的工作：</p>
<ol>
<li>安装最新版本的操作系统更新以确保树莓派打了所有的补丁</li>
<li>将系统配置为无人值守自动更新，以确保有可用更新时会自动接收并安装</li>
<li>安装 Tor 软件</li>
<li>告诉你的 NAT 路由器去转发所需要的端口（端口一般是 443 和 8080，因为这两个端口最不可能被互联网提供商过滤掉）上的数据包到你的中继节点</li>
</ol>
<p>脚本运行完成后，你需要去配置 <code>torrc</code> 文件 —— 但是首先，你需要决定打算贡献给 Tor 流量多大带宽。首先，在 Google 中输入 “<a href="https://www.google.com/search?q=speed+test">Speed Test</a>”，然后点击 “Run Speed Test” 按钮。你可以不用管 “Download speed” 的结果，因为你的 Tor 中继能处理的速度不会超过最大的上行带宽。</p>
<p>所以，将 “Mbps upload” 的数字除以 8，然后再乘以 1024，结果就是每秒多少 KB 的宽带速度。比如，如果你得到的上行带宽是 21.5 Mbps，那么这个数字应该是：</p>
<pre><code class="hljs lsl"><span class="hljs-number">21.5</span> Mbps / <span class="hljs-number">8</span> * <span class="hljs-number">1024</span> = <span class="hljs-number">2752</span> KBytes per second

</code></pre><p>你可以限制你的中继带宽为那个数字的一半，并允许突发带宽为那个数字的四分之三。确定好之后，使用喜欢的文本编辑器打开 <code>/etc/tor/torrc</code> 文件，调整好带宽设置。</p>
<pre><code class="hljs lsl">RelayBandwidthRate <span class="hljs-number">1300</span> KBytes
RelayBandwidthBurst <span class="hljs-number">2400</span> KBytes

</code></pre><p>当然，如果你想更慷慨，你可以将那几个设置的数字调的更大，但是尽量不要设置为最大的出口带宽 —— 如果设置的太高，它会影响你的日常使用。</p>
<p>你打开那个文件之后，你应该去设置更多的东西。首先是昵称 —— 只是为了你自己保存记录，第二个是联系信息，只需要一个电子邮件地址。由于你的中继是运行在无人值守模式下的，你应该使用一个定期检查的电子邮件地址 —— 如果你的中继节点离线超过 48 个小时，你将收到 “Tor Weather” 服务的告警信息。</p>
<pre><code class="hljs css"><span class="hljs-selector-tag">Nickname</span> <span class="hljs-selector-tag">myrpirelay</span>
<span class="hljs-selector-tag">ContactInfo</span> <span class="hljs-selector-tag">you</span>@<span class="hljs-keyword">example</span>.<span class="hljs-keyword">com</span>

</code></pre><p>保存文件并重引导系统去启动 Tor 中继。</p>
<h3><a href="#测试它确认有-tor-流量通过"></a>测试它确认有 Tor 流量通过</h3>
<p>如果你想去确认中继节点的功能，你可以运行 <code>arm</code> 工具：</p>
<pre><code class="hljs armasm"><span class="hljs-symbol">sudo</span> -u debian-tor <span class="hljs-meta">arm</span>

</code></pre><p>它需要一点时间才显示，尤其是在老板子上。它通常会给你显示一个表示入站和出站流量（或者是错误信息，它将有助于你去排错）的柱状图。</p>
<p>一旦你确信它运行正常，就可以将键盘和显示器拔掉了，然后将树莓派放到地下室，它就可以在那里悄悄地呆着并到处转发加密的比特了。恭喜你，你已经帮助去改善隐私和防范在线的恶意跟踪了！</p>
<p>通过来自 Linux 基金会和 edX 的免费课程 <a href="https://training.linuxfoundation.org/linux-courses/system-administration-training/introduction-to-linux">"Linux 入门"</a> 来学习更多的 Linux 知识。</p>
<hr>
<p>via: <a href="https://www.linux.com/blog/intro-to-linux/2018/6/turn-your-raspberry-pi-tor-relay-node">https://www.linux.com/blog/intro-to-linux/2018/6/turn-your-raspberry-pi-tor-relay-node</a></p>
<p>作者：<a href="https://www.linux.com/users/mricon">Konstantin Ryabitsev</a> 选题：<a href="https://github.com/lujun9972">lujun9972</a> 译者：<a href="https://github.com/qhwdw">qhwdw</a> 校对：<a href="https://github.com/wxy">wxy</a></p>
<p>本文由 <a href="https://github.com/LCTT/TranslateProject">LCTT</a> 原创编译，<a href="https://linux.cn/">Linux中国</a> 荣誉推出</p>

          
{{< /raw >}}

# 版权声明
原文链接: [https://www.zcfy.cc/article/turn-your-raspberry-pi-into-a-tor-relay-node](https://www.zcfy.cc/article/turn-your-raspberry-pi-into-a-tor-relay-node)
原文标题: 将你的树莓派打造成一个 Tor 中继节点
本文仅用于学习、研究和交流目的。转载请注明出处、完整链接以及原作者。 

本文资源来源互联网，仅供学习研究使用，版权归该资源的合法拥有者所有，

原作者若认为本站侵犯了您的版权，请联系我们，我们会立即删除！
