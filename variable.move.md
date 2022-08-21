今天听到一个很好的观点，对于普通人来说，对世界最大的贡献是让自己快乐。学习使我快乐。作为Move布道师，帮助别人学习也很快乐。

希望大家不要被面向资源编程这个概念给吓跑了（只是一只纸老虎），坚持学习。当然也不要太急功近利，技多不压身，学的技能多了，就会由量变产生质变。当学不动的时候，想想$1200美刀的时薪还没到手，有什么理由不学呢（开个玩笑）。

在前期的基础课程中，我将继续为大家一些Move的基础知识（后面会有进阶课程，但愿我能坚持下来，我也需要大家的鼓励，才会分泌更多的多巴胺）。前面我给大家介绍了如何安装Move环境，并且用一种简单的方式实现了一个最简版的HelloWorld，你入门了吗？本期课程主要介绍Move的基本数据类型以及如何定义Move变量。



## Move基本数据类型和数组

语言嘛，其实都差不多。如果有其他高级语言的编程经验，比如Python、Java、Golang、Rust等等，那么你可以跨过本期内容了。真的没啥可讲的。

Move主要有3类基本数据类型：

* 整型，其中整型包括 (`u8`, `u64`, `u128`)
* 布尔型 `boolean` 
* 地址 `address`

跟其他语言对比，Move的基本数据类型缺少两种类型：

* Move没有字符串类型（用二进制来表达嘛）
* Move没有浮点型（Solidity也没有浮点型，这个其实简单，用整数来代替就可以了），所以在各大Move公链的Framework或者Stdlib中都有一个FixedPoint32.move的模块

另外，Move的基本数据类型还有3个点需要注意的地方：

* 没有负数（这个其实也简单，用一个`bool`型表示符号就可以了）
* 没有`u256`类型（这个在DeFi里面很重要，记得当时Starcoin跟Libra的Move社区讨论过很久，一直没有通过，最后选择在`u128`上面进行封装）
* address虽然是专门的地址类型，但是本质上是整型，所以address能跟整型相互转换（Solidity也是这样的）

这是Move的基本数据类型跟其他高级语言不同的地方。

除了上面提到的基本数据类型，数组也是最常用的数据类型，所以这里把数组也一起讲了。Move的数组类型是vector。关于vector类型有3个需要说明的地方：

* vector是泛型类型（以后再讲什么是泛型），例如`vector<u8>`或者`vector<address>`等等
* vector是类型，而std::vector是一个Move模块（不是Move语言本身的），std::vector是一些用来操作vector类型数据的函数。所以一定要注意，vector和std::vector虽然有关联，但不是一个东西
* 在Move官方版本中，操作vector类型的是std::vector，而在一些公链的Stdlib中则是std::Vector，这个也需要格外注意一下



## 定义Move变量和常量

我们这里以Move官方版本为准，使用上面介绍的这些基本数据类型和数组定义Move变量。

Move是mini版的Rust（开玩笑），Move借鉴了很多Rust的思想。Move在定义变量的时候，也跟Rust一样，使用了let关键字。

1. 整型变量

~~~
//变量定义
let _a:u8 = 1;
let _b: u8;
_b = 10;
let c = 2u64;

// 类型强转
let _d = c as u128;
let _e = c as u8;
~~~

上面既介绍了整型变量的定义方式，也介绍了如何类型强转。

2. 布尔型变量

~~~
let _flag = true;
~~~

3. 地址变量

~~~
 let _addr1: address;
 _addr1 = @test_addr;

let _addr2 = 0x1;// 确定的地址
~~~

这里的地址类型需要注意一下：test_address是在Move.toml文件里面定义好的地址，通过`@`引用。

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h5e5rmg8u8j20b004c3yh.jpg" alt="image-20220821100344880" style="zoom:80%;" />

4. 数组类型

~~~
let _hello_world = b"hello_world";// 字符串会根据ASCII转换成u8数组
let _boxes: vector<u64> = std::vector::empty<u64>();// 通过std::vector定义vector<u64>类型的数组
~~~

注意一下，数组类型的两个例子很有代表性。

5. 常量

~~~
const ADDR: address = @test_addr;
const FASLE: bool = false;
~~~

常量是通过const关键字定义的，必须放在方法外面定义。



## 完整代码

我们看一下上面这些例子的完整代码：

~~~
script {
    const ADDR: address = @test_addr;
    const FASLE: bool = false;

    fun main() {
        let _a:u8 = 1;
        let _b: u8;
        _b = 10;
        let c = 2u64;

        let _d = (c as u128);
        let _e = (c as u8);

        let _flag = true;

        let _addr1: address;
        _addr1 = @test_addr;

        let _addr2 = 0x1;

        let _hello_world = b"hello_world";
        let _boxes: vector<u64> = std::vector::empty<u64>();
    }
}
~~~

这里注意一下变量前面的下划线`_`，如果定义的变量在后面没有被使用到，编译的时候会抛错，使用`_`表示你知道后面没被使用，在内存中可以快速回收掉，编译器在编译的时候不会报错。这也是Rust的风格。

最后，编译一下代码：

~~~
move build
~~~



<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h5e5qid72jj20u00xnq51.jpg" alt="move_variable" style="zoom:50%;" />

## 跟我学Move

好了，今天主要是教大家Move的基础数据类型，并且通过基础数据类型定义变量和常量。这里划重点，Move跟其他语言的不同，以及很多Move编程需要注意的细节。

我是@MoveContract（twittter），也可以关注@Move小王子的知乎专栏“手把手写Move智能合约”，跟我一起学习Move。下期我们将会介绍Move的函数定义。
