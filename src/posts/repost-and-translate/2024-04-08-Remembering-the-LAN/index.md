# 【转载】怀念以前的局域网

> 原文：[Remembering the LAN](https://tailscale.com/blog/remembering-the-lan/)
> 
> 作者：David Crawshaw

这是一段回忆，也是一个梦。

A memory and a dream.

## 当初 (How it was)

我从 1990 年代开始编程，那时我住在我父母的医疗诊所的上方。我们有 15 台商用计算机，有一台是给我的。它们都安装了 MS-DOS 操作系统。最初的网络是通过同轴电缆使用 IPX 连接到 Novell Netware 服务器，这是我们拥有过的最先进的软件。比起 TCP/IP 协议，IPX 是如此地容易配置。没有 DHCP，没有地址分配，但它就是能用。

I started programming in the 1990s living above my parent’s **medical practice**. We had 15 PCs for the business, and one for me. The standard OS was MS-DOS. The network **started off** using IPX over **coax** to a Novell Netware server, the **fanciest** software we ever owned. IPX was so much easier than TCP/IP. No DHCP and address allocation, it just worked.

最终，这些电脑都运行了 Windows 系统，并且有一台 Windows NT 服务器接管了原先通过 TCP/IP 进行的文件共享。这套商用系统在这些变化后依旧可用，只不过需要我们手动分配 IP 地址，这增加了一些操作开销。

Eventually the PCs would run Windows, and a Windows NT server took over file sharing over TCP/IP. The business software survived this **transition** unchanged, though there was more operational **overhead**. We assigned IPs manually.

我家是在澳大利亚北部的一个小镇，那时互联网对我来说还很遥远，甚至比其他地方都要远。最终，我们可以打 2000 英里远的长途电话。而美国人已经有了 AOL。

Home was a small town in Northern Australia. The internet was far off for me at this point, and would remain so longer than it did elsewhere. Eventually we would be able to make long-distance phone calls 2000 miles to try it out for a few minutes here and there. (At this point Americans had AOL.)

在我们有互联网之前，只有一些平庸的本地论坛，还有我父亲某个时间获得的一个当地大学的账号（不知怎么搞的，我们当中没有一个是学生或学校员工），我们可以拨号进去，尝试我的第一个 Unix 系统（运行在 Sun 服务器上）。尽管技术上是通过互联网链接的，这仍是一个有限的体验。我与大学文化的距离意味着直到 90 年代中期才接触到 Linux，当时我们在去香港的旅行中拿到了一份 Slackware。我真正理解 Unix 是在用了 OpenBSD 之后，它为我凑齐了最后一块拼图，让我最终理解了 Unix。

Before we had internet there were some **lackluster** local BBSs, and at one point a local university account my father acquired (**somehow or other**, none of us were students or university employees) that we could dial into and try out my first Unix on a Sun box. It was a limited experience even though technically it was on an internet link. My distance from university culture meant I wouldn’t see Linux until the mid-90s, when we picked up a copy of Slackware on a trip to Hong Kong. I didn’t really **get** Unix until I used OpenBSD, which put enough of the pieces together for me for Unix to finally make sense.

直到 21 世纪初，我有十多年都没有在 Sun 公司的服务器上看到过 root，那时我在伯克利二手市场上买了六台 UltraSPARC 服务器，大约花了 100 美元，这对当时的我来说是一大笔钱。我用这些旧机器组装了一台可用的机器，并用它编写了一个 sparc64 C 编译器后端。尽管从那以后又过去了很多时间，但是对我来说，同时在脑海中保持这两种生活是很难的。它们是完全不同的世界。

I wouldn’t see root on a Sun box for more than a decade, now the early 2000s, when I bought half a dozen UltraSPARC servers as **a lot (一批)** second hand in Berkeley (for around $100, a lot of money for me at the time). I assembled a working machine from the carcasses and used it to write a sparc64 C compiler backend. Even though more time has passed since then between these times, it is hard for me to hold both lives in my head **simultaneously**. They were different worlds.

## 童年的魔法 (The childhood magic)

局域网是一个学习计算机的神奇地方。除了在物理层面上组装和拆卸机器，我可以安全地做一些在现代互联网上难以想象的事情：无需许可的文件共享，没有安全性的实验服务器，共享软件，任何一台机器都可以通过输入一个无害的命令轻松地将网络搞崩。即使我真的搞崩了网络，影响也永远不会超出我家的范围 —— 我知道我需要向谁道歉了。

The LAN was a magical place to learn about computers. Besides the physical aspect of **assembling** and disassembling machines, I could safely do things unthinkable on the modern internet: permission-less file sharing, experimental servers with no security, shared software where any one machine could easily bring down the network by typing in an **innocuous** command. Even when I did bring down the network the impact never left the building. I knew who I had to apologize to.

（20 年后，当我在谷歌担任工程师时，我用一个配置错误的 MapReduce 搞垮了一个 borgmaster，我都不知道要向谁发送道歉邮件。）

(Two decades later when I **took down** a borgmaster with a misconfigured MapReduce as an engineer at Google, I could not figure out who should get an apology email.)

在局域网环境下，简单的事情变得容易，一些困难的事情变得可能。有些高级的解释语言使得用户界面变得简单直接，还有一些令人生畏的语言能够让计算机真正发挥光芒。

With our LAN easy things were easy, and some hard things were possible. There were high-level interpreted languages where UIs were **straightforward**, and scary languages which could make the computer really shine.

一个 200MHz 的奔腾 Pro 处理器感觉速度**非常快**，而 32MB 的内存似乎能做任何事。到了我用 OpenBSD 的时候，只要加入一些不太成熟的想法，我就能在几分钟内重新编译 Apache httpd。如今，只要我**坚持使用** GCC 2.95，我的手表都能更快地编译它。

A 200MHz Pentium Pro felt **blazing** fast and 32MB of RAM could do anything. By the time I had OpenBSD I could recompile Apache httpd in a few minutes with my own bad ideas. My **wrist (手腕)** watch could compile it faster today, as long as I **stuck** to GCC 2.95.

后来，我会把一台 PC 带到朋友家里，我们会组建**临时的**局域网，玩星际争霸之类的游戏。 (阴极射线显示器很重)。LAN 是一种教育，也是一种生活方式。

Later I would carry a PC to houses of my friends where we would build **ephemeral** LANs and play games like Starcraft. (Cathode-ray monitors were heavy.) The LAN was an education and a lifestyle.

## 小型企业的魔力 (The small business magic)

我父亲是一名全科医生，他用这套廉价的 286、386 和 486（配有三台昂贵的激光打印机）来为自己的小诊所编写医疗记录软件。这套软件被十几名医生、一名护士和一名接待员使用。可以用这套基于文件的数据库软件（在这种情况下是 Clipper）和一个无鼠标的 curses 界面做很多事情。

My father, a general **practitioner**, used this infrastructure of cheap 286s, 386s, and 486s (with three expensive laser printers) to write the medical record software for the business. It was used by a dozen doctors, a nurse, and receptionist. You can do a lot with file-based database software (in this case, Clipper) and a mouse-less curses interface.

关于这一点有几个令人惊讶的事实。作为一名工程师，令我惊讶的是，Netware 文件锁定和 SMB 文件锁定就足以实现一个被大约 15 个并发用户使用的数据库。我怀疑现在大多数的职业程序员从未使用过文件锁定，更不用说看到它正确地工作了。

There are several **astonishing** facts about this. As an engineer, it is astonishing that Netware file locking, then SMB file locking, worked well enough to implement a database used by ~15 **concurrent** users. I suspect most career programmers today have never used file locking, **let alone** seen it work correctly.

更令人惊讶的是，这是一个非程序员专业人士，他能够在白天的工作之余使用从书中学到的技能构建软件来运行他们的小型企业。这是一个非常令人鼓舞的故事。

The business story is even more astonishing. Here is a non-programming professional, who was able to build the software to run their small business in between **shifts** at their day job using skills learned from a book.

如今，专业人士当然可以学会构建一个 CRUD 应用程序，但他们会发现不懈地调整他们的软件来最大限度地减少接待员接待一个病人需要使用的按键次数，或支持磁卡阅读器、教激光打印机精确地在专用处方纸上打印（打印机使用 Postscript，但 MS-DOS 编程语言有一个更简单的指令层，使这成为可能）是多么困难。

Today a professional could surely **pick up** the skills to build a CRUD app, but they would **be hard pressed to** tune their software so **relentlessly** to minimize the keystrokes a receptionist needs to use to check-in a patient, or support a magnetic card reader, or teach laser printers to precisely print onto specialized **prescription** paper (the printers spoke postscript, but the MS-DOS programming language had an easier instruction layer over PS that made this possible).

90 年代的结果是，父亲的诊所需要的员工数量比常人认为的都要少，医生的效率比使用任何其他软件的都要高，所以生产力提高了。

The result in the 90s was the business needed fewer staff than everyone thought a medical practice of that size required, doctor’s time was used more efficiently than any other software allowed, so productivity increased.

我父亲作为一个兼职程序员优化他的小诊所，比他作为一个看病医生赚的钱还要多。

My father made more money as a part-time programmer optimizing his small business than he did as a doctor seeing patients.

## 那么现在 (How it is)

如果我的童年发生在现在的话，那么很多新事物都将成为可能。我可以用 JavaScript 轻松绘制高质量的图形。虽然不清楚这是否会比我在 BASIC 中玩的像素化大猩猩和香蕉更有吸引力。我可以为手机开发应用程序。至少在理论上是这样。实际上，我小时候对慢编译器不太耐心，成年后，我仍然很难忍受应用程序的开发环境，所以这是不可能的。

If my 90s childhood were transported to today, so many new things would be possible. I could draw high-quality graphics easily with JavaScript. It is not clear that would be more **compelling** than the pixelated gorillas and bananas I played with in BASIC. I could develop apps for my phone. In theory at least. In practice, I wasn’t particularly patient with slow compilers as a kid, and as an adult I still have trouble **stomaching** the development environment for apps, so that’s off the table.

我不会建一个玩具网站来放学校的东西，因为我有 facebook 可以用。

I wouldn’t build a toy website to put school **stuff** on, because I would have facebook for that.

和朋友联机将更加容易。我们不需要搬运沉重的电脑，也不需要学习调试 TCP/IP 配置，甚至不需要亲自见面就能玩。我猜有些人会认为这是一种进步：更多的糖果，更少的内容。

Games would be easier to play with friends. We wouldn’t have to **lug** heavy boxes or learn to debug our TCP/IP configuration or actually see each other in person to play. I guess some people would see that as an improvement: more candy, less content.

所有的技术都变得更好了，学习资源也更多了。但对我来说，我不确定我现在是否还会编程。学习如何存储密码或为你的玩具网站添加 OAuth2 并不有趣。当今的编程工作很大一部分是繁琐的，或者就是在疯狂的互联网中玩攻防对战。你可以做更多的事情，但是你很难有动力开始编写有趣的协作软件，而最终你会使用一些半成品的 SaaS。

All the technology is better. The resources to learn are better. But it is not clear to me I would program at all today. Learning how to store passwords or add OAuth2 to your toy web site is not fun. So much of programming today is busywork, or playing defense against a **raging** internet. You can do so much more, but the activation energy required to start writing fun collaborative software is so much higher you **end up** using some half-baked SaaS instead.

那么，我父亲呢？

What about my father?

如今，像我父亲这样的兼职程序员能写小企业软件吗？他能让它像我们的局域网那样安全和高效吗？也许可以吧。只要他足够精明，坚持使用 90 年代老式的桌面电脑，并将机器物理隔离于互联网。但是，以我父亲投入项目的时间，没办法安全（甚至在 HIPAA 法规下合法）地将记录转移到现代手机上。

Could a part-time programmer like my father write small-business software today? Could he make it as safe and productive as our LAN was? Maybe. If he was canny, and stuck to old-fashioned desktops of the 90s and physically isolated the machines from the internet. But there is no chance you could get the records onto a modern phone safely (or even legally under HIPAA) with the hours my father gave the project.

如果今天面临亲自编程还是购买的决策，我强烈怀疑他会选择购买。或者更有可能的是，订阅。诊所的生产力会因此降低。

If **confronted** with the build v. buy decision today, I strongly suspect he would buy. Or even more likely, subscribe. The practice would be less productive for it.

全世界的程序员们构建了这个奇妙的充满了魔法的互联网。免费的跨洲视频通话。从云服务提供商那里免费获得的 “微型” 虚拟机，其处理能力和内存比我开始编程时能买到的任何东西都要强大。

The programmers of the world have built this fantastic internet, full of magic. Free inter-continental video calls. “Micro” VMs available for free from Cloud providers with more processing power and memory than anything I could have bought when I started programming.

对于我们所有掌握的技能，某些东西已经丢失了。如果说在 1990 年代编程一个局域网就像在乡下自由自在地照料一个花园，那么今天在互联网上编程就像在麦迪逊大道的中城区里照料一个花箱。任何人都可以体验你的作品。你的耕作也会被成千上万的过路人评判，其中任何一个人都可能因为他们遛的狗没有接受过城市训练而毁掉你的作品。

For all our mastery, something has been lost. If programming a LAN in the 1990s was the care-free tending to a garden in the countryside, then programming on the internet today is tending a planter box on Madison Avenue in midtown. Anyone can experience your work. You will also have your **tilling** judged by thousands of passersby, any of whom may ruin your work because the dog they’re walking hasn’t been city trained.

## 愿景：未来会怎样？(A dream: How it will be)

在某些时刻，正确的事情会相遇，创造出一些特别的东西。许多这样的时刻是短暂的，不会重复，注定只能被铭记。

In some moments the right threads of change meet and create something special. Many of these moments are short-lived and will not repeat, destined to be, at best, remembered.

使用小型可信网络和简易程序的那段美妙时光不必被遗忘。通过一些努力，我们可以用技术来重现这种美好。

The magic moment of small trusted networks and care-free programs does not need be **relegated** to memory. With enough work, we can bend technology to recreate the magic.

我们可以再次拥有类似 90 年代的局域网体验，同时我们可以加入 21 世纪互联网的最佳部分。一个我们信任的小范围的安全空间，在这里，我们可以远离亿万互联网用户的窥视进行编程。在这里，通过良好的身份认证服务和优秀的加密技术，我们可以抵御明显的恶意行为。

We can have the LAN-like experience of the 90’s back again, and we can add the best parts of the 21st century internet. A safe small space of people we trust, where we can program away from the prying eyes of the multi-billion-person internet. Where the outright villainous will be kept at bay by good identity services and good crypto.

广义上的虚拟化网络的概念早就存在：虚拟专用网络。新的协议使 VPN 比以前更好，WireGuard 正在开创一种易用和高效的对等隧道。将 VPN 与身份结合起来，使其在任何地方都能工作，你可以拥有一个由你所有 21 世纪设备组成的虚拟 90 年代风格的局域网。让互联网只是一个管道，让你的终端根据另一端的人来决定他们将与谁交流。

The broader concept of virtualizing networks has existed forever: the Virtual Private Network. New protocols make VPNs better than before, WireGuard is **pioneering** easy and efficient tunneling between peers. Marry the VPN to identity, and make it work anywhere, and you can have a virtual 90s-style LAN made up of all your 21st century devices. Let the internet be the dumb pipe, let your endpoints determine who they will talk to based on the person at the other end.

因此，我们得到了一个系统，它结合了现代互联网的特性，重新营造出了 90 年代局域网那种令人愉快且简单的编程环境：

The result is a system with properties that work with today’s internet to give us the pleasant, simple programming environment of the ’90s LAN:

- 使用你选择的全球互联网身份系统进行身份验证，并在 IP 级别进行加密授权。
- 密钥会自动生成并自动轮换。
- 人们直接映射到无法伪造的 IP 地址。
- 在你的网络上运行自定义服务器，只有网络上的人才能访问。
- 你的数据受到小团体的简单而强大的社会动态保护。

- Use the global internet identity system of your choice for authentication, and do cryptographic authorization at the IP level.
- Keys are generated and rotated for you automatically.
- People map directly to unspoofable IP addresses.
- Run custom servers on your network and access is limited to only those people on the network.
- Your data is protected by the simple yet powerful social dynamics of small groups.

我们可以实现这一切！

We can build this.

首先，我们必须证明用户体验创造了我们想要的简单编程环境。这意味着要让产品到达客户手中，并让他们感到满意。这是 Tailscale 目前的重点，打造一个优秀的产品并让客户满意。

First, we have to prove that the user experience creates the environment we want for simpler programming. That means getting the product in the hands of customers and making them happy. This is our current focus at Tailscale, build a great product and make customers happy.

其次，我们需要稳定并发布用于构建这种网状覆盖网络的协议，以便它可以在任何地方使用。

Second, we need to stabilize and publish the protocols used to build this mesh overlay network so that it can be used anywhere.

最后，我需要帮助那些从未在安全环境中体验过简单、愉快编程的新程序员理解编程可以是有趣的。你可以设置一个让你专注创造的环境。为你的朋友编写一个 Web 服务不应该是一种网络战斗，你不应该花费你的时间担心 XSS 攻击或缓冲区溢出。你应该专注于在一个没有坏人纠缠的地方创造新的、美好的东西。

Third, I need to help new programmers who never got to experience simple, pleasurable programming in a safe environment understand that programming can be fun. You can set up your environment so you can focus on being creative. Writing a web service for use by your friends should not be a form of combat, where you spend your days worrying about XSS attacks or buffer overflows. You should be focused on creating something new and wonderful in a place without bad people **hounding** you.

我们将在今天的互联网之上，重建 90 年代的局域网（以及 BBS 和 MUD）作为一个网状网络的世界。

We are going to rebuild the LANs (and BBSs and MUDs) of the 90s as a world of mesh networks on top of today’s internet.