以简单的C程序来较深入的理解一下C程序是如何从源代码到最后的可执行程序的（对于非计算机专业的同学理解C语言，以及计算机也有很好的帮助）

首先是大家在课本上都看过的，先从整体上来看一下（以一个简单的源程序hello.c为例）：

hello.c----预处理器---->hello.i----编译器---->hello.s----汇编器---->hello.o----链接器---->hello(可执行程序)
下面是实例代码：

```c
#include <stdip.h>

int main() {
	printf("hello\n");
	return 0;
}
```

我们用记事本来创建源文件：

![记事本输入](https://img-blog.csdnimg.cn/20200305231415613.png)

如果是windows系统的话，就是直接在记事本里面编写，保存时选择所有文件，并命名为hello.c（我这里是Linux系统）

这样我们就得到了一个用高级语言编写的源文件，也是大部分同学学习的重点

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200305231437231.png)

大家都知道源文件是易于我们理解的，接近自然语言，但是计算机并不认识它，所有要想运行程序必须将其转换成可执行的二进制文件，就是01序列

接下来就是预处理器的工作了，预处理器根将系统头文件插入程序文本中，也就是上面程序中的stdio.h，得到另一个C程序，通常是以.i为文件拓展名

stdio.h当中定义了一些运行程序所需要的一些常量和函数

之后编译器将文本文件hello.i翻译成文本文件hello.s，它包含一个汇编语言程序

通过在Linux终端运行命令，可以得到汇编语言程序（虽然操作系统给我们的感觉是鼠标的点击和一系列动作在完成运行程序，但是我们要知道这背后其实是一条条命令的执行）

linux> gcc -S hello.c

![img](https://img-blog.csdnimg.cn/20200305231457835.png)

在windows的命令行窗口可以执行同样的操作

这行命令中，gcc是调用编译器，大部分的时候我们用到的都是它，比如在codeblocks等集成的IDE中，都是与其相似的编译器

-S指明生成汇编程序

![img](https://img-blog.csdnimg.cn/20200305231517862.png)

我们得到了hello.s

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200305231537735.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDY5OTA2Mg==,size_16,color_FFFFFF,t_70)

这些看不懂的东西就是确确实实在发生的，要理解这个文件还需要更多的知识，这不是我们现在的重点

接下来就是汇编阶段，汇编器将hello.s翻译成及其语言指令，把这些指令打包成一种可重定位目标程序的格式，并将结果保存在目标文件hello.o中，一个二进制文件，如果用文本编辑器打开，我们会得到一堆乱码（如果在编写代码时，不小心把main()打成mian()就会，报错找不到xxx.o）

同样执行命令

linux> gcc -c hello.c

-c指明让gcc去编译并汇编该代码
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200305231614490.png)

如果要查看机器代码文件，我们需要反汇编器，这些程序根据机器代码产生一种类似于汇编代码的格式

linux> objdump -d hello.o

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200305231633173.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDY5OTA2Mg==,size_16,color_FFFFFF,t_70)

左边是以十六进制数表示的机器指令，右边是对应的汇编代码

那么经过汇编器得到的二进制文件是不是就是我们的可执行程序呢？

答案是我们还需要经过链接，因为库函数还没有被我们导入

hello程序调用了printf函数，它是每个C编译器都提供的标准C库中的一个函数，printf函数存在于一个名为printf.o的单独的预编译好的目标文件中，这个文件必须以某种方式合并到我们的hello.o程序中，合并后我们就得到了一个可执行目标文件，可以加载到内存中，由系统执行

linux> gcc -o hello hello.c

执行这个命令，编译器就会帮我们干完所有的事情，直接从源文件到可执行文件

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020030523165459.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDY5OTA2Mg==,size_16,color_FFFFFF,t_70)

图中的hello就是Linux系统下的一个可执行文件

终端执行命令：

linux> ./hello

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200305231714667.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDY5OTA2Mg==,size_16,color_FFFFFF,t_70)

可以看到程序执行了，输出了字符，然后终止。

好了从一个简单的文件我们了解了C程序的形成过程，希望比大家在课堂上听到的更加直观！