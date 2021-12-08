Xcode工程中 Targets讲解是本文要介绍的内容，相信很多人都注意到XCode中, 有个Target的概念. 这在很多地方都有所体现, 比如打开一个工程后, 左侧的列表中有Targets一项, 而在工程界面的顶部菜单中, project里面也有多个涉及到Target的项目, 那么这个Target到底是什么呢?

Apple的人是这样说的:“ Targets that define the products to build. A target organizes the files and instructions needed to build a product into a sequence of build actions that can be taken.”

简单的理解的话, 可以认为一个target对应一个新的product(基于同一份代码的情况下). 但都一份代码了, 弄个新product做啥呢? 折腾这个有意思么?

其实这不是单纯的瞎折腾, 虽然代码是同一份, 但编译设置(比如编译条件), 以及包含的资源文件却可以有很大的差别. 于是即使同一份代码, 产出的product也可能大不相同.

我们来举几个典型的应用多Targets的情况吧, 比如完整版和lite版; 比如同一个游戏的20关, 30关, 50关版; 再或者比如同一个游戏换些资源和名字就当新游戏卖的(喂喂, 你在教些什么...)

Targets之间, 什么相同, 什么不同

既然是利用同一份代码产出不同的product, 那么到底不同Target之间存在着什么样的差异呢?

要解释这个问题, 我们就要来看看一个Target指定了哪些内容.

从Xcode左侧的列表中, 我们可以看到一个Target包含了Copy Bundle Resources, Compile Sources, Link Binary With Libraries. 其中
> Copy Bundle Resources: 是指生成的product的.app内将包含哪些资源文件

> Compile Sources: 是指将有哪些源代码被编译

> Link Binary With Libraries: 是指编译过程中会引用哪些库文件


通过Copy Bundle Resources中内容的不同设置, 我们可以让不同的product包含不同的资源, 包括程序的主图标等, 而不是把XCode的工程中列出的资源一股脑的包含进去.

而这还不是一个target所指定的全部内容. 每个target可以使用一个独立, 不同的Info.plist文件.

我们都知道, 这个Info.plist文件内定义了一个iPhone项目的很多关键性内容, 比如程序名称, 最终生成product的全局唯一id等等.

而且不同的target还可以定义完整的差异化的编译设置, 从简单的调整优化选项, 到增加条件编译所使用的编译条件, 以至于所使用的base SDK都可以差异化指定.
