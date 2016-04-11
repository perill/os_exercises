# lab4 spoc 思考题

- 有"spoc"标记的题是要求拿清华学分的同学要在实体课上完成，并按时提交到学生对应的ucore_code和os_exercises的git repo上。

## 个人思考题

### 13.1 总体介绍

(1) ucore的线程控制块数据结构是什么？
>线程控制块TCB是由proc_struct这样一个数据结构维护。

### 13.2 关键数据结构

(2) 如何知道ucore的两个线程同在一个进程？
>比较两个线程的cr3和mm_struct，因为同一个进程共享一块内存空间和页表。

(3) context和trapframe分别在什么时候用到？
>trapframe在返回中断即执行iret时用于恢复现场，context在switch_to的ret时用到，返回到要切换的进程上下文context（一组寄存器的值）。

(4) 用户态或内核态下的中断处理有什么区别？在trapframe中有什么体现？
>用户态下中断处理涉及特权级切换(从用户态到内核态),内核态不涉及

### 13.3 执行流程

(5) do_fork中的内核线程执行的第一条指令是什么？它是如何过渡到内核线程对应的函数的？

```
tf.tf_eip = (uint32_t) kernel_thread_entry;
/kern-ucore/arch/i386/init/entry.S
/kern/process/entry.S
```

>pushl %edx这条指令将trapframe中的eip指向函数入口地址来完成跳转.

(6)内核线程的堆栈初始化在哪？
	
```
static int
setup_kstack(struct proc_struct *proc) {
    struct Page *page = alloc_pages(KSTACKPAGE);
    if (page != NULL) {
        proc->kstack = (uintptr_t)page2kva(page);
        return 0;
    }
    return -E_NO_MEM;
}
```

>tf和context中的esp

(7)fork()父子进程的返回值是不同的。这在源代码中的体现中哪？

```
ret = proc->pid  //父进程返回子进程pid
proc->tf->tf_regs.reg_eax = 0 //子进程返回0
```


