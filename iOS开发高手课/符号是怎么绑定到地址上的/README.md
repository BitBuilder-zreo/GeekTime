## 扩展阅读

[iOS编译过程](https://github.com/rogertan30/GeekTime/blob/master/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE/%E7%AC%A6%E5%8F%B7%E6%98%AF%E6%80%8E%E4%B9%88%E7%BB%91%E5%AE%9A%E5%88%B0%E5%9C%B0%E5%9D%80%E4%B8%8A%E7%9A%84/iOS%E7%BC%96%E8%AF%91%E8%BF%87%E7%A8%8B.pdf)

[iOS启动过程](https://github.com/rogertan30/GeekTime/blob/master/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE/%E7%AC%A6%E5%8F%B7%E6%98%AF%E6%80%8E%E4%B9%88%E7%BB%91%E5%AE%9A%E5%88%B0%E5%9C%B0%E5%9D%80%E4%B8%8A%E7%9A%84/iOS%E5%90%AF%E5%8A%A8%E8%BF%87%E7%A8%8B.pdf)

[LLVM介绍](https://github.com/rogertan30/GeekTime/blob/master/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE/%E7%AC%A6%E5%8F%B7%E6%98%AF%E6%80%8E%E4%B9%88%E7%BB%91%E5%AE%9A%E5%88%B0%E5%9C%B0%E5%9D%80%E4%B8%8A%E7%9A%84/LLVM.pdf)

[Mach-O介绍](https://github.com/rogertan30/GeekTime/blob/master/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE/%E7%AC%A6%E5%8F%B7%E6%98%AF%E6%80%8E%E4%B9%88%E7%BB%91%E5%AE%9A%E5%88%B0%E5%9C%B0%E5%9D%80%E4%B8%8A%E7%9A%84/Mach-O.pdf)

[深入剖析iOS编译Clang/LLVM](https://ming1016.github.io/2017/03/01/deeply-analyse-llvm/)

[浅谈iOS编译过程](http://www.cocoachina.com/ios/20181205/25711.html)

## 课程笔记

[思维导图笔记](https://github.com/rogertan30/GeekTime/blob/master/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE/%E7%AC%A6%E5%8F%B7%E6%98%AF%E6%80%8E%E4%B9%88%E7%BB%91%E5%AE%9A%E5%88%B0%E5%9C%B0%E5%9D%80%E4%B8%8A%E7%9A%84/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE_withMarginNotes.pdf)

[课程原文件](https://github.com/rogertan30/GeekTime/blob/master/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE/%E7%AC%A6%E5%8F%B7%E6%98%AF%E6%80%8E%E4%B9%88%E7%BB%91%E5%AE%9A%E5%88%B0%E5%9C%B0%E5%9D%80%E4%B8%8A%E7%9A%84/05%E4%B8%A8%E9%93%BE%E6%8E%A5%E5%99%A8%EF%BC%9A%E7%AC%A6%E5%8F%B7%E6%98%AF%E6%80%8E%E4%B9%88%E7%BB%91%E5%AE%9A%E5%88%B0%E5%9C%B0%E5%9D%80%E4%B8%8A%E7%9A%84%EF%BC%9F.html)

## 摘要

>为什么有的编译起来很快，有的却很慢；编译完成后，有的启动得很快，有的却很慢。

#### iOS 开发为什么使用的是编译器？

**编译器**：把代码编译成机器码，然后直接在 CPU 上执行机器码

**解释器**：在运行时解释执行代码，获取一段代码后就会将其翻译成目标代码（就是字节码（Bytecode）），然后一句一句地执行目标代码。

#### 既然编译器效率这么高，那为什么还有人用解释器呢？

你写的程序跑起来后不用重新启动，就可以看到代码修改后的效果，这样就缩短了调试周期。程序发布后，你还可以随时修复问题或者增加新功能，用户也不用一定要等到发布新版本后才可以升级使用。所以说，使用解释器可以帮我们缩短整个程序的开发周期和功能更新周期。

* 采用编译器生成机器码执行的好处是效率高，缺点是调试周期长。

* 解释器执行的好处是编写调试方便，缺点是执行效率低。

#### iOS 开发使用的到底是什么编译器

现在苹果公司使用的编译器是 LLVM，相比于 Xcode 5 版本前使用的 GCC，编译速度提高了 3 倍。

LLVM 是编译器工具链技术的一个集合。而其中的 lld 项目，就是内置链接器。编译器会对每个文件进行编译，生成 Mach-O（可执行文件）；链接器会将项目中的多个 Mach-O 文件合并成一个。

#### iOS 编译的几个主要过程

* 首先，你写好代码后，LLVM 会预处理你的代码，比如把宏嵌入到对应的位置。

* 预处理完后，LLVM 会对代码进行词法分析和语法分析，生成 AST 。AST 是抽象语法树，结构上比代码更精简，遍历起来更快，所以使用 AST 能够更快速地进行静态检查，同时还能更快地生成 IR（中间表示）。

* 最后 AST 会生成 IR，IR 是一种更接近机器码的语言，区别在于和平台无关，通过 IR 可以生成多份适合不同平台的机器码。对于 iOS 系统，IR 生成的可执行文件就是 Mach-O。

#### 编译时链接器做了什么？

链接器的作用，就是完成变量、函数符号和其地址绑定这样的任务。而这里我们所说的符号，就可以理解为变量名和函数名。

#### 为什么要让链接器做符号和地址绑定这样一件事儿呢？不绑定的话，又会有什么问题？

如果地址和符号不做绑定的话，要让机器知道你在操作什么内存地址，你就需要在写代码时给每个指令设好内存地址。写这样的代码的过程，就像你直接在和不同平台的机器沟通，连编译生成 AST 和 IR 的步骤都省掉了，甚至优化平台相关的代码都需要你自己编写。

#### 链接器为什么还要把项目中的多个 Mach-O 文件合并成一个

你肯定不希望一个项目是在一个文件里从头写到尾的吧。项目中文件之间的变量和接口函数都是相互依赖的，所以这时我们就需要通过链接器将项目中生成的多个 Mach-O 文件的符号和地址绑定起来。

没有这个绑定过程的话，单个文件生成的 Mach-O 文件是无法正常运行起来的。因为，如果运行时碰到调用在其他文件中实现的函数的情况时，就会找不到这个调用函数的地址，从而无法继续执行。

#### 链接器对代码主要做了哪几件事儿

* 去项目文件里查找目标代码文件里没有定义的变量。

* 扫描项目中的不同文件，将所有符号定义和引用地址收集起来，并放到全局符号表中。

* 计算合并后长度及位置，生成同类型的段进行合并，建立绑定。

* 对项目中不同文件里的变量进行地址重定位。

你在项目里为某项需求写了一些功能函数，但随着业务的发展，一些功能被下掉了或者被其他负责的同事在另一个文件里用其他函数更新了功能。那么这时，你以前写的那些函数就没有用武之地了。日长月久，无用的函数越来越多，生成的 Mach-O 文件也就越来越大。

这时，链接器在整理函数的符号调用关系时，就可以帮你理清有哪些函数是没被调用的，并自动去除掉。那这是怎么实现的呢？

链接器在整理函数的调用关系时，会以 main 函数为源头，跟随每个引用，并将其标记为 live。跟随完成后，那些未被标记 live 的函数，就是无用函数。然后，链接器可以通过打开 Dead code stripping 开关，来开启自动去除无用代码的功能。并且，这个开关是默认开启的。

#### 动态库链接

**静态库**：是编译时链接的库，需要链接进你的 Mach-O 文件里，如果需要更新就要重新编译一次，无法动态加载和更新

**动态库**：是运行时链接的库，使用 dyld 就可以实现动态加载。

Mach-O 文件是编译后的产物，而动态库在运行时才会被链接，并没参与 Mach-O 文件的编译和链接，所以 Mach-O 文件中并没有包含动态库里的符号定义。也就是说，这些符号会显示为“未定义”，但它们的名字和对应的库的路径会被记录下来。运行时通过 dlopen 和 dlsym 导入动态库时，先根据记录的库路径找到对应的库，再通过记录的名字符号找到绑定的地址。

dlopen 会把共享库载入运行进程的地址空间，载入的共享库也会有未定义的符号，这样会触发更多的共享库被载入。dlopen 也可以选择是立刻解析所有引用还是滞后去做。dlopen 打开动态库后返回的是引用的指针，dlsym 的作用就是通过 dlopen 返回的动态库指针和函数符号，得到函数的地址然后使用。

**使用 dyld 加载动态库，有两种方式**：有程序启动加载时绑定和符号第一次被用到时绑定。为了减少启动时间，大部分动态库使用的都是符号第一次被用到时再绑定的方式。

#### dyld 做了什么

* 先执行 Mach-O 文件，根据 Mach-O 文件里 undefined 的符号加载对应的动态库，系统会设置一个共享缓存来解决加载的递归依赖问题；

* 加载后，将 undefined 的符号绑定到动态库里对应的地址上；

* 最后再处理 +load 方法，main 函数返回后运行 static terminator。

#### 小结

编译阶段由于有了链接器，你的代码可以写在不同的文件里，每个文件都能够独立编成 Mach-O 文件进行标记。编译器可以根据你修改的文件范围来减少编译，通过这种方式提高每次编译的速度。

了解了这种链接机制，你也能够明白，文件越多，链接器链接 Mach-O 文件所需绑定的遍历操作就会越多，编译速度也会越慢。

了解程序运行阶段的动态库链接原理，会让你更多地了解程序在启动时做的事情，同时还能够对你有一些启发。

在开发调试阶段，是不是代码改完以后可以先不去链接项目里的所有文件，只编译当前修改的文件动态库，通过运行时加载动态库及时更新，看到修改的结果。这样调试的速度，不就能够得到质的提升了么。



