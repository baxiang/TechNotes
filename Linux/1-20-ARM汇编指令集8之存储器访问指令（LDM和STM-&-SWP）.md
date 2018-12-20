[ARM汇编指令集之存储器访问指令（LDM和STM & SWP）]
*** LDM & STM**

**批量加载/存储指令可以实现在一组寄存器和一块连续的内存单元之间传输数据。LDM为加载多个寄存器，STM为存储多个寄存器，允许一条指令传送16个寄存器的任意子集或所有寄存器。**

**指令格式：**

**LDM{cond}<模式> Rn{!}，reglist{^}**

**STM{cond}<模式> Rn{!},reglist{^}**

**LDM/STM的主要用途是现场保护、数据复制、参数传送等。其模式有8种，如下：（前面四种用于数据块的传输，后面四种是堆栈操作）**

**① IA：每次传送后地址加4**

**② IB：每次传送前地址加4**

**③ DA：每次传送后地址减4**

**④ DB：每次传送前地址减4**

**⑤ FD：满递减堆栈**

**⑥ ED：空递减堆栈**

**⑦ FA：满递增堆栈**

**⑧ EA：空递增堆栈**

**其中，寄存器Rn为基址寄存器，装有传送数据的初始地址，Rn不允许为R15；后缀“!”表示最后的地址写回到Rn中；寄存器列表reglist可包含多于一个寄存器或寄存器范围，使用“，”分开，如{R1,R2,R6-R9}，寄存器排列由小到大排列；“^”后缀不允许在用户模式和系统模式下使用，如果LDM指令寄存器列表中包含有PC时使用，那么除了正常的多寄存器传送外，还将SPSR拷贝到CPSR中，这可用于异常处理返回，如果LDM指令寄存器列表中不包含PC时，加载/存储的是用户模式下的寄存器，而不是当前模式下的寄存器。**

**地址对准问题：这些地址忽略地址的[bit1,bit0]。**

**eg:**

**LDMIA R0!,{R3-R9}  ; 加载R0指向的地址上的多字数据，保存到R3-R9中，R0值更新**

**STMIA R1!,{R3-R9}  ; 将R3-R9的数据存储到R1指向的地址上，R1值更新**

**STMFD SP!,{R0-R7,LR}  ; 现场保存，将R0-R7、LR入栈**

**LDMFD SP!,{R0-R7,PC}^   ; 恢复现场，异常处理返回**

**在进行数据复制时，先设置好源数据指针，然后使用块拷贝指令 LDMIA/STMIA  LDMIB/STMIB  LDMDA/STMDA  LDMDB/STMDB 进行读取和存取。而进行堆栈操作时，则要先设置堆栈指针，一般使用SP然后使用堆栈寻址指令 STMFD/LDMFD  STMED/LDMED  STMFA/LDMFA  STMEA/LDMEA  实现堆栈操作。**

**eg:**

**下面所有图中R1为指令执行前的基址寄存器，R1'为指令执行后的基址寄存器**

**1）指令 STMIA R1!,{R5-R7}**

**![image](http://upload-images.jianshu.io/upload_images/143845-a7a7eaf8c1a1cfe7?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)**

**2）指令 STMIB R1!,{R5-R7}**

![image](http://upload-images.jianshu.io/upload_images/143845-a664961b475e2446?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**3）指令 STMDA R1!,{R5-R7}**

![image](http://upload-images.jianshu.io/upload_images/143845-fb7fe181e1724694?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**4）指令 STMDB R1!,{R5-R7}**

![image](http://upload-images.jianshu.io/upload_images/143845-0b0fd34a6ce37daf?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Q：数据是存储在基址寄存器的地址之上还是之下？地址是在存储第一个值之前还是之后？增加还是减少？

**A：看下面这个图**

![image](http://upload-images.jianshu.io/upload_images/143845-a8e9db8b4a4a26dd?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**上图中第二行的增加应改为减少。 总的来说地址的方向分为向上生长和向下生长。然后普通内存数据处理和堆栈的数据处理其实都有相应的对照关系，具体可以通过上图就可以看出。**

*** SWP**

**寄存器和存储器交换指令，SWP指令用于将一个内存单元（该单元地址放在寄存器Rn中）的内容读取到一个寄存器Rd中，同时将另一个寄存器Rm的内容写入到该内存单元中，使用SWP可以实现信号量操作。**

**指令格式:**

**SWP{cond}{B} Rd,Rm,[Rn]**

**其中，B为可选后缀。若有B，则交换字节，否则交换32位字；Rd为数据从存储器加载到的寄存器，Rm的数据用于存储到存储器中的，若Rm和Rd相同，则为寄存器和存储器的内容进行互换；Rn为要进行数据交换的存储器地址，Rn不能与Rd，Rm相同。**

**eg:**

**SWP R1,R1,[R0]     ; 将R1和R0指向的存储单元内容进行交换**

**SWPB R1,R2,[R0]   ; 将R0指向的存储单元内容读取一字节数据到R1中(高24位清零)，并将R2的内容写入到该内存单元中(最低字节有效)**
