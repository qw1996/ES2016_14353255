# Lab3-DOL实例分析&编程

### 1.修改完成的.dot截图
#### example1:
![](http://oe4493xaz.bkt.clouddn.com/example1.png)
#### example2:
![](http://oe4493xaz.bkt.clouddn.com/example2.png)



### 2.修改过程
#### 任务1：
根据example2.xml中的代码：

    <variable value="2" name="N"/>
	<!-- instantiate resources -->
	<process name="generator">
    <port type="output" name="10"/>
    <source type="c" location="generator.c"/>
	</process>

	<iterator variable="i" range="N">
    <process name="square">
      <append function="i"/>
      <port type="input" name="0"/>
      <port type="output" name="1"/>
      <source type="c" location="square.c"/>
    </process>
	</iterator>

	<process name="consumer">
    <port type="input" name="100"/>
    <source type="c" location="consumer.c"/>
	</process>

	<iterator variable="i" range="N + 1">
    <sw_channel type="fifo" size="10" name="C2">
      <append function="i"/>
      <port type="input" name="0"/>
      <port type="output" name="1"/>
    </sw_channel>
	</iterator>
example2架构中，通过迭代，也就是iterator来控制迭代出N个square模块，也就是这里的`<iterator variable="i" range="N">`实际上相当于一个for循环，通过process产生了三个square模块，因此完成任务只需要把N的值改为2即可，也就是`<iterator variable="i" range="N">`


#### 任务2：
根据提示这里找到square.c文件：

	#include <stdio.h>
	#include "square.h"
	void square_init(DOLProcess *p) {
      p->local->index = 0;
      p->local->len = LENGTH;
	}

	int square_fire(DOLProcess *p) {
    float i;

    if (p->local->index < p->local->len) {
        DOL_read((void*)PORT_IN, &i, sizeof(float), p);
        i = i*i;
        DOL_write((void*)PORT_OUT, &i, sizeof(float), p);
        p->local->index++;
    }

    if (p->local->index >= p->local->len) {
        DOL_detach(p);
        return -1;
    }

    return 0;
	}

显然，square.c文件实际上是定义了square模块的具体操作,init函数是初始化不用管，任务重点在于square_fire函数，它在每次执行square模块时都会被执行，这段代码也不难理解，相当于一个for循环，判断generator那边生成的整数还没结束，就进行一个自乘操作，显然重点在于`i = i*i;`,毫无疑问这一步时在进行平方操作，任务也只需要将其再自乘一次即可输出3次方数，即`i = i*i*i;`


### 3.实验感想
 显然，这是试验任务并不难，甚至异常的简单，但我相信TA出这次实验肯定不会是就让我们改两行代码而已，而是希望我们能通过这两个任务来深入理解DOL编程实例，其代码是如何写的，如何运行的，xml里定义的模块与模块之间是如何连接的，就例如模块中的process好比执行任务的框框，sw_channel是连接这些独立框框形成复杂功能体的线，而connection则是把这些线的这头连到另一个框的那头的工具。

 因此，无论实验多么简单，这并不重要，重要的是能透过这些看透DOL编程的本源，学习之路任重而道远啊~