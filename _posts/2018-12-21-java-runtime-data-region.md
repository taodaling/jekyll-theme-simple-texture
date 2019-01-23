---
categories: java
layout: post
---

- Table
{:toc}
# 运行时数据区域

Java虚拟机在执行java程序的过程中会把它所管理的内存划分为若干个不同的数据区域，每个区域都有各自的用途。分为：

- 方法区
- 堆
- 虚拟机栈
- 本地方法栈
- 程序计数器

## 程序计数器

程序计数器是一块较小的内存空间，它可以看作是当前线程所指向的字节码的行号记录器。字节码解释器通过改变程序计数器的值来做指令跳转。

由于多线程下，一个处理器可以执行一条线程中的指令，因此为了线程切换后能恢复到正确的执行位置，每条线程都需要有一个独立的程序计数器。

如果线程执行的是一个Java指令，这个计数器记录的是该虚拟机字节码的地址，如果执行的是Native方法，则计数器留空(Undefined)。

## 虚拟机栈

Java虚拟机栈也是线程私有的，它的生命周期与线程相同。每个方法执行的时候都会创建一个栈帧（stack frame），用于存储局部变量，操作数等信息。方法的调用和返回对应入栈和出栈的过程。

如果线程的栈深度超过了虚拟机允许的最大深度，将抛出`StackOverflowError`异常。如果虚拟机栈可以动态扩展，且扩展时无法申请到足够的内存，则会抛出`OutOfMemoryError`异常。

## 堆

堆时Java虚拟机所管理的内存中最大的一块。堆时被所有线程共享的一块内存区域，在虚拟机启动时创建。堆的唯一用处就是存放对象实例。

堆也是GC的主要发生区域，因此也称为GC堆。从垃圾回收的角度看，现在收集器都采用分代收集算法，所以堆还可以细分为：新生代和老年代。更细致第可以分为`Eden`，`From Survivor`，`To Survivor`。

如果堆中没有足够内存完成实例创建，并且堆也无法继续扩展时，将会抛出`OutOfMemoryError`异常。

## 方法区

方法区与堆一样，被所有线程所共享。它用于存储已经被虚拟机加载的类信息，常量，静态变量，JIT编译后的代码等数据。

在HotSpot虚拟机上，方法区被称为永久代（Permanet Generation）。本质上两者并不等价，仅仅时因为HotSpot虚拟机选择将GC分代扩展至方法区，这样GC就可以像管理堆一样管理这部分内存。

方法区可以选择不实现垃圾收集。一般来说，GC很少发生在方法区，但不绝对，这块区域的GC目标主要是回收常量池和卸载类型。

当方法区无法满足内存分配需求时，将抛出`OutOfMemoryError`异常。
运行时常量池时方法区的一部分。Class文件中存储着常量池，保存编译期生成的各种字面量和符号引用，常量池将保存这部分内容。运行期间也可以将新的常量放入运行时常量池，比如使用`String.intern()`。

## 直接内存

直接内存不是虚拟机运行时数据区的一部分，也没有在Java虚拟机规范中定义。

在JDK1.4中新加入了NIO类，引入了一种基于通道与缓冲区的IO方式，它可以使用Native函数库直接分配堆外内存，然后通过一个存储在堆外的DirectByteBuffer对象作为这块内存的引用进行操作。这样能在一些常见中显著提高性能，因为避免了堆和Native内存之间来回复制数据。

本机的直接内存的分配与Java堆的大小限制无关，但是作为内存，还是会受到本机总内存大小以及处理器寻址空间的限制。