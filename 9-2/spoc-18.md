##(spoc) 理解调度算法支撑框架的执行过程
###思考题

在default_sched.c中：

```
When one proc structure is selected, remember to update the stride property of the proc. (stride += BIG_STRIDE / priority) 
```

这里定义的lab6_poolstride 

```
 if (p->lab6_priority == 0)
          p->lab6_stride += BIG_STRIDE;
     else p->lab6_stride += BIG_STRIDE / p->lab6_priority;
```
这里使用了取模运算，`lab6_stride`值若大于`BIG_STRIDE`，可使得`p->lab6_stride`减去`q->lab6_stride`所得结果大于0时候取`lab6_stride`较小的值。所以这样仍然可以做出正确的调度决定。


###cprintf代码说明
修改了proc.c和default_sched.c两个文件，在其中增加了cprintf并用

```
--- output ---
```

形式输出调度过程。
make qemu的输出结果在result.txt文件