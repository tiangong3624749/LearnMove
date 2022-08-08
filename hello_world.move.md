## Move真的时薪$1200吗？

最近一则Move时薪的新闻刷爆了朋友圈。对比往日的门可罗雀，很多朋友私下开始咨询我有关Move的事情。这种突入其来的转变，作为Move的先行者(布道师)，深受震撼。了解我的人都知道，因为各种机缘，我从Move白皮书发布就一直致力于Move的研究，从纯技术学习的角度，布道Move也已经有一年多了。很坦率的说，我对Move是由衷的热爱（这里需要说明的是，Web3与区块链并不是必然的联系，但是Web3离开不了安全。法律是最底层的安全，所有的研究都基于法律允许的框架内）。如果你跟我一样，那么欢迎加入Move的大家庭。我们相互学习，一起期待有一天Move强大起来，成为Web3的重要技术。

也有很多人对Move充满了疑惑。

1）Move要挑战Ethereum？

2）除了安全，Move看上去没啥嘛。

3）Over engineering（过度设计）

4）Move没有生态，起不来

5）Move真的有那么高的时薪吗？

抱有各种各样的质疑，这些我也解答不了，交给时间去验证吧（没关系，这都是我非常理解的，毕竟Move穷志短。相信我，我在布道Move的一年多时间里，听过更难听的评论，但是并不耽误我对Move的热情）。我相信一点，技多不压身，与其漫无目的的浪费精力，不如静下心学习一下Move，万一有用呢？用李笑来的话说，你并不孤独。只是学习Move不要功利性太强，就当玩游戏吧。

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h4y96ij1suj20sm0buaaz.jpg" alt="move时薪" style="zoom:50%;" />



## Move vs Solidity

Move语言说难也难，说简单特别的简单。受限于智能合约的场景，Move不可能复杂。比起高级语言，例如并发编程、网络调用等等，智能合约语言是一个完全隔离的环境，完全串型执行，使用特别的简单（很期待哪天能设计出一种并发执行智能合约的模型出来）。所以，如果你会任何一种高级语言，完全不用担心学不会Move，实在是太简单了。

跟Soidity对比，最核心的区别，Solidity是动态语言，Move纯静态语言。在扩展性方面，Solidity依靠的是动态调用，而Move因为是静态语言，所以为了保证扩展性，Move选择了泛型编程的范式。但是正是这种底层大方向的不同，导致了安全性的巨大差异（需要说明的是，Move是从Solidity的真实安全漏洞中吸取了经验教训，才选择一种截然不同的路线）。

（从学习的角度来说，我并没有贬低Solidity的意思，所以不要以为我在搞语言之争，放平心态。）

Move最核心的两个特性是：

* 面向资源编程
* 面向泛型编程

但是上面的两个核心特性，远不足以概况Move的优点。Move有很多的安全特性，很多都对开发者完全透明，例如算数溢出、默认可见性导致的权限泄露等等（合约开发者不需要花太多的心思在安全上，Move语言已经帮你做了很多的安全处理，尤其是很多低级的安全隐患）。可以说，Move从语言层面，大幅降低了开发者的安全门槛，开发者只需要专注业务模型上的设计与实现。除此之外，Move在可扩展性的基础上，工程上也有很强的实力，能够非常轻松地设计出复杂的工程，我认为这一点也是不容忽视的。

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h4yal7idgej21hi0ga431.jpg" alt="info&resouurce" style="zoom:25%;" />



## Move & Aptos

不管出于什么目的，我们已经开始行动，学习Move了，希望对你来说这是一个愉快的过程。让我们开始吧。

对于很多刚入门的朋友来说，经常搞不清楚Move和Aptos、Sui、Starcoin等公链的关系，尤其是这些公链都有各自的IDE和Framework，有点蒙圈。对于初学者来说，到底应该怎么入手呢？

Move是一门编程语言，本身是中立的，这里是Move官方的[Github仓库]()。那为什么还会有各种IDE和Framework呢？

各个公链在发展的过程中，可能会有一些特殊的需求，需要用到Move暂时没有的一些功能，所以通常会从Move的Github中fork一份代码，自己稍加修改（别担心，通常不会是大的修改，或者以后会有兼容方案），以满足自身的发展。在自有版本的Move之上，因为需要对自己的链结合，例如Account模型上可能一些差别，或者方便开发者基于自己的链快速开发Dapp，通常会封装一层Framework。

对于我们在学习Move语言基础的时候，建议使用Move官方的版本（以后基于某个公链实战，再考虑选择Framework的问题）。

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h4zrvefcehj20xa0hm0th.jpg" alt="Move_Framework" style="zoom:40%;" />



## 搭建Move环境

们这里以Move官方标准库为基础学习Move，先搭建Move的开发环境。

截止目前，Move还没有一个很好的开发环境。好消息是，比起我学Move的时候，已经非常的方便了（2019年的时候，Move本身还不稳定，完全没有可用的IDE，靠纯文本编辑器写代码，而且错误提示经常不对）。由于Move官方没有可以直接下载的二进制Release版本，我们需要自己手动编译，需要先安装一些依赖。



## ① 安装和学习Git

这里不介绍了，开源系统，基本都通过Git协作，不会的请先查看Git教程（当然也可以绕过Git，直接下载源码到本地）。



### ② 安装Rust环境

Move官方的库是用Rust实现的，并且没有可直接下载的二进制包，所以我们先搭建Rust的编译环境（这块教程很多，自己去网上找一下，这里不详细说）。这里需要注意的是，并不要求对Rust了解，只需要借助Rust完成Move环境的搭建。

通过下面命令确定Rust环境是否安装成功：

~~~
$ rustc -V
~~~



### ③ 编译安装Move环境

先通过Git下载[Move](https://github.com/move-language/move)代码（假设Git已经安装好）：

~~~
$ git clone https://github.com/move-language/move
~~~

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h4yc101a40j20uw06ot9n.jpg" alt="move1" style="zoom:50%;" />

下载完成之后，编译和安装Move-cli：

```
$ cargo install --path move/language/tools/move-cli
```

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h4ycz6s0urj20xe07wjsk.jpg" alt="move3" style="zoom:50%;" />

这种方式安装的Move会在 cargo home里，例如~/.cargo/bin。查看安装结果：

~~~
$ move -h
~~~

如果安装正确，会显示move的帮助信息。

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h4yd0qmnxxj20t80aygmd.jpg" alt="move2" style="zoom:50%;" />

这时候，Move环境已经搭建好了。



## Move插件和IDE

前面搭建了Move的编译环境，如果想要快速开发Move程序，还需要选择合适的IDE（主要是高亮和自动提示等，帮助开发）。我习惯使用Idea开发，大家也可以选择VS Code等IDE。

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h4ydkiei4xj217u0n8whl.jpg" alt="move4" style="zoom:33%;" />

怎么理解Move环境和Move插件(IDE)呢？

Move的IDE会做一些语法提示、检查以及高亮显示，帮助我们快速开发Move。但是代码能否运行，需要依赖Move环境，编译不出错，则说明代码没有语法上面的问题。所以，Move环境和Move的IDE对我们来说，都很重要。



## 第一个Move程序：HelloWord

前期的准备完成，我们可以愉快的学习Move了。按照行业惯例，我们写第一个HelloWorld例子。

### ① Move命令使用

首先，通过前面安装的Move命令，生成一个hello_word的项目：

~~~
$ move new hello_world
~~~

结果如图所示：

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h4ydw1md3aj20jk04u3yq.jpg" alt="move5" style="zoom:80%;" />

生成了Move.toml文件和source目录，这里先不介绍各自的作用。



### ② 编写HelloWord程序

这里可以使用IDE引入刚创建的hello_world项目（以Idea为例）。在source文件夹上点击右键，选择`Move File`。生成一个叫hello_world的Script（以后会详细介绍Script的作用）：

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h4ynona6tsj21f20taq6d.jpg" alt="move6" style="zoom:33%;" />

生成一个hello_world.move的文件，如下图所示：

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h4zs8udiorj20hk0cqjs4.jpg" alt="move8" style="zoom:70%;" />

在编写代码之前，我们需要打开Move.toml文件设置依赖（整个风格跟Rust有点像）：

~~~
[addresses]
std = "0x1"

[dependencies]
MoveNursery = { git = "https://github.com/move-language/move.git", subdir = "language/move-stdlib/nursery", rev = "main" }
~~~

其中dependencies配置依赖的代码，这里指定到Move官方的Github仓库。addresses需要格外注意，要跟Move官方的stdlib中的地址变量保持一致。由于Move代码里面是std，所以我们这里也用std（大小写也要一致）。

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h4zsgc0pioj20t00l8wh3.jpg" alt="move10" style="zoom:33%;" />

终于到编写Move代码这一步了。打开hello_world.move文件，输入代码：

~~~
script {
    use std::debug;
    fun main() {
        debug::print(&b"hello world");
    }
}
~~~

HelloWorld的代码就完成了。也是最关键的一步，编译代码：

~~~
move build
~~~

编译成功，说明代码没有问题。

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h4ynp0na0ij21a80oaju9.jpg" alt="move7" style="zoom:33%;" />

最后一步，验证结果：

~~~
move sandbox run sources/hello_world.move
~~~

打印结果出来了。

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h4zsysf7m7j210e0483yy.jpg" alt="move11" style="zoom:33%;" />



## 跟我学Move

今天主要是教大家如何搭建Move的开发环境，并写了第一个Move程序，顺便解答了一些有关Move的疑惑。

我是Move小王子(微博和知乎ID)，欢迎大家关注我并定义“手把手写Move智能合约”专栏，跟我一起学习Move。下期我们将会介绍Move的基础语法。

