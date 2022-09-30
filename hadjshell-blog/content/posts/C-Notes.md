---
title: "C Notes"
Date: "2022-02-06"
tags: ["刀法", "C"] 
---



***

## Reference

* [C语言程序设计 - 翁恺](https://www.bilibili.com/video/BV19W411B7w1?spm_id_from=333.1007.top_right_bar_window_custom_collection.content.click)
* Kernighan, B.W. & Ritchie, D.M. (1988) The C programming language. 2nd ed.. Englewood Cliffs, N.J.: Prentice Hall.

***

## 导论

* 计算机的思维方式：枚举

* 算法改进暴力枚举

* 程序的执行：

  * 解释：借助一个程序，那个程序试图理解你的程序，然后按照你的要求执行

  * 编译：借助一个程序，把你的程序翻译成机器语言，然后执行机器语言的程序

    >编译是将源程序翻译成可执行的目标代码，翻译与执行是分开的；而解释是对源程序的翻译与执行一次性完成，不生成可存储的目标代码。这只是表象，二者背后的最大区别是：对解释执行而言，程序运行时的控制权在解释器而不在用户程序；对编译执行而言，运行时的控制权在用户程序。

  * 语言本身没有解释和编译之分，只是具体执行方式不同
  * 现代两种执行方式已经没有过大的区别，各有优劣

* 现代编程语言语法上差异很小，几乎都是C-like语言

* 某些场景，例如操作系统，只能用C写

* 语言的能力/适用领域主要决定因素：
  * 库
  * 传统
  
* FORTRAN -> BCPL -> B -> C

  > C implementations give a better understanding of how the machine behaves. There is no language runtime environment or virtual machine between you and the underlying machine. 

* 标准：`ANSI C` -> `C99`

* C is a general-purpose programming language.

* 指针是C的灵魂

* C的应用场景：

  * 操作系统，编译器
  * 嵌入式系统
  * 驱动程序
  * 底层驱动
  * 图形引擎、图像处理、声音效果

* Ｃ是一种工业语言
  * 开发效率＞＞学习过程
  * 开发效率＞＞开发乐趣

* 历史原因，Ｃ的编译器有很多，出现了“方言”现象

***

## 变量和常量

* 变量是一个保存数据的地方，是一段存储空间的别名
* 变量定义：`<type> <identifier>;`
* 标识符：字母，数字，下划线；第一个字符必须为字母或下划线；大小写敏感；不能用保留字
* 赋值：`=`右边的值赋给左边的变量
* 初始化：定义变量时赋值
* 没有初始化的变量，值为随机数
* C语言类型严格
* `C99`可以在任意位置定义变量；`ANSI C`只能在代码开头定义变量
* 字面量（literal）：直接写在程序里的数值（magic number）
* 常量定义：`const <type> <identifier> = <value>`
* 常量好处：物理意义；便于Debug

***

## 数据类型

* C语言的变量，必须：

  * 在使用前定义
  * 确定类型

* C以后的语言向两个方向发展：

  * C++，Java更强调类型，对类型检查严格

  * JavaScript，Python，PHP不看重类型，甚至不需要事先定义

    > 支持强类型的观点认为明确的类型有助于尽早发现程序中的简单错误
    >
    > 反对强类型的观点认为多于强调类型迫使程序员面对底层实现而非事务逻辑

* C语言需要类型，但是对类型的安全检查并不够

* | 类型       | 关键字                                      |
  | ---------- | ------------------------------------------- |
  | 整数       | `char`, `short`, `int`, `long`, `long long` |
  | 浮点数     | `float`, `double`, `long double`            |
  | 逻辑       | `bool`                                      |
  | 指针       |                                             |
  | 自定义类型 |                                             |

* 数据类型的区别：

  * 内存中占据的大小
  * 内存中的表达形式：补码，编码

* `sizeof()`：是一个运算符，给出某个**类型**或**变量**在内存中所占据的字节数

  * 静态运算符，结果在编译时刻就决定了
  * 不要在`sizeof`的括号里做运算，这些运算是不会做的

* 字面量也有数据类型

  * 默认`int, double, char, string`
  * 可以指定类型：加后缀

### 整型

* | 类型        | 字节                |
  | ----------- | ------------------- |
  | `char`      | 1                   |
  | `short`     | 2                   |
  | `int`       | 取决于编译器（CPU） |
  | `long`      | 取决于编译器（CPU） |
  | `long long` | 8                   |

* 计算机内部表达：补码

* | 类型  | 范围            |
  | ----- | --------------- |
  | `char` | -128 ~ 127     |
  | `short` | -32768 ~ 32767 |
  | `int` | -2<sup>31</sup> ~ 2<sup>31</sup> - 1 (-2147483648 ~ 2147483647) |

* `unsigned`

  * 初衷并非扩展数能表达的范围，而是为了做纯二进制运算，主要是为了移位

* 整数是以纯二进制方式进行运算的

* 整数越界

* | 格式化输入输出 | 类型                     |
  | -------------- | ------------------------ |
  | `%d`           | `char, short, int, long` |
  | `%u`           | `unsigned`               |
  | `%ld`          | `long long`              |
  | `%lu`          | `unsigned long long`     |
  | `%o`           | 8进制                    |
  | `%x`           | 16进制                   |

* 以`0`开始的数字字面量是8进制；以`0x`开始的数字字面量是16进制

* 为什么有这么多整数类型？

  * 为了准确表达内存，做**底层程序**的需要
  * 历史原因

* 没有特殊需要，选择`int`

  * 现在CPU字长普遍32位或64位，一次内存读写就是一个`int`，一次计算也是一个`int`，选择更短的类型不会更快，甚至可能更慢
  * 现代编译器一般会设计内存对齐，所以更短的类型实际在内存中有可能也占据一个`int`的大小
  * `unsigned`与否只是输出的不同，内部计算是一样的

### 浮点类型

* | 类型     | 字长 | 范围                                                       | 有效数字 |
  | -------- | ---- | ---------------------------------------------------------- | -------- |
  | `float`  | 32   | $\pm(1.20 * 10^{-38} - 3.40 * 10^{38}), 0, \pm inf, nan$   | 7        |
  | `double` | 64   | $\pm(2.20 * 10^{-308} - 1.79 * 10^{308}), 0, \pm inf, nan$ | 15       |

  * `printf`输出`inf`表示超过范围的浮点数，输出`nan`表示不存在的浮点数

    ```c
    int main() {
        printf("%f\n", 12.0 / 0.0);
        printf("%f\n", -12.0 / 0.0);
        printf("%f\n", 0.0 / 0.0);
        
        return 0;
    }
    
    Output:
    inf
    -inf
    nan
    ```

* | 类型     | `scanf` | `printf` |
  | -------- | ------- | -------- |
  | `float`  | `%f`    | `%f, %e` |
  | `double` | `%lf`   | `%f, %e` |

  * `%e`：科学计数法格式
  * 字面量也可使用科学计数法格式，例：`1e-10`
  * 输出精度，例：`%.5f`，四舍五入

* 浮点类型对数据的表示是不准确的，是近似的

  * 二进制能表示的数是有限的，但数是连续的

* 浮点运算没有精度

  * 不能直接比较
  * 误差会累积
  * 转而使用整型解决实际问题

  ```c
  int main() {
      float a, b, c;
      
      a = 1.345f;
      b = 1.123f;
      c = a + b;
      if(c == 2.468)
          printf("Equal!\n");
      else
          printf("Not equal! c = %.10f, or %f\n", c, c);
      
      return 0;
  }
  
  // f1 == f2 可能失败
  // fabs(f1 - f2) < 1e-12 来比较
  ```

* 浮点数内部表达：编码

* 浮点数在计算时是由专用的硬件部件实现的

* 没有特殊需要，使用`double`

### 字符类型

* `char`既是整型，也是字符型
* 格式化输入输出：`%c`
* 字符字面量：`''`表示
* ASCII码

#### 转义字符-Escape character

* 用来表达无法打印出来的控制字符或特殊字符，由`\`开头，后面跟上一个字符

* | 常用转义字符 | 意义           |
  | ------------ | -------------- |
  | `\b`         | 回退一格       |
  | `\t`         | 到下一个表格位 |
  | `\n`         | 换行           |
  | `\r`         | 回车           |
  | `\"`         | 双引号         |
  | `\'`         | 单引号         |
  | `\\`         | 反斜杠本身     |

  * 回退一般不代表删除
  * 制表位：每行的固定位置，而不是固定大小的字符数量
  * 换行实际上是回车和换行两个动作

### 类型转换

#### 自动类型转换

* 当运算符两边出现不一致的类型时，会自动转换成较大的类型
* `char -> short -> int -> long -> long long`
* `int -> float -> double`
* 对于`printf`，任何小于`int`的类型会被转换成`int`，`float`会被转换成`double`
* `scanf`不会

#### 强制类型转换

* `(type) value`
* 注意安全性
* 只是从那个变量计算出了一个新的类型的值，它并不改变那个变量，无论是值还是类型
* 强制类型转换的优先级高于四则运算

### 逻辑类型

* C语言原本是没有`bool`类型的
* `#include <stdbool.h>`
* 之后可以使用`true`，`false`
* 实际上是整型，`1`和`0`

***

## 运算

* 表达式：一系列运算符和操作数的组合

* 运算符（operator）

  | 运算类型 | 运算符          |
  | -------- | --------------- |
  | 四则     | + - * / % ()    |
  | 关系     | < > <= >= == != |
  | 逻辑     | && \|\| !       |
  | 位       | & \| ^ >> << ~  |
  | 单目     | + -             |
  | 赋值     | =               |
  | 复合赋值 | += -= *= /= %=  |
  | 递增递减 | ++ --           |
  | 条件     | ? :             |
  | 逗号     | ,               |

* 操作数（operand）

### 四则

* 整型除法舍弃小数部分
* 浮点数不能取余
* 优先级：正负 > 乘除 > 取余 > 加减 > 赋值

### 赋值

* 赋值也是运算，也有结果
* 例：`a = 6`的结果是`6`
* 自右向左结合

### 递增递减

* 前缀：`a++`结果是`a`加`1`以前的值
* 后缀：`++a`结果是`a`加`1`以后的值
* `a + 1`是附作用

### 关系

* 比较结果：逻辑值真1假0
* 优先级比算术低，比赋值高
* 连续的关系运算从左向右执行

### 逻辑

* 短路法则：逻辑运算是自左向右进行的，如果左边的结果已经能够决定结果了，就不会做右边的计算

### 逗号

* 逗号用来连接两个表达式，并以右边的表达式的值作为它的结果
* 逗号运算符的优先级是最低的
* 主要在`for`中使用

### 位运算

* 直接对二进制位进行操作
* 按位与常用于：
  * 让某一位或某些位为0：和0与
  * 取一个数中的一段：`x & 0x00ff0000`
* 按位或常用于：
  * 使得一位或一些位为1：和1或
  * 把两个数拼起来：`0x00ff | 0xff00`
* 按位异或：相同为0，不同为1
  * 某位取反：和1异或
  * 某位不变：和0异或
* 左移
  * 高位移出，低位补零
  * 所有小于`int`的类型，移位以`int`的方式做，结果是`int`
  * `x <<= 1 == x *= 2`
* 右移
  * 低位移出，高位补符号位
  * `x >>= 1 == x /= 2`

***

## 流程控制

* 结构化的基石
* 块：`{}`

### 选择

#### If - else

* 二路分支
* 嵌套
* 级联
* else总是和最近的if匹配
* “单一出口”原则

#### Switch

* 多路分支
* 控制表达式只能是整数型
* `case`常量可以是常数，也可以是常数计算的表达式
* `case`判断代码块内执行的开始
  * 合并分支
* `break`语句，没有`break`会顺序向下执行
* 默认情况执行`default`下的语句
* `default`不是必须但是最好有

### 循环

#### While

* 先判断条件再执行循环体

#### Do while

* 先执行一次循环体再判断条件

#### For

* 如果有固定次数，用`for`；如果必须执行一次，用`do while`；其他情况用`while`

* 循环控制：`break`，`continue`；最近匹配原则，针对当前循环

  > `break`对`if-else`不起作用

* 循环嵌套

* 跳出多重循环：`goto`

* 给定条件的整数集：枚举 + 剔除

***

## 函数

* 函数是可以重复利用的代码块，接受零个或多个参数，做一件事情，并返回零个或一个值

* 函数本质是占用一片连续内存的代码

* 函数定义
  * 函数头：返回类型 函数名 参数列表
  * 函数体

* 函数调用：`函数名(参数值);`
  * 执行 -> 调用函数 -> 执行调用函数 -> 返回 -> 恢复执行

* 函数返回：`return`
  * `return`停止函数的执行，并返回一个值
  * `return;`, `return exp;`

* `void`
  * `void`不能用于定义变量
  * 用于函数定义，表示无返回值或无参数
  * `type func();`表示接受任意多的参数

* 函数的先后关系
  * C的编译器自上而下顺序分析
  * 调用函数需要先声明或定义
  * 声明定义要一致

* 参数传递
  * 值传递
  * 调用函数时给的值与参数类型不匹配是C语言传统上最大的漏洞，编译器会隐式地进行类型转换
  * 每个函数有自己的变量空间，参数也位于这个空间中

* 变量分类
  * 局部（本地）变量：函数的每次运行，就产生了一个独立的变量空间，在这个空间中的变量，是函数这次运行所独有的，称作局部变量
    * 定义在函数内部的变量，参数
    * 定义在语句块内的变量
    * 只能在当前**块**中访问使用
    * 块外定义变量在块内仍然有效
    * 本地变量**不会被默认初始化**
    * 参数在进入函数时会初始化
  * 全局变量：定义在函数外面的变量
    * 没有初始化的全局变量会得到`0`值
    * 指针会得到`null`值
    * 只能用编译时已知的值来初始化全局变量
    * 全局变量初始化发生在`main`函数之前
    * 工程中，全局变量通常以`g`作为前缀命名
  * 同名变量
    * 不同块中的局部变量可以同名
    * 同一块中的局部变量不可以同名
    * 块内同名变量覆盖块外同名变量
  * 静态变量：`static`
    * 静态本地变量
      * 作用域：块内；生命期：程序生命期
      * 静态本地变量只会在第一次进入函数时初始化一次
      * 离开函数时，静态本地变量会继续存在并保持其值
      * 特殊的全局变量，创建于全局数据区
    * 静态全局变量
      * 作用域：文件作用域；生命期：程序生命期
  * 使用全局变量和静态本地变量的函数是线程不安全的

* 作用域：变量定义后的可访问范围
  * 局部变量：定义开始到代码块结束
  * 全局变量：程序的任何地方

* 生命期：变量从创建到销毁的时间
  * 局部变量：进入作用域时创建，离开作用域时自动销毁
  * 全局变量：程序开始运行时创建，程序结束时自动销毁

* 调用函数里的`,`不是运算符

* C语言不允许函数嵌套定义，可以放声明

* C语言程序的入口：`main`函数
  * `main()`是应用程序与操作系统的“约定”

* C语言可以定义参数可变的函数

  * 参数可变函数的实现依赖于`stdarg.h`
  * `va_list`：参数集合
  * `va_arg`：取具体参数值
  * `va_start`：标识参数访问的开始
  * `va_end`：标识参数访问的结束
  * 可变参数必须从头到尾按照顺序逐个访问
  * 参数列表中至少要存在一个确定的命名参数
  * 可变参数函数无法确定实际存在的参数的数量
  * 可变参数函数无法确定参数的实际类型

  ```c
  #include <stdio.h>
  #include <stdarg.h>
  
  float average(int n, ...)
  {
      va_list args;
      int i = 0;
      float sum = 0;
      
      va_start(args, n);
      
      for(i=0; i<n; i++)
      {
          sum += va_arg(args, int);
      }
      
      va_end(args);
      
      return sum / n;
  }
  
  int main()
  {
      printf("%f\n", average(5, 1, 2, 3, 4, 5));
      printf("%f\n", average(4, 1, 2, 3, 4));
      
      return 0;
  }
  ```

* 函数设计原则
  * 函数从意义上应该是一个独立的功能模块
  * 函数名要在一定程度上反映函数的功能
  * 函数参数名要能够体现参数的意义
  * 尽量避免在函数中使用全局变量
  * 当函数参数不应该在函数体内部被修改时，应该加上`const`声明
  * 如果参数是指针，且仅作输入参数，则应加上`const`声明
  * 不能省略返回值的类型
  * 对参数进行有效性检查，对指针参数的检查尤为重要
  * 函数体的规模要小，尽量控制在80行代码之内
  * 相同的输入对应相同的输出，避免有状态函数
  * 避免函数有过多的参数，尽量控制在4个以内
    * 太多可以考虑结构体
  * 有时候函数不需要返回值，但为了增加灵活性，如支持链式表达，可以附加返回值

***

## 数组

* 数组是相同数据类型变量的有序集合
* 数组定义：`<type> identifier[size];`
* 数组类型：`<type>[size]`
* 元素数量必须是整数，必须是编译时刻确定的字面量
  * **`C99`之后可以使用变量**
* 数组在计算机内是一片连续的内存
* 数组一旦创建，**不能改变大小**
* 数组访问：`identifier[index]`
  * 数组下标从`0`开始
  * 可以使用变量作为下标
  * 编译器和运行环境都不会检查数组下标是否越界，无论读写
  * 一旦程序运行，数组越界**可能**造成问题，导致程序崩溃
  * 不同机器上无法复现程序的一大问题：没有考虑数组越界
* 长度为0数组可以定义，但是无用，自然越界
* 数组初始化
  1. `<type> identifier[N] = {v0, v1, ..., vn-1};` 不初始化是随机值
  2. 自动确定数组大小：`<type> identifier[] = {v0, v1, ..., vn-1};`
  3. 部分初始化：`<type> identifier[N] = {[0] = 2, [2] = 3, 6, };`
     * 用`[n]`在初始化数据中给出定位
     * 没有定位的数据姐在前面的位置后面
     * 其他位置补零
     * 适合初始数据稀疏的数组
* 数组大小：`sizeof(identifier)`
* 数组个数：`sizeof(identifier) / sizeof(identifier[0])`
* 数组变量本身不能被赋值。要想拷贝一个数组，只能遍历
* 数组作为函数参数时，往往必须再用另一个参数来传入数组的大小
* 数组变量本身表达地址，无需用`&`取地址；数组单元表达的是变量
* 二维数组
  * `<type> identifier[M][N];`
  * 通常理解为M行N列的矩阵，或者M个大小为N的一维数组
  * 二维数组初始化列数必须给出，行数可以自动确定
  * 检查行
  * 检查列
  * 检查对角线，反对角线
* `const`数组
  * `const <type> identifier[] = {0, };`
  * 表明每个数组单元都是`const`
  * 必须通过初始化进行赋值
  * **保护数组**不被函数破坏


***

## 指针

* `&`运算符：获得变量的地址，他的操作数必须是变量

  * 格式化输出：`%p`
  * 地址的大小与编译器有关
  * 内存地址本质上是一个无符号整数

* 指针变量：保存地址的变量

  * 普通变量放的是值
  * 禁止将普通数值当作地址赋值给指针变量，除非你知道自己在干什么

* 指针定义：`<type> *p;`或者`<type>* p;`

  * `<type>`决定**访问内存时的长度范围**

    > 个人理解，指针类型也是个数据类型

* 访问指针地址上的变量：`*p`

  * 可以做右值也可以做左值
  * 没有赋值前不要访问

* 指针应用场景

  * 交换变量
  * 函数返回多个值，某些值就只能通过指针返回
    * 传入的参数实际上是需要保存结果的变量的地址
  * 函数返回运算的状态`(0, 1)`，结果通过指针返回
    * 可能会出错的运算
    * 后续语言（`C++`，`Java`）采用了异常机制来解决这个问题

* 指针与数组

  * 函数参数表中的数组实际上是指针，但是可以用数组的运算符`[]`进行计算，不再含有数组长度信息

  * `void func(int a[]);`和`void func(int* a);`作为函数原型是等价的

  * **数组类型和指针类型不是等价的**

  * `[]`运算符可以对数组做，也可以对指针做

  * `*`运算符可以对指针做，也可以对数组做

  * `array`, `&array`, `&array[0]`区别

    * `array`可以**当作**一个**指针常量**，指向**数组第一个元素**

      * 两种情况数组名不是用指针常量来表示
        1. `sizeof(array)`：返回整个数组的长度，而不是指针的长度
        2. `&array`：返回一个指向数组的指针，而不是一个指向指针的指针

    * `&array`是一个**指针值**，指向**整个数组**

    * `&array[0]`是一个**指针值**，指向**数组的第一个元素**

    * 三者值相同，但意义不同

      > [这是一个很好的解释](https://blog.csdn.net/jingzi123456789/article/details/66478310)

    ```c
    int main() {
        int array[5] = {0};
        int (*p)[5] = &array;	// pointer to whole array
    
        // ERROR
        // Because array is a const pointer
        // array = 5345454;
    
    	printf("        array = %p\n", array);
    	printf("       &array = %p\n", &array);
    	printf("    &array[0] = %p\n", &array[0]);
    	printf("    array + 1 = %p\n", array + 1);
    	printf("&array[0] + 1 = %p\n", &array[0] + 1);
    	printf("   &array + 1 = %p\n", &array + 1);
    
    	printf("\n");
    
    	printf(" sizeof(array) = %d\n", sizeof(array));
    	printf("sizeof(&array) = %d\n", sizeof(&array));
    
    	printf("\n");
    
        return 0;
    }
    
    Output:
            array = 0061FEEC
           &array = 0061FEEC
        &array[0] = 0061FEEC
        array + 1 = 0061FEF0
    &array[0] + 1 = 0061FEF0
       &array + 1 = 0061FF00
    
    sizeof(array) = 20
    sizeof(&array) = 4
    ```

* 指针与`const`

  * 指针是`const`：
    * `<type>* const p;`
    * 该指针变量一旦得到了地址值，不能再指向其他变量
  * 指针所指的是`const`：
    * `<type> const* p;`，`const <type>* p;`
    * 表示不能通过这个指针去修改指向的变量，**并不能**使得那个变量成为`const`
  * 总是可以把一个非`const`的值转换成`const`
    * 当要传递的参数类型比地址大的时候，这是一种常用的手段：既能用较少的字节数传递值给参数，又能避免函数修改外面的变量

* 指针运算

  * 给指针加，减一个整数

    * **偏移量**
    * `p + n -> (unsigned int) p + n * sizeof(*p)`
    * 如果指针不是指向一片连续分配的空间，如数组，则这种计算没有意义

  * 递增递减

  * 两个指针相减

    * 结果也是偏移量

  * `*p++`

    * 取出`p`所指的数据，然后把`p`移到下一个位置去

    * `*`的优先级低于`++ --`

    * 常用于数组类的连续空间操作

      ```c
      char array[] = {0, 1, 2, -1};
      char* p = array;
      
      while(*p != -1) {
          printf("%d", *p++);
      }
      ```

    * 在某些CPU上，这可以直接被翻译成一条汇编指令

  * 比较

* 0地址

  * 所有程序都有0地址，通常不能随意读写
  * `NULL`是一个预定义的符号，表示0地址
  * 可以用0地址来表示特殊的事情
    * 返回的指针是无效的
    * 指针没有被真正初始化（**先初始化为0**）

* 指针的类型转换

  * `void*`表示不知道指向什么的指针
    * 不能访问数据
    * 计算与`char*`相同（不相通）
    * 往往用在底层程序中
  * `(<type>*) identifier;`
  * 并不改变指向变量的类型

* 动态内存分配

  * ```c
    #include <stdlib.h>
    <type>* p = (<type>*) malloc(n * sizeof(<type>));
    if(p != NULL) {
        // statement
    }
    free(p);
    p = NULL;
    // 动态分配内存的基本操作架构
    ```
    
  * `C99`可以直接用变量定义数组大小

  * `void* malloc(size_t size);`

    * 申请的空间大小是以字节为单位的
    * 需要自己将返回结果转换成自己需要的类型
    * 申请失败返回`0(NULL)`

  * `free()`

    * 把申请的来的空间还给系统
    * 只能还**申请**来的空间的**首地址**
    * `free(NULL)`不会出错

* 指针与函数

  * 函数名就是函数体代码的起始地址

  * 通过函数名调用函数，本质为指定具体地址的跳转执行

  * 可以定义指针保存函数入口地址

  * `<type> (*pFunc)(<type1>, <type2>) = func;`

  * `&func`和`func`数值相同，意义相同

  * 调用函数：`pFunc(par);`或`*pFunc(par);`

  * 不能进行指针运算

  * 应用：

      * 让程序跳转到固定地址执行，多用于嵌入式开发
      * 回调函数（监听器模式的基础）
          * 调用者**不知道**具体事件发生时需要的调用函数
          * 被调函数**不知道**何时被调用，只知道需要完成的任务
          * 当具体时间发生时，调用者通过函数指针调用具体函数
          * 回调机制中调用者和被调函数**互不依赖**

      ```c
      #include <stdio.h>
      
      typedef int(*Weapon)(int);
      
      void fight(Weapon wp, int arg)
      {
          int result = 0;
          
          printf("Fight boss!\n");
          
          result = wp(arg);
          
          printf("Boss loss: %d\n", result);
      }
      
      int knife(int n)
      {
          int ret = 0;
          int i = 0;
          
          for(i=0; i<n; i++)
          {
              printf("Knife attack: %d\n", 1);
              ret++;
          }
          
          return ret;
      }
      
      int sword(int n)
      {
          int ret = 0;
          int i = 0;
          
          for(i=0; i<n; i++)
          {
              printf("Sword attack: %d\n", 5);
              ret += 5;
          }
          
          return ret;
      }
      
      int gun(int n)
      {
          int ret = 0;
          int i = 0;
          
          for(i=0; i<n; i++)
          {
              printf("Gun attack: %d\n", 10);
              ret += 10;
          }
          
          return ret;
      }
      
      int main()
      {
          fight(knife, 3);
          fight(sword, 4);
          fight(gun, 5);
          
          return 0;
      }
      ```

  * 返回指针的函数

    * 返回本地变量的地址是危险的
    * 返回全局变量或静态本地变量的地址是安全的
    * 返回在函数内`malloc`的内存是安全的，但是容易造成问题
    * 最好的做法是返回传入的指针

* 数组参数退化

  | 数组参数          | 等效的指针参数   |
  | ----------------- | ---------------- |
  | `<type> a[size]`  | `<type>* a`      |
  | `<type>* a[size]` | `<type>** a`     |
  | `<type> a[m][n]`  | `<type> (*a)[n]` |

***

  ## 字符数组和字符串

* 字符数组：`char word[] = {'H', 'E', 'L', 'L', 'O'};`

### 字符串

* C语言实际上没有字符串概念，用字符数组模拟

* `char word[] = {'H', 'E', 'L', 'L', 'O', '\0'};`

* 以`0`（整数0）结尾的一串字符

* `0`或`\0`是一样的（字节大小不同，4：1），但是和`'0'`不同

* `0`表示字符串的结束，但它不是字符串的一部分，计算长度时不包含这个`0`

* 字符串以数组形式存在，以数组或指针的形式访问，更多的是以指针的形式

* `string.h`

* 字符串常量（字面量）

  * `"string"`
  * 会被编译器变成一个字符数组放在**全局只读代码段**，数组长度是`字符个数+1`，结尾有表示结束的`0`
  * 两个相邻的字符串常量会连接起来
  * 可以当作常量指针

* 字符串变量

  * `char* str = "Hello";`
    * `str`是一个指针，初始化为指向一个字符串常量
    * 因为字符串常量不能被修改，所以实际上`str`是`const char*`类型
    * 历史原因，编译器接受不带`const`的写法
    * 试图对`str`所指的字符串做写入会导致程序崩溃
  * `char word[] = "Hello";`
    * 定义为数组类型可以修改内部的字符串
    * 实际上编译器隐式地将字符串常量拷贝到定义的数组空间
  * 指针还是数组？
    * **构造**字符串：数组
    * **处理**字符串：指针
  * `char line[10] = "Hello";`

* 字符串赋值：`char* str1 = "hello";    char* str2 = str1;`

  * 并没有产生新的字符串，只是让`str2`指向了`str1`所指的字符串

* 字符串输入输出：`%s`

  * `scanf`读入一个单词（到空格，tab或回车为止）
  * `scanf`是不安全的，因为不知道要读入的内容的长度
  * `%7s`：安全的输入，数字表示最多允许读入的字符数量。如果输入的长度超过指定长度，下一次的`scanf`会在上一次读入的终点接着读

* 空字符串

  * 只有一个`\0`的字符串
  * `char buffer[100] = "";`
    * 一个空字符串，`buffer[0] == '\0'`
  * `char buffer[] = "";`
    * 这个数组长度只有1

* 字符串数组

  * `char** a`

    * `a`是一个指针，指向另一个指针，那个指针指向一个字符（串）
    * 也可以**看成**一个指针数组，前提访问合法

  * `char a[][size]`

    * 可以表示一个字符串数组，但是里面的字符串大小有限制

  * `char* a[]`

    * 也可以表示一个字符串数组，里面是一个个指向字符串的指针

    * `<type> (*p1)[size]`和`<type>* p2[size]`的区别

      > [一个很好的解释](https://www.geeksforgeeks.org/difference-between-int-p3-and-int-p3/)
      >
      > 一个是指向整个数组的指针，一个是数据元素是指针的数组
      >
      > `*p1`返回的是指向的数组的首地址
      >
      > `*p2`返回的是指针数组里第一个指针元素的值

* 程序参数

  * `int main(int argc, char const *argv[])`
  * `argv[0]`是命令本身，当使用`Unix`的符号链接时，反应符号链接的名字

* `putchar`

  * `int putchar(int c);`
  * 向标准输出写一个字符
  * 返回写了几个字符，`EOF(-1)`表示写失败

* `getchar`

  * `int getchar(void);`
  * 从标准输入读入一个字符
  * 返回读到的字符
  * 返回`EOF`
    * Windows：`Ctrl + Z`
    * Unix：`Ctrl + D`
  * `shell`进行行编辑，然后程序从缓冲区读入数据

* 字符串函数

  * `strlen`

    * `size_t strlen(const char *s);`
    * 返回字符串的长度

  * `strcmp`

    * `int strcmp(const char *s1, const char *s2);`
    * 比较两个字符串，返回：
      * `0`：`s1 == s2`
      * `s1[idx] - s2[idx]`：`s1 != s2`，`idx`是第一个不相等字符的下标

  * `strcpy`

    * `char* strcpy(char* restrict dst, const char* restrict src);`

    * 把`src`的字符串拷贝到`dst`，返回`dst`

    * `restrict`关键字：表明不重叠（`C99`）

      ```c
      char* dst = (char*) malloc(strlen(src) + 1);
      strcpy(dst, src);
      ```

  * `strcat`

    * `char* strcat(char* restrict s1, const char* restrict s2);`
    * 把`s2`拷贝到`s1`的后面，接成一个长的字符串，返回`s1`
    * `s1`必须有足够的空间

  * `strcpy`和`strcat`都可能出现安全问题，即目标没有足够的空间。尽可能不要使用这两个函数

  * 安全版本

    * `char* strncpy(char* restrict dst, const char* restrict src, size_t n);`
    * `char* strncat(char* restrict s1, const char* restrict s2, size_t n);`

  * `strncmp`

    * `int strncmp(const char *s1, const char *s2, size_t n);`
    * 比较前n个字符

  * `strchr`，`strrchr`

    * 字符串中找字符，一个从左开始，一个从右开始

    * `char* strchr(const char* s, int c);`

    * `char* strrchr(const char* s, int c);`

    * 返回指针，返回`NULL`表示没有找到

    * 找第二个

      ```c
      char s[] = "Hello";
      char* p = strchr(s, 'l');
      p = strchr(p + 1, 'l');
      ```

    * 找到后拷贝

      ```c
      char s[] = "Hello";
      char* p = strchr(s, 'l');
      
      // 拷贝后一段
      {
          char* t = (char*) malloc(strlen(p) + 1);
      	strcpy(t, p);
          printf("%s\n", t);
          free(t);
      }
      
      // 拷贝前一段
      {
          char c = *p;
          *p = '\0';
          char* t = (char*) malloc(strlen(s) + 1);
          strcpy(t, s);
          *p = c;
          printf("%s\n", t);
          free(t);
      }
      ```

  * `strstr`，`strcasestr`

    * 字符串中找字符串，`strcasestr`忽略大小写
    * `char* strstr(const char* s1, const char* s2);`
    * `char* strcasestr(const char* s1, const char* s2);`
  
  * `strtok`

***

## 枚举

* 常量符号化：`const`
* 枚举是一种用户定义的数据类型，意义是给一些可以排列起来的常量值名字
* `enum typeName {name 1, name 2, ..., name n};`
* 枚举类型本质上是**整型**
* 声明枚举量可以指定值
* 枚举类型名字通常并不真的使用，要用的是大括号里的名字，因为它们就是常量符号，类型是`int`，值则依次从`0`到`n`
* 类型名字可以省略
* 定义枚举变量：`enum typeName var = name 1;`
* 枚举变量本质上是**整型变量**
* 套路：自动计数的枚举 `enum COLOR {RED, YELLOW, GREEN, NumCOLORS};`
* 虽然枚举可以当作类型使用，但实际上很少用
* 如果有意义上排比的名字，用枚举比`const int`方便
* 枚举比宏好，因为枚举量有`int`类型

***

## 自定义数据类型

### 结构体

* 结构体变量的本质是变量的集合

* 结构体变量中的成员占用独立的内存

* 结构类型声明，定义结构体变量，访问成员变量，初始化

  ```c
  // First form
  struct <yourType> {
      <type> var1;
      <type> var2;
      <type> var3;
  };
  
  struct <yourType> yourVar;
  
  // Second form
  struct {
      <type> var1;
      <type> var2;
      <type> var3;
  }yourVar1, yourVar2;
  
  // Third form
  struct <yourType> {
      <type> var1;
      <type> var2;
      <type> var3;
  }yourVar1, yourVar2;
  
  // access
  yourVar.var1 = <value>;
  
  // Initialisation
  struct <yourType> yourVar1 = {<value1>, <value2>, <value3>};
  struct <yourType> yourVar1 = {.var1 = <value1>, .var2 = <value2>};	// yourVar1.var3 == 0
  ```

  * 在函数内部声明的结构类型只能在函数内部使用
  * 通常在函数外部声明结构类型
  * 不同无名结构体变量，尽管内部成员变量类型都相同，也不是同一数据类型

* 结构运算

  * 访问整个结构
  * 赋值
    * `struct <type> var;	var = (struct <type>){<value>, };`
    *  `var1 = var2;`
  * 取地址
    * 结构变量的名字并不是结构变量的地址，必须使用`&`运算符
    * `struct <type>* pVar = &var;` 
    * 用指针变量访问结构成员：`(*p).var`或者**`p->var`**
    * `.`，`->`优先级高于`&`
  * 传给函数参数

* 结构与函数

  * 整个结构作为参数的值传入函数
  * 函数内新建一个结构变量，复制调用结构变量的值
  * 可以返回一个结构

* 输入结构

  * 没有直接的方式可以一次`scanf`一个结构
  * 可以自己写一个函数
    * `struct <type> getStruct(void);`
    * `struct <type>* getStruct(struct <type>* p);`
      * 返回传进来的指针的好处是可以把程序连起来

* 结构数组

* 结构中的结构

* 位域：利用结构体类型指定成员变量占用内存的比特位宽度

  * 某些特殊场合，远古代码中可能被使用
  * 位域成员必须是整型，占用位数不能超过类型宽度
  * 当存储位不足时，自动启用新存储单元
  * 可以舍弃当前未使用的位，重新启用存储单元

  ```c
  struct BW {
      unsigned char a : 4;	// a 占用一个字节的4位宽度
      unsigned char b : 5;	// b 占用一个字节的5位宽度
      unsigned char   : 0;	// 重启一个存储单元表示新的成员
      unsigned char c : 2;	// c 占用一个字节的2位宽度
  }
  ```

### `typedef`

* 声明一个已有数据类型的新名字

* 没有创建新类型，只是创建了类型别名

* `typedef <type> <newTypeName>;`

* 改善了程序的可读性

  ```c
  // typedef basic data type
  typedef unsigned char byte;
  
  // typedef function type
  int func(int a);
  int (*pFunc)(int) = func;
  typedef int(IFuncI)(int);
  IFuncI* pFunc = func;
  
  // typedef array type
  float array[5];
  float (*pArray)[5] = &array;
  typedef float(FArr5)[5];
  FArr5* pArray = &array;
  
  // typedef struct type
  typedef struct {
      double x;
      double y;
  } Node;
  Node aNode = {0.0, 0.0};	// rather than struct Node aNode;
  ```

### 联合

* `union`

* 语法上和`struct`一样

* `sizeof(union <name>) == sizeof(每个成员)的最大值`

* 所有成员共享一个空间

* 同一时间只有一个成员是有效的

* `union`类型的变量只能以第一个成员类型的有效值进行初始化

* 应用

  * 判断系统大小端

    ```c
    int isLittleEndian() {
        union {
            int i;
            char a[4];
        }test;
        test.i = 1;
        return (test.a[0] == 1);
    }
    ```

***

## 宏

* 编译预处理指令：`#`开头

* 预处理指令不是C语言的成分

* 宏定义：`#define <name> <value>`

* 没有`;`因为不是C语句

* 名字必须是单词，值可以是各种东西

* 在C语言的编译器开始编译之前，编译预处理程序（The C Preprocessor）会把程序中的名字换成值

* 预处理宏：**简单的文本替换**

* 预处理器不会对宏定义进行语法检查

* 如果一个宏的值中有其他宏的名字，也会被替换

* 如果一个宏的值超过一行，最后一行之前的行末需要加`\`

* 宏的值后面出现的注释不会被当作宏的值的一部分

* 宏定义之后，后面代码可以随意使用，**没有作用域的概念**

* 没有值的宏
  * 例：`#define _DEBUG`
  * 这类宏是用于条件编译的，后面有其他的编译预处理指令来检查这个宏是否已经被定义过了
  * 定义宏编译指令：`gcc -DMACRO=1 file.c`

* 预定义的宏
  * `__LINE__`：当前所在行的行号
  * `__FILE__`：源代码文件的文件名
  * `__DATE__`：编译时的日期
  * `__TIME__`：编译时的时间
  * `__STDC__`：编译器是否遵循标准C规范，值为`1`或`0`

* 带参数的宏
  * 类似函数的宏
  * `()`的原则
    * 一切都要带括号
    * 整个值要括号
    * 参数出现的地方要括号
  * 宏表达式中不能出现递归定义

* 宏和函数

  * 宏是由预处理器直接替换展开的，编译器不知道宏的存在，所以不安全

    函数是由编译器直接编译的实体，调用行为由编译器决定

  * 多次使用宏会导致最终可执行程序的体积增大

    函数是跳转执行的，内存中只有一份函数体存在

  * 宏的效率比函数要高，因为直接展开，无调用开销

    函数调用时会创建活动记录，效率不如宏

  * 使用原则

    * 能用函数实现的使用函数

    * 宏可以用于生成一些常规性的代码；封装函数，加上类型信息

      ```c
      // 函数封装
      #define MALLOC(type, x)   (type*)malloc(sizeof(type) * x)
      #define FREE(p)           (free(p), p = NULL)
      
      #define LOG_INT(i)     printf("%s = %d\n", #i, i)
      #define LOG_CHAR(c)    printf("%s = %c\n", #c, c)
      #define LOG_STRING(s)  printf("%s = %s\n", #s, s)
      
      #define FOREACH(i, n)	 for(int i = 0; i < n; i++)
      #define BEGIN			{
      #define END				}
      ```


***

## 多文件程序设计

* 一个源代码文件太长了适合分成几个文件

* 工程项目 -> 功能X，Y，Z...

* 编译和链接

  * 一个`.c`文件是一个编译单元

  * 编译器每次编译只处理一个编译单元

  * 编译完形成`.o`文件，目标代码文件

    > A file ending in .o is an *object file*. The compiler creates an object file for each source file, before linking them together, into the final executable.

  * 多个`.o`文件链接起来得到可执行文件`.exe`

* 文件可以定义为功能接口（可被其他文件的函数或数据）

  * 源文件：**代码实现**文件，`.c`
    * 标准库的函数代码实现在某个`.lib`（Windows）或`.a`（Unix）中
  * 头文件：源文件的**接口**定义文件，`.h`

* 声明和定义

  * 声明是不产生代码的东西，意义是告诉编译器程序单元的存在
    * C语言中通过`extern`进行程序单元的声明，一些程序单元可以省略该关键字
    * 函数原型
    * 变量声明
    * 结构声明
    * 宏声明
    * 枚举声明
    * 类型声明
    * `inline`函数
  * 定义是产生代码的东西，意义是指示程序单元的意义

* 头文件`.h`

  * 存放声明
    * 规矩上，只有声明可以被放在头文件中
    * 如果放定义，会造成一个项目中多个编译单元里有重名的实体
    * 某些编译器允许几个编译单元中存在同名函数，或者用`weak`修饰符来强调这种存在
  * 在需要调用头文件中的函数的源代码文件`.c`中`#include`这个头文件，就能让编译器在编译的时候知道这个函数的原型，**保证你调用时给出的参数值和返回值是正确的类型**
  * `#include`是一个编译预处理指令，会把那个文件的全部文本内容**原封不动**地插入到他所在的地方
  * `#include`的两种形式
    * `""`：要求编译器首先在当前目录中（`.c`文件所在的目录）寻找这个文件，如果没有，到编译器指定的目录去找
    * `<>`：让编译器只在指定的目录去找
  * 编译器自己知道自己的标准库的头文件在哪里，环境变量和编译器命令行参数也可以指定寻找头文件的目录
  * 现在的C语言编译器默认会引入所有的标准库
  * 在使用和定义函数的地方都应该`#include`这个头文件
    * 使用：编译器可以检查函数调用的正确性
    * 定义：编译器可以检查对外宣称的函数原型和实际定义是否一致
    * 一般的做法是任何的`.c`都有对应的同名`.h`（`main.c`除外），把所有对外公开的函数的原型和全局变量的声明都放进去
  * 不对外公开的函数：`static`修饰，使得该函数只能在所在的编译单元中被使用
  * 静态全局变量`static`修饰，使得它成为只能在所在的编译单元中被使用的全局变量
  * 普通全局变量的**声明**：`extern <type> <identifier>;`

* 标准头文件结构

  * 重复声明问题

    * 同一个编译单元里，同名的结构不能被重复声明
    * 如果你的头文件里有结构的声明，很难让这个头文件不会在一个编译单元里被`#include`多次
    * 所以需要**标准头文件结构**

  * 运用条件编译和宏，保证这个头文件在一个编译单元中只会被`#include`一次

    ```h
    #ifndef _LIST_H_
    #define _LIST_H_
    
    #include "node.h"
    
    typedef struct _list {
        Node* head;
        Node* tail;
    } List;
    
    #endif
    ```

  * `#progma once`也能起到相同的作用，但不是所有的编译器都支持

***

## 简论编译和链接

* 编译器（广义）

  * 预处理器

  * 编译器

  * 汇编器

  * 链接器

    ```mermaid
    graph LR;
    A((file.c))
    B((file.h))
    C(预处理器)
    A --> C
    B --> C
    D((file.i))
    E(编译器)
    C --> D
    D --> E
    F((file.s))
    G(汇编器)
    E --> F
    F --> G
    H((file.o))
    G --> H
    ```

* 预编译

  * 生成中间文件`.i`
  * 处理所有的注释，以空格代替
  * 将所有的`#define`删除，并且展开所有的宏定义
  * 处理条件编译指令`#if, #ifdef, #elif, #else, #endif`
  * 处理`#include`，展开被包含的文件
  * 保留编译器需要使用的`#pragma`指令
  * 预处理指令：`gcc -E file.c -o file.i`

* 编译

  * 对预处理文件进行词法分析，语法分析和语义分析
    * 词法分析：分析关键字，标识符，立即数等是否合法
    * 语法分析：分析表达式是否遵循语法规则
    * 语义分析：在语法分析的基础上进一步分析表达式是否合法
  * 分析结束后进行代码优化生成相应的汇编代码文件
  * 编译指令：`gcc -S file.c -o file.s`

* 汇编

  * 汇编器将汇编代码转变为机器的可执行指令
  * 生成机器代码目标文件`.o`
  * 汇编指令：`gcc -c file.s -o file.o`

* 链接

  * 链接器的主要作用是把各个模块之间相互引用的部分处理好，使得各个模块之间能够正确的衔接

  * 静态链接：链接器在链接时将库的内容直接加入到可执行程序中，执行时与原来的那些文件没有关系

    ```mermaid
    graph TB;
    A((file1.o))
    B((file2.o))
    C((libc.a))
    D(链接器)
    E((a.out))
    A --> D
    B --> D
    C --> D
    D --> E
    ```

    * `Linux`下静态库的创建和使用
      * 编译静态库源码：`gcc -c lib.c -o lib.o`
      * 生成静态库文件：`ar -q lib.a lib.o`
      * 使用静态库编译：`gcc main.c lib.a -o main.out`

  * 动态链接：可执行程序在运行时才动态加载库进行链接，库的内容不会进入可执行程序当中

    ```mermaid
    graph LR;
    A((lib1.so))
    B((lib2.so))
    C[stub1]
    D[stub2]
    E(链接器)
    F((a.out))
    A --- C
    B --- D
    C --> E
    D --> E
    E --> F
    ```

    * `Linux`下动态库的创建和使用
      * 编译动态库源码：`gcc -shared dlib.c -o dlib.so`
      * 使用动态库编译：`gcc main.c -lbl -o main.out`
      * 关键系统调用：
        * `dlopen`：打开动态库文件
        * `dlsym`：查找动态库中的函数并返回调用地址
        * `dlclose`：关闭动态库文件

* 条件编译

  * 条件编译是**预编译**指示命令，用于控制是否编译某段代码

    ```c
    #if (C == 1)
    /*
    #ifdef C
    #ifndef C
    */
    	// statements
    #else
    	//statements
    #endif
    ```

  * 实际工程中条件编译主要用于以下两种情况：

    * 不同产品线共用一份代码
    * 区分编译产品的调试版和发布版

* `#error`，`#warning`

  * 用于生成一个自定义的编译错误消息/警告

  * 用法：`#error message`，`message`不需要双引号

  * 是一个预编译器指示字

  * 可用于提示编译条件是否满足

    例：

    ```c
    #ifndef __cplusplus
    	#error This file should be processed with C++ compiler.
    #endif
    ```

  * 编译过程中的任意错误信息意味着无法生成最终的可执行程序

* `#pragma`

  * 用于指示编译器完成一些特定的动作
  * `#pragma`所定义的很多指示字是编译器特有的
  * 在不同的编译器间是不可移植的
    * 预处理器将忽略它不认识的`#pragma`指令
    * 不同的编译器可能以不同的方式解释同一条`#pragma`指令

  * 用法：`#pragma parameter`
    * `#pragma message`
      * `message`参数在大多数的编译器中都有相似的实现
      * 在编译时输出消息到编译输出窗口中
      * 可以用于条件编译中提示代码的版本信息


***

## 输入输出

* 格式化输入输出

  * `printf`：`%[flag][width][.prec][hIL]type`

    | `Flag`    | 含义             |
    | --------- | ---------------- |
    | `-`       | 左对齐           |
    | `+`       | 在前面放`+`或`-` |
    | `(space)` | 正数留空         |
    | `0`       | 0填充            |

    | `width`, `prec` | 含义                     |
    | --------------- | ------------------------ |
    | `number`        | 最小字符数               |
    | `*`             | 指代参数是字符数         |
    | `.number`       | 小数点后的位数           |
    | `.*`            | 指代参数是小数点后的位数 |

    | `hIL` | 含义          |
    | ----- | ------------- |
    | `hh`  | 单个字节      |
    | `h`   | `short`       |
    | `l`   | `long`        |
    | `ll`  | `long long`   |
    | `L`   | `long double` |

    | `type`  | 用于               |
    | ------- | ------------------ |
    | `i / d` | int                |
    | `u / U` | unsigned int       |
    | `o / O` | 八进制             |
    | `x`     | 十六进制           |
    | `X`     | 字母大写的十六进制 |
    | `f / F` | float，6位         |
    | `e / E` | 指数               |
    | `g`     | float              |
    | `G`     | float              |
    | `a / A` | 十六进制浮点       |
    | `c`     | char               |
    | `s`     | 字符串             |
    | `p`     | 指针               |
    | `n`     | 读入/写出的个数    |

  * `scanf`：`%[flag]type`

    | `Flag`   | 含义           |
    | -------- | -------------- |
    | `*`      | 跳过           |
    | `number` | 读入最大字符数 |
    | `hh`     | `char`         |
    | `h`      | `short`        |
    | `l`      | `long, double` |
    | `ll`     | `long long`    |
    | `L`      | `long double`  |

    | `type`       | 用于                            |
    | ------------ | ------------------------------- |
    | `d`          | `int`                           |
    | `i`          | `int`，可以接受十六进制或八进制 |
    | `u`          | `unsigned int`                  |
    | `o`          | 八进制                          |
    | `x`          | 十六进制                        |
    | `a, e, f, g` | float                           |
    | `c`          | char                            |
    | `s`          | 字符串                          |
    | `[...]`      | 所允许的字符                    |
    | `p`          | 指针                            |

  * `printf`返回输出的字符数，`scanf`返回读入的项目数

* 文件输入输出

  * 程序运行重定向：`<`， `>`

  * `FILE`

    * 标准库里声明的类型

    * ```c
      FILE* fopen(const char* restrict path, const char* restrict mode);
      int fclose(FILE* stream);
      fscanf(FILE*, ...);
      fprintf(FILE*, ...);
      
      // 打开文件的标准代码
      FILE* fp = fopen("file", "r");
      if(fp) {
          fscanf(fp, ...);
          fclose(fp);
      }
      else {
          // ...
      }
      
      /*
      	r:		打开只读
      	r+:		打开读写，从文件头开始
      	w:		打开只写。如果不存在则新建，如果存在则清空
      	w+:		打开读写。如果不存在则新建，如果存在则清空
      	a:		打开追加。如果不存在则新建，如果存在则从文件尾开始
      	..x:	只新建，如果文件已存在则不能打开
      */
      ```

* 文本文件 vs 二进制文件

  * 本质上，所有文件都是二进制文件

  * 文本文件无非是用最简单的方式可以读写的文件

  * 二进制文件是需要专门的程序来读写的文件

  * 文本文件的输入输出是格式化，可能经过转码

  * `Unix`喜欢用文本文件来做数据存储和程序配置；`Windows`喜欢用二进制文件

  * 文本的优点是方便人类读写，而且跨平台；缺点是输入输出要经过格式化，开销大

    二进制的优点是程序读写快，缺点是人类读写困难，而且不跨平台

  * 程序为什么要文件

    * 配置：`Unix`用文本，`Windows`用注册表
    * 数据：稍微有点量的数据都放数据库了
    * 媒体：只能是二进制

  * 现实是，程序通过第三方库来读写文件，很少直接读写二进制文件做底层操作

***

## 程序的基本数据区

* 栈
  * 用于维护函数调用上下文
    * 保存了参数，返回地址，`old ebp`，寄存器信息，局部变量，其他数据信息
  * 后进先出，栈底栈顶
  * 每次函数调用都对应着栈上的一个活动记录
    * 调用函数的活动记录位于栈的中部
    * 被调函数的活动记录位于栈的顶部
  * 函数调用时，对应的栈空间在函数返回前是专用的
  * 函数调用结束后，栈空间将被释放，数据不再有效；在调用下一个函数前数据仍然存在
* 堆
  * 堆是程序中一块预留的内存空间，可由程序自由使用
  * 堆中被程序申请的内存在被主动释放前将一直有效
  * 为什么有了栈还需要堆？
    * 栈上的数据在函数返回后就会被释放掉，无法传递到函数外部
  * 系统对堆空间的管理方式：空闲链表法，位图法，对象池法等
* 静态存储区
  * 随着程序的运行而分配空间
  * 生命周期直到程序运行结束
  * 在程序的编译期静态存储区的大小就已经确定
  * 主要用于保存全局变量和静态局部变量
  * 静态存储区的信息最终会保存到可执行程序中
* 程序的内存布局
  * 可执行文件的布局：`File Header | .text | .data | .bss`
  * 映射到进程的地址空间：`stack | heap | .bss | .data | .text | 未映射区域`
  * 堆栈段在程序运行后才正式存在
  * `.bss`段存放的是未初始化的全局变量和静态变量
  * `.text`段存放的是程序中的可执行代码
  * `.data`段保存的是已经初始化了的全局变量和静态变量
  * `.rodata`段存放程序中的常量值，如字符串常量
  * 静态存储区通常指程序中的`.bss`和`.data`段
  * 只读存储区通常指程序中的`.rodata`段
