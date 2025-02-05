---
{"dg-publish":true,"permalink":"/SystemVerilog/SV_DataType/SystemVerilog_DataType/"}
---


## Data-type
| Data-type | 2-state/4-state | # Bits | signed/unsigned | C equivalent |
| :-------: | :-------------: | :----: | :-------------: | :----------: |
|    reg    |        4        |  >= 1  |    unsigned     |              |
|   wire    |        4        |  >= 1  |    unsigned     |              |
|  integer  |        4        |   32   |     signed      |              |
|   real    |                 |        |                 |    double    |
|   time    |                 |        |                 |              |
| realtime  |                 |        |                 |    double    |
|           |                 |        |                 |              |
|   logic   |        4        |  >=1   |    unsigned     |              |
|    bit    |        2        |  >= 1  |    unsigned     |              |
|   byte    |        2        |   8    |     signed      |     char     |
| shortint  |        2        |   16   |     signed      |  short int   |
|    int    |        2        |   32   |     signed      |     int      |
|  longint  |        2        |   64   |     signed      |   long int   |
| shortreal |                 |        |                 |    float     |

## 四值逻辑
### tri
- 与wire是相同的数据类型。
- 可以用于多个diver驱动一个net的情况。
- wire或者tri上来自多个相同强度源的逻辑冲突会导致不定态。
### integer
- 默认值是X。
 *==Usage：==* 
	`integer integer_data;`
### time
- 用于模拟时间测量的特殊数据类型。表示64位无符号整数
- 与$time系统函数结合保存使用来保存当前的仿真时间
 *==Usage：==* 
	`time variable_name;`
	`time_data = $time;`
### real
- 以64位实数实现。可以使用十进制或者科学计数(12e3)。默认值为0。
 *==Usage：==* 
	`real veriable_name;`
	`real real_data = 12e3;`

## 二值逻辑
### byte
- 表示8位有符号整数。默认值为0。
*==Usage：==*
`byte variable_name;`
### int
- 常用于testbench，表示32位有符号整数。默认值为0。
- *shortint(16bits)*、*longint(64bits)*.
*==Usage：==*
`int variable_name;`

## 数组

## 字符串

## 结构体和联合体

## 枚举类型

枚举类型可以定义一组具有命名的值。默认从左到右第一个代表数值0，接下来为1、2、···。

*Example：*
```verilog
// bit type; RED = 0, YELLOW = 1, GREEN = 2
enum bit[1:0] {RED, YELLOW, GREEN}         light_2;         
```

可以自定义命名值的代表值，指定后之后的命名值会自动被连续命名。
```verilog
// RED = 4, YELLOW = 9, GREEN = 10 (automatically assigned)
enum          {RED = 4, YELLOW = 9, GREEN} light_4;         
```

*note： 命名值不能以数字开头。*

*==Example：==*
```verilog
module tb;
	// "e_true_false" is a new data-type with two valid values: TRUE and FALSE
	typedef enum {TRUE, FALSE} e_true_false;

	initial begin
		// Declare a variable of type "e_true_false" that can store TRUE or FALSE
		e_true_false  answer;

		// Assign TRUE/FALSE to the enumerated variable
		answer = TRUE;

		// Display string value of the variable
		$display ("answer = %s", answer.name);
	end
endmodule
```

#### Ranges：

`name[N]`或者`name[N] = C`可以进行更便利的定义

*==Example：==*
```verilog
// name[N] : Generates N named constants : name0, name1, ..., nameN-1
  // BLACK0 = 0, BLACK1 = 1, BLACK2 = 2, BLACK3 = 3
  typedef enum {BLACK[4]} color_set_3;
```
#### Methods

SystemVerilog包括了一系列method来处理枚举类型。

|         | Usages                                   | Indicates       |
| ------- | ---------------------------------------- | --------------- |
| first() | function enum first();                   | 返回枚举的第一个成员的值    |
| last()  | function enum last();                    | 返回枚举最后一个成员的值    |
| next()  | function enum next (int unsigned N = 1); | 返回枚举的下一个成员的值    |
| prev()  | function enum prev (int unsigned N = 1); | 返回枚举中前一个成员的值    |
| num()   | function int num();                      | 返回给定枚举中的成员数量    |
| name()  | function string name();                  | 返回给定枚举值的字符串表示形式 |

## 自定义类型



## 数据类型转换

*转换发生在编译时，不会在运行时错误。不适合面向对象的编程概念。*

*==Usage：==*
````verilog
data_type'(variable or expression or value);
````
*Example:* (interger to int) 
````verilog
	int_data = int'(integer_data);
````
## Void

 `void`表示没有存在数据，能够声明作为被指定作为function和task的返回类型。
 
*Example：*
````verilog
function void display ();
	$display ("Am not going to return any value");
endfunction

task void display ();
	#10 $display ("Me neither");
endtask
````

## real to in Convertion

`real`到`int`的转换会四舍五入到最近的整数而不是截断(舍弃小数部分)

```verilog
// Casting will perform rounding
int'(2.0 * 3.0)
shortint'({8'hab, 8'hef})

// Using system tasks will truncate
integer  	$rtoi ( real_val )
real 		$itor ( int_val)
```