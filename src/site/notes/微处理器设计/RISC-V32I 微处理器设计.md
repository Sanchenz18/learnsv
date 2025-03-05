---
{"dg-publish":true,"permalink":"/微处理器设计/RISC-V32I 微处理器设计/"}
---

## 1. **任务描述**

本次课程设计要求设计一款32位单周期（或流水线）RISC-V CPU，至少支持12条指令，指令类型覆盖R型、I型、S型、B型、U型以及J型（指令格式见图1），要求完成上述CPU的可综合电路设计、应用程序开发及FPGA下载验证。

### RISC-V指令集指令格式定义

![alt text](/img/user/微处理器设计/imgs/image.png)

### 指令集
本次课程设计所设计的32位RISC-V CPU所支持的指令如下：
![Pasted image 20250305223105.png](/img/user/%E5%BE%AE%E5%A4%84%E7%90%86%E5%99%A8%E8%AE%BE%E8%AE%A1/imgs/Pasted%20image%2020250305223105.png)

### **具体含义**

| and rd, rs1, rs2   | rs1，rs2按位与，结果存储在rd                      |
| ------------------ | --------------------------------------- |
| or rd, rs1, rs2    | rs1，rs2按位或，结果存储在rd                      |
| xor rd, rs1, rs2   | rs1，rs2按位异或，结果存储在rd                     |
| andi rd, rs1, imm  | rs1，imm按位与，结果存储在rd                      |
| ori rd, rs1, imm   | rs1，imm按位或，结果存储在rd                      |
| xori rd, rs1, imm  | rs1，imm按位异或，结果存储在rd                     |
| sll rd, rs1, rs2   | 将rs1逻辑左移，左移位数为rs2，结果存储rd                |
| srl rd, rs1, rs2   | 将rs1逻辑右移，右移位数为rs2，结果存储rd                |
| sra rd, rs1, rs2   | 将rs1算术右移，右移位数为rs2，结果存储rd                |
| slli rd, rs1, imm  | 将rs1逻辑左移，左移位数为imm，结果存储rd                |
| srli rd, rs1, imm  | 将rs1逻辑左移，右移位数为imm，结果存储rd                |
| srai rd, rs1, imm  | 将rs1算术左移，右移位数为imm，结果存储rd                |
| add rd, rs1, rs2   | rd = rs1 + rs2                          |
| sub rd, rs1, rs2   | rd = rs1 - rs2                          |
| addi rd, rs1, rs2  | rd = rs1 + imm                          |
| mul rd, rs1, rs2   | rd = rs1 \* rs2                         |
| lw rd, imm(rs1)    | 从内存地址中加载一个32位有符号或无符号字到寄存器rd，地址为rs1+imm  |
| lh rd, imm(rs1)    | 从内存地址中加载一个16位有符号半字到寄存器rd，地址为rs1+imm     |
| lhu rd, imm(rs1)   | 从内存地址中加载一个16位无符号半字节到寄存器rd，地址为rs1+imm    |
| lb rd, imm(rs1)    | 从内存地址中加载一个8位有符号字节到寄存器rd，地址为rs1+imm      |
| lbu rd, imm(rs2)   | 从内存地址中加载一个8位无符号字节到寄存器rd，地址为rs1+imm      |
| sw rd, imm(rs2)    | 将rd中的32位值存储到（rs2+imm）地址的内存中             |
| sh rd, imm(rs2)    | 将rd中的最低16位值存储到（rs2+imm）地址的内存中           |
| sb rd, imm(rs2)    | 将rd中的最低8位值存储到（rs2+imm）地址的内存中            |
| beq rs1, rs2, imm  | 如果rs1等于rs2，跳转到pc+imm                    |
| bne rs1, rs2, imm  | 如果rs1不等于rs2，跳转到pc+imm                   |
| blt rs1, rs2, imm  | rs1小于rs2（有符号），跳转到pc+imm                 |
| bltu rs1, rs2, imm | rs1小于rs2（无符号），跳转到pc+imm                 |
| bge rs1, rs2, imm  | rs1大于等于rs2（有符号），跳转到pc+imm               |
| bgeu rs1, rs2, imm | rs1大于等于rs2（无符号），跳转到pc+imm               |
| jal rd, imm        | 首先将PC+4的地址值存储到返回寄存器rd，然后跳转到pc+imm标签处    |
| jarl rd, rs1, imm  | 首先将PC+4的地址值存储到返回寄存器rd，然后跳转到imm+rs1得到的地址 |
| slt rd, rs1, rs2   | 如果rs1值小于rs2（有符号），则rd置1，否则置0             |
| slti rd, rs1, imm  | 如果rs1值小于立即数imm（符号扩展后），则rd置1，否则置0        |
| sltu rd, rs1, rs2  | 如果rs1值小于rs2（无符号），则rd置1，否则置0             |
| sltui rd, rs1, imm | 如果rs1值小于立即数imm（无符号），则rd置1，否则置0          |

## 2. **硬件电路总体设计方案**

processor\_pipelined\_core顶层模块图

![alt text](/img/user/微处理器设计/imgs/image-1.png)

RISC-V CPU的总体结构图如图所示：

   其中在基本的单周期处理器数据通路基础上添加了四个触发器用以寄存五级流水线每级的控制信号及数据，添加指令预测（branch\_pre）、数据前递（forwarding）两模块作为五级流水线处理器特有的模块，分别提供控制冒险、数据冒险的解决方案

表 RISC-V CPU顶层模块输入输出端口定义

| 序号 | 端口名称 | 端口位宽 | 输入\输出类型 | 端口说明 |
| --- | --- | --- | --- | --- |
| 1 | clk | 1bit | input | CPU时钟输入 |
| 2 | rst\_n | 1bit | input | CPU复位输入 |
| 3 | me\_alu\_result | 32bits | output | 访存阶段alu结果 |
| 4 | me\_mem\_write | 1bit | output | 访存阶段ram写信号 |
| 5 | me\_mem\_read | 1bit | output | 访存阶段ram读信号 |
| 6 | me\_mem\_ctrl | 3bits | output | 访存阶段ram控制信号 |
| 7 | me\_mem\_in | 32bits | output | 访存阶段ram输入数据 |
| 8 | if\_inst\_addr | 32bits | output | 取指阶段指令地址 |
| 9 | GPIOA | 8bits | inout | 通用输入输出端口 |

## 3. **硬件电路详细设计方案**

**3.1指令存储器（Inst\_Mem）模块设计**

**3.1.1 Inst\_Mem功能描述**

在时钟上升沿来临时根据地址输出指令

给出reg定义的instruction memory读出时序图

![alt text](/img/user/微处理器设计/imgs/image-2.png)

**3.1.2 Inst\_Mem端口定义**

表2 指令存储器模块输入\输出端口定义

| 序号 | 端口名称 | 端口位宽 | 输入\输出类型 | 端口说明 |
| --- | --- | --- | --- | --- |
| 1 | clk | 1bit | input | 指令存储器时钟 |
| 2 | inst\_addr | 32bits | input | 指令存储器访存地址 |
| 3 | data | 32bits | output | 指令存储器指令输出 |

**3.1.3 Inst\_Mem详细设计方案**

本设计使用条件编译，有reg定义存储器与block memory ip存储器可选择，存储器宽度都为32bits深度为6000。

可使用distributed memory ip，设计深度为6000，适用于小规模应用程序处理

**3.1.4 Inst\_Mem模块级仿真结果**

![alt text](/img/user/微处理器设计/imgs/image-3.png)

**3.2数据存储器模块（Data\_Mem）设计**

**3.2.1 Data\_Mem功能描述**

与寄存器的数据进行互通。将寄存器中的数据写入数据存储器进行存储，或将数据存储器的数据读出，加载到寄存器中

给出data memory存储器读写时序

![alt text](/img/user/微处理器设计/imgs/image-4.png)

**3.2.2 Data\_Mem端口定义**

| 序号 | 端口名称 | 端口位宽 | 输入\输出类型 | 端口说明 |
| --- | --- | --- | --- | --- |
| 1 | clk | 1bit | input | 存储器时钟 |
| 2 | address | 13bits | input | 读写地址 |
| 3 | we | 1bit | input | 写使能 |
| 4 | oe | 1bit | input | 读使能 |
| 5 | mem\_ctrl | 3bit | input | 存储行为控制信号 |
| 6 | data\_in | 32bits | input | 数据输入 |
| 7 | data\_out | 32bits | output | 存储器输出 |
| 8 | vga\_out | 4bits | output | 数字识别结果 |

**3.2.3 Data\_Mem详细设计方案**

data\_memory使用distributed memory ip，设计深度为6000，适用于小规模应用程序处理

**3.2.4 Data\_Mem模块级仿真结果**

![alt text](/img/user/微处理器设计/imgs/image-5.png)

通过仿真图可以看出，本模块设计考虑到了非全字的load与store，全部功能正常

**3.3寄存器堆模块（Regfiles）设计**

**3.3.1 Regfiles功能描述**

使用reg定义32个32位寄存器，能够在时钟上升沿来临时输出地址ra、rb的数据busa、busb，并在下降沿来临时写入地址rd数据busw。

**3.3.2 Regfiles端口定义**

| 序号 | 端口名称 | 端口位宽 | 输入\输出类型 | 端口说明 |
| --- | --- | --- | --- | --- |
| 1 | clk | 1bit | input | regfile时钟 |
| 2 | rst\_n | 1bit | input | regfile复位 |
| 3 | ra | 5bits | input | rs1写地址 |
| 4 | rb | 5bits | input | rs2写地址 |
| 5 | rw | 5bits | input | rd读地址 |
| 6 | wen | 1bit | input | 读使能 |
| 7 | busw | 32bits | input | rd写入数据 |
| 8 | busa | 32bits | output | rs1输出数据 |
| 9 | busb | 32bits | output | rs2输出数据 |

**3.3.3 Regfiles详细设计方案**

使用复位初始化每个寄存器为0，并使x0为常零，每个寄存器的写使能为当寄存器文件写wen信号为高时与写地址相与，得到具体每位寄存器的写使能。

给出regfile读写时序

![alt text](/img/user/微处理器设计/imgs/image-6.png)

**3.3.4 Regfiles模块级仿真结果**

![alt text](/img/user/微处理器设计/imgs/image-7.png)

通过仿真图可以看出，本模块实现了预期的全部功能，上升沿读取与下降沿写入

**3.4主控制模块（Control）设计**

**3.4.1 Control功能描述**

时钟上升沿来临时，根据输入的指令译出各模块所需的控制信号

**3.4.2 Control端口定义**

| 序号 | 端口名称 | 端口位宽 | 输入\输出类型 | 端口说明 |
| --- | --- | --- | --- | --- |
| 1 | op | 7bits | input | 指令opcode |
| 2 | func3 | 3bits | input | 指令func3 |
| 3 | func7 | 7bits | input | 指令func7 |
| 4 | mem\_write | 1bit | output | memory写使能信号 |
| 5 | alu\_src | 1bit | output | alu rs2来源 |
| 6 | reg\_write | 1bit | output | regfile写使能 |
| 7 | mem\_reg | 1bit | output | 写回数据选择信号 |
| 8 | mem\_read | 1bit | output | memory读使能 |
| 9 | pc\_branch | 1bit | output | j型指令跳转信号 |
| 10 | pc\_src | 1bit | output | 指令地址计算数选择 |
| 11 | alu\_op | 4bits | output | alu行为控制 |
| 12 | mem\_ctrl | 3bits | output | memory行为控制 |
| 13 | pc\_src\_j\_n | 1bit | output | 写回选择linkpc |

**3.4.3 Control详细设计方案**

使用七位opcode、三位function3、七位function7直接译出各个模块的控制信号，其中alu使用两级译码，控制器内有两位alu\_ctrl，用以做一级译码，再继续根据function3、function7做二级译码，最终输出alu\_op

其余信号使用一级译码可以直接输出

控制信号真值表如下：

| instr\_type | Branch | MemRead | MemtoReg | MemWrite | ALUSrc | RegWrite |
| --- | --- | --- | --- | --- | --- | --- |
| U\_type | 0 | 0 | 0 | 0 | 1 | 1 |
| R\_type | 0 | 0 | 0 | 0 | 1 | 1 |
| load | 0 | 1 | 1 | 0 | 1 | 1 |
| I\_type | 0 | 0 | 0 | 0 | 0 | 1 |
| B\_type | 1 | 0 | 0 | 0 | 1 | 0 |
| jal | 1 | 0 | 0 | 0 | 0 | 1 |
| jalr | 0 | 0 | 0 | 0 | 0 | 1 |
| S\_type | 0 | 0 | 0 | 1 | 1 | 0 |

**3.4.4 Control模块级仿真结果**

![alt text](/img/user/微处理器设计/imgs/image-8.png)

通过仿真图可以看出，设计符合预期，模块为组合逻辑电路，则每个指令来临时都可以输出各个模块的控制信号

**3.5** ALU模块**设计**

**3.5.1** ALU**功能描述**

Arithmetic and Logic Unit 算术逻辑单元，是处理器中负责运算的组件，能够进行算出运算和逻辑运算

**3.5.2** ALU**端口定义**

| 序号 | 端口名称 | 端口位宽 | 输入\输出类型 | 端口说明 |
| --- | --- | --- | --- | --- |
| 1 | alu\_src0 | 32bits | input | rs1寄存器源操作数 |
| 2 | alu\_src1 | 32bits | input | rs2寄存器源操作数 |
| 3 | alu\_op | 4bits | input | ALU运算模式控制信号 |
| 4 | zero | 1bit | output | 计算结果为0信号 |
| 5 | alu\_result | 32bits | output | ALU计算结果 |

**3.5.3** ALU**详细设计方案**

模块设计时使用条件编译，设计有使用符号及例化运算器的两种运算方式。例化32位加法器、乘法器和移位器，通过alu\_op信号对运算方式进行选择，对两个源操作数进行运算，将运算的结果从alu\_result输出。若加法器计算结果为0，则zero信号拉高。

控制信号与运算类型间的对应关系如下表：

| **alu\_ctrl** | **运算类型** | **对应指令** |
| --- | --- | --- |
| 0100 | add | add/addi/jal/jalr/load/store |
| 0101 | sub | sub/B\_type/slt/slti/sltiu |
| 1000 | xor | xor |
| 0111 | or | or/ori |
| 0110 | and | and/andi |
| 0001 | sll | sll/slli |
| 0010 | srl | srl/srli |
| 0011 | sra | sra/srai |
| 1001 | slt | sltu/slt/sltiu |
| 1100 | mul | mul |
| 1011 | lui | lui |

**3.5.4 ALU模块级仿真结果**

![alt text](/img/user/微处理器设计/imgs/image-9.png)

仿真图可以看出，在source有输入时，通过计算就可以得出各个结果，然后根据alu\_op就可以选择出所需要的结果输出

**3.6**立即数产生模块（Imm\_gen）设计

**3.6.1** Imm\_ext**功能描述**

对指令中给出的立即数进行扩展，从而可以送入各模块中进行计算

**3.6.2** Imm\_ext**端口定义**

| 序号 | 端口名称 | 端口位宽 | 输入\输出类型 | 端口说明 |
| --- | --- | --- | --- | --- |
| 1 | inst | 32bits | input | 32位指令 |
| 2 | imm\_o | 32bits | output | 扩展后的32位立即数 |

**3.6.3** Imm\_ext**详细设计方案**

通过指令不同的opcode区分出不同型态的指令，直接根据指令译码得到符号位扩展后的立即数

**3.6.4 Imm\_ext模块级仿真结果**

![alt text](/img/user/微处理器设计/imgs/image-10.png)

通过仿真图可以看出，本模块可以实现符号扩展的立即数生成

**3.7**外围电路设计

**3.7.1 功能描述**

本设计具有交互性外设，有拨码开关四位，VGA显示输出。能够控制当前输入数字以及显示最终识别的结果

**3.7.2 详细设计方案**

由于提前将需要识别的数字存于片上的block memory，则使用四位拨码开关进行选择输入处理器进行运算

在得到识别结果之后，将需要识别的数据与识别成功的数据传至VGA，最终同时在显示器上显示

## 4. **验证方案及总体仿真结果**

**4.1仿真验证方案**

仿真验证方案设计思路：

编写汇编语言测试程序，在Ripes上运行，然后将指令初始化在处理器的指令存储器中进行仿真，与Ripes中的结果进行比对。用此种方法验证，必须保证测试程序将每种指令的每种情况都涵盖，尽可能验证处理器的全部功能正确。

验证程序：

```
addi x31 zero -8 #将x30和x31写入了两个负值，
srai x30 x31 1 #用于后续涉及到符号数、无符号数的指令的验证
addi x1 zero 1 #验证一部分alu运算指令
slli x2 x1 1
ori x3 zero 3
xori x4 zero 4
addi x5 zero 5
add x6 x1 x5
add x7 x2 x5
sll x8 x4 x1
xor x9 x8 x1
add x10 x8 x2
or x11 x8 x3
addi x12 zero 12
addi x20 zero 10 #验证分支跳转指令，#出现错误会使x1寄存器置0
beq x10 x20 8
addi x1 zero 0
addi x13 zero 13
bne x1 x2 8
addi x1 zero 0
addi x14 zero 14
blt x1 x2 8
addi x1 zero 0
addi x15 zero 15
bge x4 x3 8
addi x1 zero 0
addi x16 zero 16
bltu x31 x30 8
addi x1 zero 0
addi x17 zero 17
bgeu x30 x31 8
addi x1 zero 0
addi x18 zero 18
add x20 x18 x1
sw x20 0(x16)
lw x19 0(x16)
andi x4 x5 4        #验证一部分alu指令，出现错误会覆盖正确结果
srli x2 x4 1
srai x1 x2 1
sub x3 x5 x2
srl x4 x8 x1
sra x3 x6 x1
slt x20 x30 x31     #验证置位指令，正数与正数，正数与负数，负数与负数情况均包含
slt x21 x31 x30
slt x22 x2 x1
slt x23 x1 x2
slt x24 x1 x31
slt x25 x31 x1
sltu x26 x31 x1
sltu x27 x1 x31
sltiu x28 x31 4
slti x29 x5 6
jal x20 8            #test 验证无条件跳转指令
addi x1 zero 0
jalr x20 x8 0xd8
addi x1 zero 0
lui x31 0x12345      #验证lui指令
addi x31 x31 0x678
addi x20 zero 0x10   #验证三种store指令
sb x31 0(x20)
addi x20 x20 0x4
sh x31 0(x20)
addi x20 x20 0x4
sw x31 0(x20)
lb x22 0(x20)        #验证三种load指令
lh x24 0(x20)
lw x26 0(x20)
```

说明：

  x30和x31写入了两个负值，用于后续涉及到符号数、无符号数的指令的验证。x1-x12验证了ALU的大部分运算，包括加法、移位、逻辑运算和带立即数的运算；x13-x18验证了B—type的指令跳转是否正确，后续又验证了ALU的另外一部分指令，如果出现错误会使之前写入到寄存器文件的值发生改变。x20-x29验证了slt、sltu、sltiu这几种写回为0或1的ALU运算，包括正数和正数、负数和负数、正数和负数全部情况的比较。最后用x20、x22、x24、x26寄存器验证了无条件跳转指令和存储器的各类读写指令。

若处理器的功能正确，结果应为：

1. x1-x19寄存器中的值与寄存器号相同；
2. x20为中间寄存器；
3. 当验证到slt一类的指令时，x20-x29寄存器中，寄存器号为奇数的寄存器中的值为1，寄存器号为偶数的寄存器中的值为0；
4. x22、x24、x26寄存器的值分别被78、5678、12345678覆盖
5. 整个过程中x1-x9的值始终不被覆盖
6. 若x1寄存器的值为0，则说明该位置的分支跳转指令出错

**4.2仿真验证结果**

![alt text](/img/user/微处理器设计/imgs/image-11.png)

结合验证方案对仿真结果进行分析：

![alt text](/img/user/微处理器设计/imgs/image-12.png)

  在仿真结果中可以看到，寄存器x1-x12的值依次为寄存器号，x20作为中间寄存器，值一直在改变并且与Ripes中的结果一致，在x20-x29中，奇数号寄存器的值为1，偶数号寄存器的值为0，最后x22、x24、x26寄存器的值分别被78、5678、12345678覆盖，由仿真结果可以看出寄存器和存储器对应位置的值均正确，验证了load指令和store指令。

## 5.**应用程序设计及FPGA开发板验证**

**5.1 应用程序设计**

本次处理器设计的应用程序为基于LeNet-5的手写数字识别，因为LeNet-5的权重参数较大，我们对改网络的架构进行了简化，网络架构如下图

![alt text](/img/user/微处理器设计/imgs/image-13.png)

图片数据大小为灰度值8bit，权重量化同样为8位，权重参数共780B ，可以存储到片上存储中直接访问。

本设计首先使用高级语言对整个计算过程进行构建，然后使用risc-v toolchain将高级语言汇编为机器码。

C语言中部分核心代码如下：

***C
// C1卷积层

```c
for(int n=0; n<3; n++)//3层
{
	for(int row=0; row<=23; row++)//中层循环的上限为24，以确保在28x28的输入图像上滑动一个5x5的窗口时不越界。
	{
		for(int col=0; col<=23; col++)
		{
			conv\_temp\_c1 = 0;
			for(int x=0; x<5; x++)
			{
				for(int y=0; y<5; y++)
				{
					conv_temp_c1 += img_data[(row+x)*28 + (col+y)] * cnn_param_w_c1[x*5+y+n*25];
				}
			}
			conv_temp_c1 = conv_temp_c1 >> 8;
			conv_temp_c1 += cnn_param_b_c1[n];//卷积核的偏置
			conv_rlst_c1[row*24+col+n*24*24] = conv_temp_c1;
		}
	}
}
```

使用编译器将C语言汇编为机器码，写入instruction memory ip的coe中，在Ripes中查看编译后的RISC-V汇编语言。

![alt text](/img/user/微处理器设计/imgs/image-14.png)

**5.2 FPGA下载验证**

![alt text](/img/user/微处理器设计/imgs/image-15.png)

至此，本设计完成从设计到验证到原型机搭建验证。