# lab4 report

##[练习1]分配并初始化一个进程控制块
> 首先调用alloc_proc函数来通过kmalloc函数获得proc_struct结构的一块内存块-，作为第0个进程控制块。并把proc进行初步初始化（即把proc_struct中的各个成员变量清零）。但有些成员变量设置了特殊的值，proc->state=PROC_UNINIT、proc->pid=-1、proc->cr3 = boot_cr3
> struct context context含义：存储进程上下文结构体。struct trapframe *tf含义：指向中断帧结构体的指针。

##[练习2]为新创建的内核线程分配资源
> 具体实现：分配并初始化进程控制块（alloc_proc函数），分配并初始化内核栈（setup_stack函数），根据clone_flag标志复制或共享进程内存管理结构（copy_mm函数），设置进程在内核（将来也包括用户态）正常运行和调度所需的中断帧和执行上下文（copy_thread函数），把设置好的进程控制块放入hash_list和proc_list两个全局进程链表中，把进程状态设置为“就绪”态，设置返回码为子进程的id号。
> ucore给每个新fork的线程分配的id是一个唯一的id。get_pid函数分配PID是循环遍历proc_lish链表，找到未重复的PID。这个方法不是多线程安全的，所以get_pid()到list_add(&proc_list,&proc->list_link);的步骤之间是关中断的，来保证get_pid函数单线程运行，因此分配的PID是唯一的。

##[练习3]阅读代码，理解 proc_run 函数和它调用的函数如何完成进程切换的。
> 创建运行了2个内核线程。语句local_intr_save(intr_flag);....local_intr_restore(intr_flag)作用：暂时关闭中断：如果当前中断为开，则在local_intr_save关闭中断，待local_intr_restore再打开中断；如果当前中断为关，那么什么也不做。

##重要的知识点
> fork。含义：创建一个几乎一样的进程，各自拥有独立的内存空间。在父进程中返回子进程ID，在子进程中返回0，错误则返回负值。