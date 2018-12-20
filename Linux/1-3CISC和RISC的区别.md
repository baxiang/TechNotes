CISC：
* Complex Instruction-set computer 复杂指令集CPU
* CISC体系的设计理念是用最少的指令（注意说的是指令而不是指令集）来完成任务（譬如计算乘法只需要一条MUL指令即可），因此CISC的CPU本身设计复杂、工艺复杂，但好处是编程器好设计。CISC出现较早，至今Intel还一直采用CISC设计。

RISC：
* Reduced Instruction-set Computer 精简指令集CPU
* RISC的设计理念是让软件来完成具体的任务，CPU本身只提供基本功能指令集。因为RISC CPU 的指令集只有很少的指令，这种设计相对于CISC，CPU的设计和工艺简单了，但是编译器的设计变难了。

CPU的设计方式发展：
* 早期简单CPU，指令和功能都很有限。
* CISC年代-------CPU功能拓展依赖于指令集的拓展，实质是CPU内部组合逻辑电路的拓展。
* RISC年代-------CPU仅仅提供基础功能指令（譬如内存与寄存器通信指令，基本运算与判断指令符等），功能拓展由使用CPU的人利用基础架构来灵活实现。

RISC与CISC指令集对比：
* 一般典型的CISC CPU指令在300左右。
* ARM CPU常用指令在30条左右。

发展趋势：
* 没有纯粹的RISC或CISC，发展方向是RISC与CISC结合，形成一种介于两者之间的CPU类型。
