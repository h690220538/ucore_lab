# lab2 report

##[练习1]实现 first-fit 连续物理内存分配算法

> alloc的时候，找到一个可以容纳的块，如果有多余的空间，切开后将其property位置为1，并把它的link加到原来的link后面，再删去原来的link。free的时候，找到它的位置，在前面插入link。合并时，若前面或后面有相邻的块，则合并，把后面的块property置0，删除后面的link，并更新前面块的大小信息。

> 改进：可以适当建立索引，提高分配和释放的速度。

##[练习2]实现寻找虚拟地址对应的页表项

> 首先得到一级页表的对应项，检查其是否有效，如果无效则需要新建页面再返回二级页表项，否则直接返回。

请描述页目录项（Pag Director Entry）和页表（Page Table Entry）中每个组成部分的含义和以及对ucore而言的潜在用处。
> 页目录项中有对应的二级页表基地址，以及权限位：是否有效、是否可写、是否可被用户访问。
> 页表中有对应页的物理基地址，以及权限位：是否有效、是否可写。
> 用处：判断页面是否存在、管理内存、保护数据。

如果ucore执行过程中访问内存，出现了页访问异常，请问硬件要做哪些事情？
>访问硬盘，进行内存与硬盘间的数据访问。

##[练习3]释放某虚地址所在的页并取消对应二级页表项的映射

>判断二级页表是否有效，无效则直接退出，否则，将其指向的页的引用次数减1，如果到0则释放。然后把二级页表清零，刷新TLB。

数据结构Page的全局变量（其实是一个数组）的每一项与页表中的页目录项和页表项有无对应关系？如果有，其对应关系是啥
> 有，页表是从end开始的连续一段内存存放的。

如果希望虚拟地址与物理地址相等，则需要如何修改lab2，完成此事？
> 令gcc编译出的虚拟起始地址从0x100000开始即可。

##重要的知识点
> first-fit算法。含义：分配页时，大于所需大小的连续可分配页中，地址最小的被匹配。