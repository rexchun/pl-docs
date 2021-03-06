﻿# 《UTF-8 遍地开花宣言》批判

[Created](https://gist.github.com/FrankHB/00731fedf07b4ea271afa70a5cdc8d9d/a7be171d43a4182f1a95ac22916ab5f074209215#utf-8-everywhere-manifesto) @ 2016-10-17 01:30, [rev v5](https://gist.github.com/FrankHB/00731fedf07b4ea271afa70a5cdc8d9d/4faddf4ba2bc5a2bbb4aeaf131d9066ad7206e1c#utf-8-everywhere-manifesto) @ 2016-10-17 01:54.

## 概述

以下评论基于[中译版本](http://utf8everywhere.org/zh-cn)。

一些科普常识和结论（例如 UTF-16 因为变长特性导致的无能，以及代码页是不合理的烂设计）是没有问题的。但是，从动机、论证的目的以及参照的依据看，存在相当多的低级错误，应予视为误导。

### 1-3

第一个错误是混淆了“文本”的用途。**文本首先是人类书写系统的直观具象。**文本编码的最初设计也遵循这个基本要点，虽然之后**作为妥协**混入了“控制字符”也已有相当长的历史了，但那只应该是妥协。

文中提到文本作为通信协议表现形式的常用现状。这仍然只是妥协。注意通常称为二进制编码的非文本是无法被文本取代的，因为严格地来说文本充其量只是二进制编码的一种。原用于反映书写系统、之后作为妥协折衷产物的这种形式具有非常多不适合编码的普遍糟糕特性（例如浪费资源），只是体现不合理的 worse is better 渣设计的历史包袱的实例而已，其被普遍接受并非来自于自身的技术优势——换句话说，也是妥协。

造成用户不得不“享受”这种现状的，早年罪魁祸首之一是 UNIX 系为首的业界通行设计，近年来则是 Web 。 Unicode 承前启后不假，但在“流行”充其量是拾人牙慧—— UTF-8 几乎全是借 Web 的东风而大势所趋，尽管技术上来讲对 Linux userland 来说它更能发挥作用。

这些规范在标准化的意义上来讲相当不得体。作为其中影响最巨大的几个余孽（和妥协失败品），基于 UNIX 早年对文本处理的过于简化的错误假设， ISO C 长期以来没能说清楚什么叫“字符”(character) ； ISO C++ 也有类似的问题。（并且相关术语定义的混乱和微妙的不一致仍然存在于正式(normative) 文本中。）

### 4

不透明“参数”是此文较有建设性的论点，可惜并没有给出实用的做法。因为语言设计中普遍缺乏约束，这基本只能靠用户（程序员）自觉，效果没法指望。更正确的姿势是，对“字符集”和编码方案透明，减少**字符集特性的冗余假设**。这样，简单起见只有 ASCII 自然扩展才是 UTF-8 的核心优势。“完整” Unicode 支持从头到尾都是没有说服力的。

### 5

之所以 Unicode 有必要单独抽出来评价，是因为在概念混淆和把妥协强行扶正为正式惯用技俩推广的不可替代的消极作用。

技术上 Unicode 有显著的问题。作为妥协的以“字符集”为中心特性之一的规范，一个明显的笑话是“字符集”无法精确定义——因为它根本就没有（也没法）规定，什么“感官”“字符”应该是字符集的收录范围。

因为不是随便谁都能往 Unicode 里添加公认接受的字符，这导致了 Unicode 扩展不同于寻常的字符集定义的流程，缺乏文化和厂商中立性，并且可能会对实现添加麻烦。

近期较为显著的例子是 emoji 。一个笑话：为什么 FreeType 突然需要依赖 libpng 才能看起来使 Unicode 渲染的支持“完整”一些呢？

### 6

把传输编码和文本表示编码比较是一种常识性错误。应该逆转过来，从原始需求出发：如果一开始就做正确的事——就让文本编码尽量只表示人能读写的真正意义上的纯文本，那么这样的比较还有何意义？

反过来，正是因为这种不切实际（需求）的浮夸比较，进一步助长了对不适合作为通信协议的“文本”这种形式的滥用，这是一种恶性循环。

至于只用于真正“文本”的情形，作为东亚语言母语用户，我很清楚**几乎从不需要英文字母以外的那些在 UTF-8 中占用两个八元组的西欧文字**，作者对此非常地想当然。另外，真要论东亚语言占用空间，而不担心随机定位性质的话， GB18030 跳出来就能把 UTF-8 打得满地找牙（嘛，本是同根生……）。

### 7

真正优雅的操作是把编解码和对码位的操作分离——虽然会让人怀疑为什么究竟还需要编码这回破事了。

也就是说，这方面 UTF-8 永远被 UCS-4 吊打。

### 8

这里没有否认对感官字符的计数不重要，却有意无意地忽略了一点，在计数能可靠到显得重要的时候，码位和感官字符是 1:1 的。主要显著例子是没有复杂拼接规则的字母文字（如英文）和东亚语言文字。

绝大多数人口——不管是不是 Unicode 的用户——只关心使用这类文字。

还有一个自以为是的错觉是了解 Unicode Normalization Forms 对所有用户都很重要。文本的（最终）用户通常不应该需要理会编码细节——这点跟“程序员不需要成为一个语言文字专家就能够正确地写一个文件复制程序了”对比，莫名喜感。

### 9

UTF-16 当然很烂。

> 性能几乎不是问题

偷换概念。空间占用难道不是性能问题？别忘了为什么 UTF-32 没有把 UTF-8 干趴。

> 很多人认为不应该用文本通信协议，但实际上通信协议几乎都用英语和 ASCII 字符组成，UTF-8 更有优势。对不同类的字符串用不统一的编码使复杂度大大上升，更容易引发问题。

定义不清的“统一”的编码只会掩盖问题。“几乎都用英语和 ASCII 字符组成”也仍然打不死 GB18030 ，只能说是历史包袱原因。

> 我们尤其认为给 C++ 标准增加 wchar_t 是一个错误，给 C++11 增加 Unicode 亦然。我们更想要一个能存储任意 Unicode 数据的基本执行字符集。

这是技术错误。让 ISO C++ 引用 ISO 10646 并不十分困难，但要求基本执行字符集的想法不切实际——即便排除 Unicode 可变版本兼容的困扰，要求运行时分辨被编码的字符需要额外的开销并不能被任意实现接受——特别是处理“文本”毫无用处的应用场景上。而只是要求容纳码位，并不需要更改执行字符集。

实际上 ISO C++ 连 ASCII 都仍然懒得完整要求。说到这里是不是又得想想 UTF-EBCDIC 哭晕在厕所呢？

> 标准库的 facet 有一堆设计缺陷。

依赖区域的特性本来就是一坨笑话。

### 10

仍然通篇逃避区分内部和外部编码这类关于选型的文本程序开发人员的基本义务，反而把问题搞得更复杂，同时强迫最终用户承担本可能不必要的额外开销。

### 11

> 但既然存在，那么说明文本不一定是给人类看的。

更不说明**应该**存在。

> 操作子字符串的方法会兴高采烈地把非 BMP 字符一刀两断，返回一个无效的字符串。

为什么判断“字符串”“有效”必须以 Unicode 为准则？

> 底层框架选错了内部字符串表示的方法，影响了 ASP.NET 这样的 Web 框架：网络应用几乎全要用 UTF-8 来输出（和输入）字符，在大流量的网络应用和服务中，字符串转换导致了显著开销。底层框架选错了内部字符串表示的方法，影响了 ASP.NET 这样的 Web 框架：网络应用几乎全要用 UTF-8 来输出（和输入）字符，在大流量的网络应用和服务中，字符串转换导致了显著开销。

偷换概念。某些特定的应用层不等于“网络”。

更应该说的是选错了服务的领域。要求统一编码，只剩“外部”的场景，就不应该做适配的无用功。所谓物以类聚和 garbage in garbage out 对这里的场景更合适。

> 不论原来 UTF-8 是否在创造时是作为一个兼容性措施，现在它比任何其它 Unicode 编码更好，也更流行。

“更好”再次被 GB18030 打脸。顺便，到现在 UTF-8 也不是任何一个官方使用东亚语言的政府强制要求支持的标准。

> 你不打算让你的软件设计完整支持 Unicode，是在开玩笑吗？

指望多数用户的大脑能“支持”非 BMP 字符才更加可笑——事实上是 BMP 字符也太多了。

另外同上，因为外延的不确定性， Unicode “字符集”论及“完整支持”，也是彻头彻尾的笑话。敢明确版本嘛？

> 那么，既然你打算支持，而非 BMP 字符罕见就不支持，除了给软件测试增加难度，实在没什么好处。

支持才是明显增加测试工作量。

> 然而，真正值得在意的是，真实的应用程序不怎么操作字符串——只是原封不动地传递字符串。这意味着“几乎定长”几乎没有性能优势（参见“性能”），让字符串短一点倒有可能挺重要。

莫名其妙的逻辑。码位计数可用且码位和感官字符是 1:1 时，分隔完整字符边界的操作是 `O(1)` 还是 `O(n)` 在长字符串上性能差异可能相当明显。当然这看来的确不很常见，但这恐怕也是 UTF-8 在编码东亚文字文本上唯一打得过 GB18030 的一个技术理由。

> 我们坚信，只要让软件 API 默认使用统一的主流编码，程序员不需要成为一个语言文字专家就能够正确地写一个文件复制程序了。

回顾上面的 normalization 打脸。

> 但是如果你将来打算加个配置文件或日志文件，请考虑把字符串都转换成窄字符串。这样可以免除后患。

这是常见的鸵鸟方法论。正确的做法是之前隐晦提到过的，区分“内部”和“外部”编码方案——如果不能自己发明设计，至少也需要了解应用。这才是处理文本和其它数据的程序员需要做且必须擅长做的基本功课。

> 我们无法接受，即使是在像文件连接这样简单的情形下，所有现有的代码都得注意到 BOM 的问题。我们无法接受，即使是在像文件连接这样简单的情形下，所有现有的代码都得注意到 BOM 的问题。

这则是彻头彻尾的**愚蠢**。

注意 UTF-8 的 T 的含义。所谓字节序标记本质上是传输时**必要**的元数据。只有附加假定以字节流传输，才可以省略字节序时指望正确的数据流——但是不一定包括边界。正确的传输总是需要这种附加的 cookie ，区别无非是哪一层做罢了。提供字节序是对边界的一种附加的（不完全）**强制验证**。在必须考虑边界时，丢掉附加验证并不会更“简单”。

另外，当处理“文件”这种作为外部传输的单独映像时，二进制文件的所谓文件头的元数据基本是不会被任意丢弃，处理“文本”附加额外的不一致的抽象，对“简单”而言，得不偿失。

只有假定没有边界（如 UNIX 的 `cat` 传输的数据）时才需要显式丢弃元数据的干扰。但这种缺乏类型标记的非结构化数据传输即便是对 shell 编程这种用途来讲，都是不怎么靠谱的，基本上只适合用完即弃。

另外， Unicode 标准实际上并没有显式鼓励传输时丢弃字节序的做法——在数据是**已知 UTF-8 有效载荷**的情况下，加入冗余 BOM 当然是添乱，所以应予避免。

> 永远使用 \n (0x0a) 作为行尾标记，即使是在 Windows 上。文件应以二进制模式读写，这保证了互操作性——一个程序在任何系统上都会给出相同结果。

依赖本不必要涉及的二进制表示通常是灾难的开始。

> 既然 C 和 C++ 标准采用了 \n 作为内部行尾表示，这就导致了所有文件会以 POSIX 惯例输出。

纯属谣言。

> 文件在 Windows 上用“记事本”打开可能会出问题；然而任何像样的文本编辑器都能理解这样的行尾。

而且，“实际”并不能保证行尾可信。记事本就是一个假定行尾二进制表示的反面例子。

至于为什么这么麻烦嘛……怪一开始的“妥协”好了。不过，考虑控制字符的设计， CR+LF 在含义上是比 LF 要合理的。

另外，实际上很多（应用层网络）协议就是规定用 CR+LF 而不是 LF ，为何在此反复？

> 我们也偏好 SI 单位、ISO-8601 日期格式，用句点而不是逗号作为小数点。

SI 最近正在改基准单位定义， ISO-8601 一坨几种来着……

> 一个更稠密的编码就更有利于性能。

对东亚文字一如既往被 GB18030 吊打。

> 没有纯文本这种概念。没理由在一个名叫“string”的类里存储仅 ASCII 或 ANSI 代码页编码的文本。

然而 `std::string` 其实彻头彻尾存的是二进制数据，还能在中间夹杂 `\0` ……

> 况且，MultiByteToWideChar 不是最快的 UTF-8↔UTF-16 转换函数。

作为撸过[目前为止还没有发现更快的 UTF-8 → UTF-16 解码实现](https://bitbucket.org/FrankHB/yslib/src/a202273d25704212e9a1c61faa2f714b8e7e2b25/YFramework/include/CHRLib/StaticMapping.hpp?at=master&fileviewer=file-view-default#StaticMapping.hpp-303)的，我可以负责任地讲，实际上相当慢……虽然通常并不介意这点差距。

> 将文件存为无 BOM的 UTF-8 可以解决。MSVC 会认为文件已经在正确的代码页，就不会碰你的字符串。

然后 MSVC 很高兴地会瞎猜编码了……嗯，如果不介意打开 IDE 就弹窗警告的话。

> 如果你打算设计一个接受字符串的程序库，只要用简单、标准且轻量的 `std::string` 就好。

“接受”和“储存”不是一个概念。

这种观念显然过时了，被 `string_view` 吊打。

> 如果你是 C 或 C++ 程序库作者，用 char* 和 std::string，默认 UTF-8 编码，并且拒绝支持 ANSI 代码页——因为这东西本来就不支持 Unicode。

负责的接口设计者应只在外部编码**约定使用**一致的普适编码，如 UTF-8 ，而**避免**作为实现细节的内部编码污染接口。至于 ANSI 代码页这玩意儿烂和 Unicode 无关。

### 12

> 这是我们基于经验和现实中程序员对于 Unicode 犯下真正的错误

然而对历史上导致这些设计的真正的错误却睁一只眼闭一只眼。

### 13

> 支持 Unicode 方可大幅提升用户体验。我们建议选择 UTF-8 而不是 UTF-16、GBK 或 GB18030 作为应用程序的默认编码。

这两句话连在一起，说得 GB18030 不是[基于 Unicode](https://en.wikipedia.org/wiki/GB_18030#cite_ref-2) 一样……（虽然 UTF-16 的部分应该不会被理解错。）原文并没有讲清的东西一笔带过，含义有些不清楚。




