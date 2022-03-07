# ChromeTranslate
chrome book translate

## Part one

今天，我们使用网络来做的事情已经不仅仅是网页，而是应用程序

人们观看并上传视频，聊天，玩网页游戏

所有的这些事情在第一个浏览器诞生时，是不存在的

wouldn't it be great,then, to start from scratch

当一切开始时并没有这么神奇，让我们从头开始

让我们看看chrome是如何设计成满足今天的应用程序和用户需要的

---

首先，浏览器需要更稳定。当你写一个重要的电邮或编辑文件，浏览器崩溃是一件大事。

浏览器也需要要快。它们需要要启动得更快，加载页面更快,

——对于网页应用来说，javascript本身可以更快。

they need to be more secure, given what's known about mass browser exploits

考虑到目前已知的大规模浏览器漏洞，它们需要更加安全,需要做一些架构改变以抵御恶意软件

我们要在更多功能和简洁界面之间找到一个最佳平衡点，有一个干净、简单、高效的用户界面

最后，google chrome是一个完全开源的浏览器

我们期望其他人能从我们这里得到启发

就像我们从其他人那里得到启发

---

当我们开始这个项目时，有一些工程师说浏览器的单线程是首先要解决的问题

例如，一旦Javascript开始执行，浏览器就啥事都干不了，只能等着Javascript返回控制权给浏览器

所以开发者只能将API写成异步的

你干完了就call我

ok，没问题

否则一不小心浏览器就进入等待状态....

繁忙中....

---

工程师们设计了一种多线程的浏览器，好吧，让我们看看多线程会带来什么

如果我们有了多线程，那么每个线程都有属于自己的内存和全局数据结构拷贝

进程

chrome 进程管理

我们使用与现代操作系统相同的多线程隔离技术

于是，每个进程渲染不同的TAB页

---

现在你也有了单独的JAVASCRIPT线程

如果一个TAB繁忙,你仍然可以使用其他的TAB页

并且如果一个TAB页上出现bug(我们几乎不可能消灭所有的bug),我们也只丢失了一个TAB页

当一个TAB崩溃时会出现一个“悲伤标签”，但它没有使整个浏览器崩溃。

这个就是“悲伤标签”

一个多进程设计意味着使用一点更多的内存。每个进程都有一个固定的额外内存占用

但随着时间的推移，它也意味着更少的内存膨胀。

在传统的浏览器中，您只有一个进程和一个地址空间，你一直在其中加载网页页面。

当你有太多标签页打开，可以关闭一些释放内存

当你打开一个新Tab页，使用的内存是之前使用过的。

---

但随着时间的推移，一小片一小片的内存碎片出现了(太小有一定概率不能被使用，又占用空间)，虽然曾经使用这些内存碎片的TAB页已经被关闭了

either we have memory that nothing can refer to again, or there's a piece of de-allocated memory we still have pointers to

要么我们的内存没有任何东西可以再次引用，要么就是有一块被取消分配的内存仍然有指向的指针

最后总会发生,当浏览器想要打开一个新标签时，却没有足够大块的内存空间,

于是操作系统只能给浏览器分配更多的地址空间

直到总有一个时刻，浏览器的生命必须结束

快点啊

关几个TAB页

我已经关了，但是没用

但是当google  chrome的TAB页被关闭时，你得到的是整个进程的--

---

--内存，所有的一整块内存都被释放

现在打开一个新标签页,一切都是重新开始

当你浏览的时候，我们一直创建和消灭进程。如果有一个疯狂的内存膨胀存在，也不会影响你太久，因为你不经意间可能会关闭TAB页，从而让内存释放

下面我们讨论第一步，假设你从域名A导航到域名B，这两个域名站点没有联系

所以我们可以完全抛弃旧的渲染引擎，旧的数据结构，旧的进程

甚至整个TAB页，我们都可以扔进回收站，回收整个进程

---

就像用你的操作系统，你可以看看的google chrome的任务管理器，哪些网站使用最多的内存，下载最多的字节，占用最多的CPU

为什么这个应用正在下载整个因特网？

你也可以看见扩展程序的进程，它们在任务管理器中作为独立的进程而存在

这样当浏览器变慢时，你可以清楚地看见时谁导致的，为什么变慢

于是你就可以干掉它

placing blame where blame belongs

不作恶就不会死

---

google chrome是一种巨大的、复杂的产品,会加载数十亿不同的网络页面,所以测试是至关重要的

幸运的是,在google,我们有一个同样巨大的基础设施用于WEB页面爬虫

浏览器每次构建的20-30分钟内，我们就可以在成千上万个不同的网页上测试它。

每周，“chrome 测试机器人”都要测试数百万次的页面，尽早给我们的开发人员测试结果，否则他们就要等待BEAT测试。

关键是尽早地发现bug，发现得越早，解决的成本就越小，如果过了几天，bug就难以跟踪了

更早地发现bug也有利于工程师解决它们，工程师会说，“OH，这个bug是这种模式的一部分”,下一次，他们就不会这样干了

---

10亿个

当然，现在有数十亿个网页，也许是数万亿个。如果每个构建只测试一百万个网页，我们如何选择呢？

幸运的是，我们对这个问题也有一个专门的处理

我们已经对一般用户最有可能访问的页面进行了排序

至少我们将确保chrome在人们每天访问各种网站上运行得很好

针对每次发布我们有几种测试方法，既有针对代码片段的单元测试--

--也有针对用户行为的自动化脚本测试--

--还有混沌测试，例如随机输入一些乱七八糟的东西

---

in layout testing, webkit found that producing a schematic of what the browser thinks it's displaying is a more precise way to compare layouts than taking screenshots and creating a cryptographic hash

在布局测试中，webkit发现，将浏览器认为自己正在显示的内容制作成示意图，是一种比截图和创建加密哈希值更精确的比较布局的方法

when we started we were passing 23% of webkit's layout tests. moving from there to 99% has been a fun challenge and an interesting example of test-driven design

当我们开始的时候，我们只通过了23%的webkit布局测试。后来到达了99%，这是一个有趣的挑战，也是测试驱动设计的一个生动案例

我们在自动化测试方面能够做的很有限

例如，我们不能测试需要密码的网站

人们使用浏览器也不一定按照设计者所设计的方式

很难达到100%，但这是我们尽力做的目标

我不关心少数几个很酷的功能，我要做的是把这个产品打造成岩石一样牢固

---

google chrome所使用的渲染引擎是WEBKIT,它是开源的

它的速度之快给我们留下了深刻印象

我们曾经问过在google内部的android团队，“你们为什么使用webkit？”

他们回答，webkit能更有效率地使用内存，很适合嵌入式设备，对于新的浏览器开发者很容易掌握和使用

浏览器很复杂，但webit的一项优点就是让浏览器变得简单

---

因为 Javascript 对于今天的网页是如此的重要--

--所以我们为Javascript打造了一个虚拟机--

--准确地说，这是在丹麦的V8团队干的事情

V8团队的成员都是虚拟机方面的专家，如果你想把某种编程语言放入虚拟机，他们都可以告诉你如何编写它

虚拟机提供了安全和平台独立

但之前为Javascript提供的虚拟机是为小型编程模型准备的，所以性能和交互性都不是这个虚拟机所考虑的重点

它当时所考虑的都是非常简单的网页需求

---

但如今，像Gmail这样的网页应用操作起文档对象模型和 Javascript 的需求可不是开玩笑的--

and that simplistic approach to javascript engines isn't enough anymore

--一个简单功能的javascript引擎已经不够用了，我们考虑重新写一个

开始之前我们不是写代码，而且进行头脑风暴，考虑如何让它运行得很快--

--例如引入隐藏类转换(HIDDEN CLASS TRANSITIONS)

Javascript本身是没有类的(kursk注：prototype感觉被无视了)，你可以创造一个对象，然后动态地增加它的属性(kursk注：动态语言的缺点or优点？)

但是在V8中，当程序正在运行时，有相同属性的对象在生命终结之前，可以共享相同的隐藏类，并且我们可以利用这一点做动态优化

another factor in v8's speed is dynamic code generation

v8 速度快的另一个因素是动态代码生成

当其它 javascript 引擎运行时，它查看的是javascript的源代码，所以要先解析(interpret)源代码

---

when you have to do interpretation, you have to look at the structure of your internal representation over and over again

而当做解析时，你必须一遍又一遍地查看内部的内部表征结构(structure of your internal representation)

V8与此不同，V8将浏览器的Javascript源代码转换编译成可以直接在CPU上运行的机器码

当你一次解析和编译机器码时，
这个代码就是你javascript源代码的表征
它不需要被解析，它已经可以运行了

最后一个当前的javascript引擎的核心设计问题是--

--槽糕的垃圾回收机制

javascript和其它的现代面向对象编程语言都有自动内存管理机制

当一个对象没有任何指针引用它时，它就可以被回收了，这就是垃圾回收——一个微不足道的进程

但现在的javascript虚拟机中，使用的是保守的垃圾回收机制--

---

保守意味着你不能精确地知道所有指针的指向

当你遍历执行栈(execution stack)查找看起来像指针的字时

你有时可能会感到疑惑，如果碰巧在对象堆(object heap)中一个整型数(integers)的值与一个对象的地址相同，你就会问，这东西到底是不是指针？

在V8中，我们使用精确的垃圾回收机制，我们精确地知道栈中所有的指针的位置，这给了我们很多便利

一是我们可以通过重写一个指针从而改变一个对象的位置

二是因为我们精确地知道指针的位置，所以可以实现增量垃圾回收

这意味着一种更快的垃圾回收机制，对于100M数据的垃圾回收，原来的方式可能引起秒级暂停，而采用增量垃圾回收机制是非常少的毫秒级

---

这意味这更好的网页应用交互体验，例如更平滑的拖拽操作

V8有一个特别为google chrome准备的API，但V8引擎的核心是与google chrome独立的

所以，其它的浏览器也可以采用V8引擎--

--或者，如果其它项目也使用Javascript，该项目的开发者也可以采用V8

我们希望V8的性能可以成为一个记录，其他开发团队可以继续这个游戏，挑战新的记录

because if you look at any other system that's become faster over time, what happens is that you get bigger, better, more inventive apps

因为如果你回顾一下任何随着时间变得更快的系统，这些应用程序都是变得更大、更好、更有创造性！

---

一 1 - 9

二 10 - 17 

---

在google chrome上用户使用最多的就是TAB页

as soon as we started thinking about it that way, the design naturally followed

所以我们从一开始就打算重点设计TAB页

我们开始着手重构TAB的交互截面，比如把它移到了顶端

我们分离了各个TAB页，实现这个设计很简单，因为浏览器和TAB页的进程是独立的

我们还让TAB页可以从一个窗口拖到另一个窗口

---

因为TAB页是最重要的交互部分，所以每个TAB页都属于自己的控制模块

也有属于自己的地址栏

around here we've been calling the 'OMNIBOX'

在这里，我们一直称之为 "OMNIBOX"

OMNIBOX可不仅仅是地址栏

它还提供了搜索建议，你之前访问的最多的页面，你没有访问过但热度很高的页面......

还包括你所有的搜索历史，如果你昨天发现了一个不错的数字相机的网站，你不必查询书签，

仅仅输入“数字相机”就能快速回到这个网站

---

when the team suggested autocompletion in line, I said I hated it when browsers stick all this crap into a location bar as I'm typing . it's never what I want.

当团队建议在行中提供自动补全的功能时，我说我讨厌浏览器在我打字时把所有这些垃圾塞进位置栏,这绝不是我想要的

but, they said, no, no, it'll be fine, trust us -- and they went on and made it something really compelling...

但是，他们说，不，不，会好的，相信我们 -- 他们继续做了一些非常引人注目的东西......

inline completions will never flicker, never flash. it's perfectly, aesthetically non-distracting

自动补全的内容永远不会闪烁，永远不会闪动，它是完美的，用户使用时不会分散注意力

plus, it'll only autocomplete to something you've explicitly typed before

另外，它只会自动补全你之前明确输入的内容

例如，输入一个"C",再按下回车键，你就可以访问cnn.com了

而不是输入下面这么一堆字符

并且当你在亚马逊、维基百科、google等网站的搜索框上搜索时

你输入的搜索内容也会被本地的系统所保存

所以你可以用不同的方式来搜索这些网站，你只需要在地址栏上输入搜索引擎的名字，使用tab键选择需要的搜索引擎

---

在大多数的浏览器上打开一个新的TAB页时显示的就是你的主页

有些用户为了能快点打开会将空白页作为主页

但用户每次打开一个TAB页都是有目地，说明用户想访问某个地方

可能用户知道要去哪里，可能用户不知道需要搜索

我们设计了一个能快速展示的页面，帮助用户实现行动目标

我们设计的默认页面是一个新TAB页，上面包括9个最近经常访问的网站--
 
--和你经常搜索的网站

---

你可以在这个TAB页输入你要访问网站的URL，google chrome让这个页面不断调整，不断适应你的行为

你打开它后可能会问，浏览器怎么知道我要访问哪里？然而使用一段时间后，你不知不觉中就会认为他是你的浏览器，他应该了解你

google chrome有一个隐私功能，你可以产生一个“隐匿”窗口，这个窗口的所有行为都不会被google chrome记录，甚至不会记录到你的电脑上

TAB页上发生，TAB页上结束

它是只读模式，你仍然可以访问你的书签，但访问历史不会保存到浏览器上--

--当你关闭窗口时，会话时产生的cookies也会被清除

---

want to keep a surprise gift a secret

浏览过程中如果要跳到其它网站，但又想在原窗口继续处于隐匿模式

仅仅需要继续在任何其它窗口浏览就可以了

在chrome中没有弹窗驱动，JAVASCRIPT已经没有办法突然弹一个窗口打断用户

pop-ups are scoped to the tab they came--

弹出式窗口的范围是它们来自的TAB页--

--from and confined there

--并局限于此

如果有时你想接收弹窗--

--只需要把它拖出来，它 只会显示在它自己的窗口上

---

开发者可以调用浏览器交互界面的子窗口

但是我们想减少对子窗口的使用，我们让网页应用开发者能自己打造行云流水般便捷操作的窗口界面，不需要使用浏览器的工具栏和地址栏

我们不想做任何打断用户的事情

如果你使用浏览器的时候并没有意识到浏览器的存在，那么我们的目标达到了！

---

三 18 - 24

---

恶意软件和网络钓鱼对用户来说是个大问题，影响信任和对网络的信心

当我们开始Google Chrome项目时，我们认识到Chrome面临的情况与其他浏览器开始之时很不相同

当时，浏览器的作用是渲染页面和展示很酷的东西，还没有货币交易这种引起恶意软件偷窃的东西

而现在，在经济利益的驱使下，恶意软件大肆偷窃着密码，干着偷钱的勾当

in thinking about security, we began with the assumption that your browser would get compromised, you will eventually encounter malware

在思考安全问题时，我们开始假设你的浏览器会被破坏

你最终将遇到恶意软件

---

使用沙盒，我们的目标是防止恶意软件在您的计算机上安装自己，或者能够干出超出所在的TAB，影响另一个TAB的事情

所以,对于每一个这些进程中我们剥夺了它们所有的权利

它们可以计算，但是不能在你的硬盘上写文件，也不能从敏感的目录中读文件，例如系统盘的用户文档目录和桌面目录

沙盒被做成了一个--

--限制已经存在的进程的--

--监狱

---

这意味着不能监视你输入信用卡的数字

不能与鼠标操作进行交互

不能读你的税务数据

不能让windows操作系统在开机后启动一个程序

有些坏家伙可能在TAB页中运行--

--但只要你一关掉TAB页，他就没戏了

不能影响你的电脑，也不能影响其他的进程

the perimeter of the sandbox is largely based on permission

沙盒的大小主要根据权限

vista uses a modified version of the biba security model which has three levels

vista使用修改过的BIBA安全模型，有三个级别

非常信任

有时信任

从来不信任

---

备份系统、程序更新这类程序使用“非常信任”等级

所有用户正常运行的程序使用“有时信任”等级，例如记事本、纸牌、计算器…

读操作被允许从低向高--

而写操作仅被允许从高向低

通常，从因特网上接收和处理数据的应用程序被安排到两个较低的等级

问题是“有时信任”等级与高等级不同，有很多敏感信息在这里--

--“从来不信任”等级不应该被允许读“有时信任”等级的内容

---

于是在我们的模型中，我们设计了用户与沙盒沟通机制，任何沙盒(沟通对象)首先必须被用户初始化

沙盒只能与创建它的用户沟通，不能与其它用户沟通

因为我们写了所有的chrome安全的代码，所以我们可以控制所有的实现

好吧，我承认，插件(PLUGINS)是一个例外

因为网页应用不仅仅是HTML和Javascript

---

在我们设计的这个权限系统中，google chrome的渲染程序可能运行在非常低的权限等级，而插件却可能运行在相同权限等级或者更高的权限等级

插件有能力不遵守公共的标准，所以我们不能让沙盒运行的权限等级超过chrome的等级

though with some small changes on the part of the pulgin maker, we can get them to run at a lower privilege which would be much much safer

虽然在插件制造者方面做了一些小改动，我们可以让它们在较低的权限下运行，这将是更安全的

and meanwhile, we have a huge surface area reduction in vulnerability, from all this to this

就是这样，我们让产品防御的脆弱点，从整体缩小到仅仅--

--插件上

---

当一个插件与HTML和Javascript组合在一起，它们运行在相同的进程时

如果任何运行崩溃或者内存冲突导致进程异常，它们就都完了

于是，我们将插件从渲染进程中拉了出来，作为一个单独的进程而存在

通过这种设计方式，即使插件进程出现异常，页面部分仍然作为沙盒能正常运行

---

沙盒可以帮助保护用户免受恶意软件侵害，但是他们仍然可以被钓鱼网站欺骗

一个典型的网络钓鱼方案是，攻击者发送邮件说:“我是你的银行，你的账户被攻破了，请给我你的SSN。我可以核实，等等……”

然后他们将用户发送到一个几乎完全相同的银行网站，并开始窃取他们的信息

这一套动作下来一般需要24小时--

--有时会持续一周多的时间

困难的问题是如何在用户损失之前发现欺骗行为

---

google chrome持续下载网站黑名单，一个是钓鱼网站，一个恶意网站

如果你访问了名单中的网站，chrome就会警告你

这项服务是免费的，我们很高兴让所有人使用它，所以提供了公共的API

第二个名单是恶意网站，这些网站只要你一访问，就会在你的电脑上大肆破坏

当我们发现有些网站上被坏人用于传播病毒，我们会提醒网站的所有者，他们不是有意这样的，收到提醒后他们会清除病毒

---

another thing we build into google chrome is gears

我们在google chrome中创建的另一个东西是构件(gears)

gears basically adds an api to your browser -- an extension that improves its capabilities

构件基本上给你的浏览器添加了一个api--一个提高其能力的扩展

从我的的角度来看,google chrome构件从两个方向切入网络

浏览器项目让用户的体验更好

而构件团队使得浏览器对网页开发者更友好

---

今天的浏览器对于开发者的应用有很多限制，这些浏览器具备这些功能，那些浏览器具备那些功能，但是我们想，如果一个浏览器有有一个很酷的功能，如何让其它浏览器也很容易地具备--

--于是我们想如何让开发者能很容易地跨越不同浏览器

构件正是为此为创造出来，他改善了所有浏览器的基本功能，包括google chrome

whatever the advantages of building native apps over web apps, we want to build those enhancements through gears--

无论桌面应用比网络应用有什么优势，我们都希望通过构件来构建这些增强功能--

--并且使得这些功能成为网络时代的新标准

---

所以，开放的标准是让浏览器变得更好的一种好方法

团队也做了一些其它有趣的事情，包括提高速度、改善稳定性和人机交互，例如新的TAB页

这些工作都可能变成标准--

--有些也不会

但是--

--既然它是开源的--

--其它的浏览器开发者就可以从中获得他们想要的

---

他们不用付钱给我们，他们也不必询问我们

他们也不必分享他们基于此的工作成果或者报告bug*

*虽然我们提供系统供他们这么做，只要他们愿意

但他们可以使用我们所提供的代码来构建他们自己的程序

虽然我们可以把这一切都锁起来，只为我们自己服务

但是google生存在互联网上

it's in our interest to make the internet better and without competition we have stagnation

让互联网变得更好符合我们的利益，没有竞争就会停滞不前

这就是为什么我们选择开源的原因，我们需要互联网成为一个充满公平、智慧和安全的地方

---

让所有的浏览器变得功能更强大，是与创造google chrome同样令人激动人心的事情--

--为了让网页能够不断进化，构建现代网页应用的坚实基础

我们为其它开源浏览器，例如MOZILLA和WEBKIT也提供构件

这是我们的贡献，我们希望人们能理解这些想法，调整它们，使用它们，不断改进它们，推进互联网的发展

---

台词：

Google Chrome团队

漫画改编：

Scott McCloud

翻译：

kursk.ye@gmail.com

该文档全部翻译文件可从 https://github.com/kursk-ye/ChromeTranslate 免费获取
传播时请保留原作者，尊重劳动成果