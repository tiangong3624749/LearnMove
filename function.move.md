最近比较忙，一直没时间更新专栏，抓住中秋假期的尾巴，写篇Move教程。

在我们的Move教程基础篇中，前面讲了如何搭建Move环境、Move的基本数据类型，今天我们要讲一讲Move的函数定义以及函数调用。对于所有的高级语言，函数定义其实都差不多，无非是一些细节上的不同。因为Move的智能合约语言，所以对比Solidity来讲（就事论事，没有喷Solidity的意思）。



## Move与Solidity的函数对比

首先需要了解一点，Move的代码跟状态是分离的。对比Solidity，Move的函数显得非常的干净：

* 没有Library的概念
* 没有继承的概念
* 没有特殊函数（Solidity有fallback、receive等函数，都是为了处理特殊的状态）
* 没有Solidity的calldata、memory、storage等变量（但是有mutable和immutable的引用变量）
* 没有Modifier
* 没有payable

在函数定义方面，对比Solidity，除了语法上的区别，还有几个重要的不同：

* 函数可见性（Move有更丰富的函数可见性）
* Move可以传值，也可以传引用（其他高级语言也是这样）
* 返回结构体struct（这个在Move进阶课程中再介绍）和tuple元组（也就是多个值），还可以返回引用

在函数调用方面，Move和Solidity也有很大的不同：

* Move没有Solidity的msg上下文了（比较难理解的特性，容易出现安全问题）
* Move是静态调用（没法构造递归调用，也没有Solidity合约DelegateCall这些容易带来安全隐患的东西）
* Move只能修改当前合约的数据，Solidity被调用合约可以修改调用合约的数据（反常识）

Move是一门更“正常”的编程语言，除了acquires关键字之外，其他的都跟我们对高级语言的理解是一样的。Solidity因为代码和状态没有完全分离，所以添加了很多“为了解决某个状态问题而专门设计的特性”，例如payable和fallback函数等等，还有容易让人混淆的msg上下文等概念，有点缝缝补补的感觉。（这里暂时不涉及泛型对函数定义和调用的影响，在后面讲泛型的时候，专门介绍）。



## Move的函数签名

前面对比了Move跟Solidity的不同，接下来我们介绍一下Move的函数签名。

高级语言的函数签名都大同小异，一般包含以下部分：

* 函数可见性
* 函数名
* 函数参数(包括传值和传引用)
* 函数返回值
* acquires（这是智能合约特有的关键字）

Move的编程风格有点类似于Rust，以下是函数的定义：

~~~
函数可见性 fun 函数名(参数列表): 返回值列表 acquires 结构体列表 {
		函数体
		return 结果
}
~~~

这里有几个地方简单说明一下：

* `fun`是函数定义的关键字（Rust是function，为了区分Move跟Rust，Move团队特意引入了一些小的区别）
* Move 函数使用snake_case命名规则，小写字母以及下划线作为单词分隔符
* `:`指定返回值，可以是列表，列表用`()`括起来，也支持struct类型的返回值（跟Solidity有区别），不指定表示没有返回值
* `acquires`指定函数引用到的struct（这块在后面的进阶教程中，介绍Struct的时候再讲），不指定表示没有引用
* `return` 在函数体的最后可以省略，同时，不需要`;`，否则编译会抛错



## 函数可见性

前面在讲函数签名的时候没有讲Move函数的可见性。Move的可见性非常有意思，值得单独拿一个小节来讲。

了解Solidity的人都知道，Solidity只有`public`和`private`两种可见性。对于开源的DeFi系统来说，函数要么是所有人可见，要么说有人都不可见，有点激进。不知道大家用过Java没有？Java有4种级别的函数可见性：`public`、`friendly(无关字)`、`protected`、`private`，其中，`protected`表示继承关系的可见性，`friendly`指定package级别的可见性。Move也设计了4种函数的可见性，分别是（关于什么是模块Module，暂时可以类比Solidity的Contract，在进阶课程会专门介绍）：

* private：当前Module可见（Move函数的默认可见性）
* friend：当前address下被授权的Module可见（可以理解为address级别的可见性，类比Java的package级别的可行性。Solidity通常使用Modifier来控制调用权限，这依赖开发者的个人能力，容易引发安全隐患，被黑客绕过）
* public：所有Module可见，但是用户不能直接作为交易使用（跟Solidity不一样，Solidity是调用public的合约函数发起链上交易）
* entry或者script：交易的入口（可以理解为可编程的交易，在讲Module的时候再详细介绍）

以上4种可见性，可见范围逐渐放大。

### 1. public & private

我们先看看`public`和`private`的使用。`public`和`private`跟Solidity大同小异，没什么特别的地方。下面是在public函数中调用了一个private函数：

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h63y0swmp5j20jo0b6q3c.jpg" alt="move_fun_1" style="zoom:50%;" />

这里简单说明一下：

* ① 是一个private的函数，③ 是一个public的函数，尤其要注意的是，Move默认是函数可见性是private的（这点跟Solidity不一样，Solidity默认是public的），能很好的避免函数的泄露
* ② 是返回值，需要注意，这里不能有`;`，否则编译不通过
* ④ 和 ⑤ 都是对当前合约的`test_private`函数的调用，Self表示当前Module

### 2. friend

接下来，我们再介绍一下`friend`可见性的使用。

可见性的本质是控制函数对外的访问权限。`friend`的作用是控制「相同address下，跨Module的函数调用」，这样精细化的访问控制，能更好的避免函数泄露的问题。下来是`friend`使用的例子。

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h67dkro28sj218y0igmy3.jpg" alt="move_friend_1" style="zoom:33%;" />

我们定义了一个function1的模块，简单说明一下：

* ② 通过`public(friend)`关键字定义了一个`friend`可见性的test_friend1的函数
* ① 通过`friend`授权让std地址（本实例是0x2地址）的function2的模块可以调用

我们再定义一个function2的模块，对test_friend1进行调用：

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h67dpjdfidj21600i8wfe.jpg" alt="move_friend_2" style="zoom:33%;" />

简单说明一下（这块跟调用public函数一样，没有区别，注意一下跨Module调用以及use关键字）：

* ③ 是引入function1的模块
* ④ 调用function1的test_friend1函数

以上是`friend`可见性函数的调用例，都是在std的address下（本例是0x2）。我们来看一下，`friend`的函数能不能授权给其他的address调用呢？

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h67dvqcvaij20y40u0ta7.jpg" alt="move_friend_3" style="zoom:33%;" />

上图是假设授权给0x3这个地址的function3的模块使用，可以看到编译错误，所以`friend`可见性的函数是不可以跨地址使用的。

## 3. entry & script

entry或者script可见性，也是Move很有意思的一个函数可见性，是用户跟链上交互的入口。

在EVM合约开发中，Solidity代码编写完之后，要将所有的代码部署到链上，然后，用户通过调用Contract的public方法发起交易。这么处理当然没问题，但是灵活性不足，因为Contract是部署在链上，并且部署之后不能修改（或者说修改很麻烦），而用户的需求往往会随着事情的发展而改变的。

Move在合约设计上，将链上和链下区分开。也就是说，Move的交易是链下的，是可以编程的，拥有极好的灵活性，并不像Solidity那样通过函数名来发起交易。而entry或者script可见性正是用来代表交易的函数。

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h69frgrh8dj20fk084jrn.jpg" alt="image-20220917112202477" style="zoom:80%;" />

Move最早叫script可见性，后面修改成entry可见性了，更直接，更好理解，也能跟模块化的script进行区分（讲模块化的时候会讲到）。不管叫什么，也不管关键字是什么，理解了它的应用场景最重要，使用起来很简单。下面是例子：

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h69nswy9ujj21gu0s0djj.jpg" alt="move_entry" style="zoom:33%;" />

简单说明一下：

* ① 是使用`public(script)`定义的函数
* ② 使用`entry`关键字定义函数
* ③ 编译的时候提示了`public(script)`将会弃用



# 传值 & 传引用

前面对Move的4种函数可见性做了详细介绍。跟Solidity对比，除了上面这些区别，还有一个很大的区别，Move的参数既可以传值，也可以传引用（常见的高级语言也是这样，例如Java），并且区分`mutable ref`「可变引用」和`immutable ref`「不可变引用」，这是Rust的风格。

我们来理解一下`immutable ref`和`mutable ref`：

* `immutable ref`可以类比只读锁，`mutable ref`可以类比独占的写锁。
* 同一个结构体的实例，同时可以有多个`rimmutable ef`
* 同一个结构体的实例，同时只能有一个`mutable ref`
* 同一个结构体的实例，`mutable ref`和`immutable ref`不能同时存在，但是`mutable ref`可以当`immutable ref`使用

所以我们在定义函数的时候一定要注意，引用参数应该是「可变」还是「不可变」。「可变」意味着函数逻辑可以修改当前结构体instance的数据，而「不可变」意味着只读。下面是例子：

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h69oy4un9ej21620p4q5w.jpg" alt="move_mut_ref" style="zoom:33%;" />

简单说明一下：

* ① 定义了一个strut（这个在进阶课程会详细介绍）
* 对immutable ref的定义是`&`,而mutable ref的定义是`&mut`
* ② 定义了一个immutable ref参数的函数，只读
* ③ 定义了一个mutable ref参数的函数，可以修改



## 返回Struct & 引用 & 元组

对比Solidity，Move函数的返回值也很有特点。

Solidity的函数最早只能返回基本数据类型，后面增加了返回结构体的支持。但是，Move函数的返回值更加自由，能返回任何形式的数据，例如`immutable ref`、`mutable ref`、tuple元组。

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h69pgpxyjkj20u00x0q56.jpg" alt="move_return" style="zoom:50%;" />

上面是Move函数返回各种类型的数据的例子，简单说明一下：

* ① 定义了一个strut
* ② 函数返回了一个struct
* ③ 函数返回了一个immutable的引用，以为着数据只读
* ④ 函数返回了一个immutable的引用，意味着数据可以被修改
* ⑤ 函数返回了一个tuple类型，调用了 ② 的函数



## 跟我学Move & 完整代码

当前教程详细介绍了Move函数如何定义和调用，重点对比Solidity、介绍了Move函数的可见性、如何传引用、如何返回数据类型。完整的代码如下（Module和Struct、acquires以及函数的泛型在后面的进阶课程再介绍）：

~~~
module std::function1 {

    fun test_private() :bool {
        true
    }

    public fun test_public() : bool {
        Self::test_private();
        test_private();
        false
    }

    friend std::function2;
//    friend 0x3::function3;

    public(friend) fun test_friend1() : u64 {
        1
    }

    public(script) fun test_script() {}

    public entry fun test_entry() {}

    struct Test{
        flag:bool
    }

    fun test_immutable(test: &Test):bool {
        return test.flag
    }

    fun test_mutable(test:&mut Test) {
        test.flag = true;
    }

    fun test_struct_return() : Test {
        Test{
            flag: true
        }
    }

    fun test_im_return(test: &Test) : &Test {
        test
    }

    fun test_mut_return(test: &mut Test) : &mut Test {
        test
    }

    fun test_tuple_return() : (Test, bool) {
        (test_struct_return(),false)
    }
}
~~~

对比Solidity的函数，Move的函数确实显得更简洁、丰富、自由。

我是@MoveContract（twittter），也可以关注@Move小王子 的知乎专栏“手把手写Move智能合约”，跟我一起学习Move。下期是Move基础课程的最后一讲，主要讲Move经常用的到一些类库，例如vector、table等等。
