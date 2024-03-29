---
title: '物联网助长了 Linux 恶意软件' 
date: 2019-01-22 2:30:08
hidden: true
slug: y3qd5rcvp1
categories: [reprint]
---

{{< raw >}}

            <h1><a href="#物联网助长了-linux-恶意软件"></a>物联网助长了 Linux 恶意软件</h1>
<p>针对 Linux 系统的恶意软件正在增长，这主要是由于连接到物联网设备的激增。</p>
<p>这是网络安全设备制造商 <a href="http://www.watchguard.com/">WatchGuard Technologies</a> 上周发布的的一篇报告中所披露的。</p>
<p>该报告分析了全球 26,000 多件设备收集到的数据，今年第一季度的前 10 名恶意软件中发现了三个针对 Linux 的恶意软件，而上一季度仅有一个。</p>
<p>WatchGuard 的 CTO Corey Nachreiner 和安全威胁分析师 Marc Laliberte 写道：“Linux 上的攻击和恶意软件正在兴起。我们相信这是因为 IoT 设备的系统性弱点与其快速增长相结合的结果，它正在引导僵尸网络的作者们转向 Linux 平台。”</p>
<p>他们建议“阻止入站的 Telnet 和 SSH，以及使用复杂的管理密码，可以防止绝大多数潜在的攻击”。</p>
<h3><a href="#黑客的新大道"></a>黑客的新大道</h3>
<p>Laliberte 观察到，Linux 恶意软件在去年年底随着 Mirai 僵尸网络开始增长。Mirai 在九月份曾经用来攻击部分互联网的基础设施，迫使数百万用户断线。</p>
<p>他告诉 LinuxInsider，“现在，随着物联网设备的飞速发展，一条全新的大道正在向攻击者们开放。我们相信，随着互联网上新目标的出现，Linux 恶意软件会逐渐增多。”</p>
<p>Laliberte 继续说，物联网设备制造商并没有对安全性表现出很大的关注。他们的目标是使他们的设备能够使用、便宜，能够快速制造。</p>
<p>他说：“开发过程中他们真的不关心安全。”</p>
<h3><a href="#轻易捕获"></a>轻易捕获</h3>
<p><a href="http://www.alertlogic.com/">Alert Logic</a> 的网络安全布道师 Paul Fletcher 说，大多数物联网制造商都使用 Linux 的裁剪版本，因为操作系统需要最少的系统资源来运行。</p>
<p>他告诉 LinuxInsider，“当你将大量与互联网连接的物联网设备结合在一起时，这相当于在线大量的 Linux 系统，它们可用于攻击。”</p>
<p>为了使设备易于使用，制造商使用的协议对黑客来说也是用户友好的。</p>
<p>Fletcher 说：“攻击者可以访问这些易受攻击的接口，然后上传并执行他们选择的恶意代码。”</p>
<p>他指出，厂商经常给他们的设备很差的默认设置。</p>
<p>Fletcher 说：“通常，管理员帐户是空密码或易于猜测的默认密码，例如 ‘password123’。”</p>
<p><a href="http://www.sans.org/">SANS 研究所</a> 首席研究员 Johannes B. Ullrich 表示，安全问题通常“本身不是 Linux 特有的”。</p>
<p>他告诉 LinuxInsider，“制造商对他们如何配置这些设备不屑一顾，所以他们使这些设备的利用变得非常轻易。”</p>
<h3><a href="#10-大恶意软件"></a>10 大恶意软件</h3>
<p>这些 Linux 恶意软件在 WatchGuard 的第一季度的统计数据中占据了前 10 名的位置：</p>
<ul>
<li>Linux/Exploit，它使用几种木马来扫描可以加入僵尸网络的设备。</li>
<li>Linux/Downloader，它使用恶意的 Linux shell 脚本。Linux 可以运行在许多不同的架构上，如 ARM、MIPS 和传统的 x86 芯片组。报告解释说，一个为某个架构编译的可执行文件不能在不同架构的设备上运行。因此，一些 Linux 攻击利用 dropper shell 脚本下载并安装适合它们所要感染的体系架构的恶意组件。</li>
<li>Linux/Flooder，它使用了 Linux 分布式拒绝服务工具，如 Tsunami，用于执行 DDoS 放大攻击，以及 Linux 僵尸网络（如 Mirai）使用的 DDoS 工具。报告指出：“正如 Mirai 僵尸网络向我们展示的，基于 Linux 的物联网设备是僵尸网络军队的主要目标。</li>
</ul>
<h3><a href="#web-服务器战场"></a>Web 服务器战场</h3>
<p>WatchGuard 报告指出，敌人攻击网络的方式发生了变化。</p>
<p>公司发现，到 2016 年底，73％ 的 Web 攻击针对客户端 - 浏览器和配套软件。今年头三个月发生了彻底改变，82％ 的 Web 攻击集中在 Web 服务器或基于 Web 的服务上。</p>
<p>报告合著者 Nachreiner 和 Laliberte 写道：“我们不认为下载式的攻击将会消失，但似乎攻击者已经集中力量和工具来试图利用 Web 服务器攻击。”</p>
<p>他们也发现，自 2006 年底以来，杀毒软件的有效性有所下降。</p>
<p>Nachreiner 和 Laliberte 报道说：“连续的第二个季度，我们看到使用传统的杀毒软件解决方案漏掉了使用我们更先进的解决方案可以捕获的大量恶意软件，实际上已经从 30％ 上升到了 38％ 漏掉了。”</p>
<p>他说：“如今网络犯罪分子使用许多精妙的技巧来重新包装恶意软件，从而避免了基于签名的检测。这就是为什么使用基本的杀毒软件的许多网络成为诸如赎金软件之类威胁的受害者。”</p>
<hr>
<p>作者简介:</p>
<p>John P. Mello Jr.自 2003 年以来一直是 ECT 新闻网记者。他的重点领域包括网络安全、IT问题、隐私权、电子商务、社交媒体、人工智能、大数据和消费电子。 他撰写和编辑了众多出版物，包括“波士顿商业杂志”、“波士顿凤凰”、“Megapixel.Net” 和 “政府安全新闻”。</p>
<hr>
<p>via: <a href="http://www.linuxinsider.com/story/84652.html">http://www.linuxinsider.com/story/84652.html</a></p>
<p>作者：<a href="">John P. Mello Jr</a> 译者：<a href="https://github.com/geekpi">geekpi</a> 校对：<a href="https://github.com/wxy">wxy</a></p>
<p>本文由 <a href="https://github.com/LCTT/TranslateProject">LCTT</a> 原创编译，<a href="https://linux.cn/">Linux中国</a> 荣誉推出</p>

          
{{< /raw >}}

# 版权声明
本文资源来源互联网，仅供学习研究使用，版权归该资源的合法拥有者所有，

本文仅用于学习、研究和交流目的。转载请注明出处、完整链接以及原作者。

原作者若认为本站侵犯了您的版权，请联系我们，我们会立即删除！

## 原文标题
物联网助长了 Linux 恶意软件

## 原文链接
[https://www.zcfy.cc/article/iot-fuels-growth-of-linux-malware](https://www.zcfy.cc/article/iot-fuels-growth-of-linux-malware)

