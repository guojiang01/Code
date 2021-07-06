# VHDL

```vhdl
--------------------- 1------------------------
LIBRARY ieee;
USE ieee.std_logic_1164.all;
-- ieee : std_logic_1164/std_logic_arith/std_logic_signed/std_logic_unsigned
----------------------2-------------------------
ENTITY entity_name IS
	PORT (
			port_name: signal_mode signal_type;
			port_name: signal_mode signal_type;
			...);
END entity_name;
-- signal_mode : IN, OUT, INOUT, BUFFER
----------------------------------------------------
ENTITY nand_gate IS
	PORT (a,b: IN BIT;
			x: OUT BIT);
END nand_gate;
----------------------------------------------------
---------------------3----------------------------
ARCHITECTURE architecture_name OF entity_name IS
	[declarations]
BEGIN
	(code)
END architecture_name;

----------------------------------------------------
----------------------------------------------------
SIGNAL x:BIT
--将x声明为一个位宽为1的BIT类型的信号
SIGNAL y:BIT_VECTOR(3 DOWNTO 0);
--将y声明为一个位宽为4的位矢量，其中最左边的一位是最高位MSB
SIGNAL w:BIT_VECTOR(0 DOWNTO 7);
--将y声明为一个位宽为8的位矢量，其中最右边的一位是最高位MSB
x<= '1'
--x是位宽为1，值为‘1’的信号。当位宽为1时，位值放在单引号中
y<="0111"
--y是位宽为4，值为“0111”的信号。当位宽大于1时，位矢量值放在双引号中
'x' "强" 不确定值（综合后为不确定值）
'0' "强" 0 （综合后为0）
'1' "强" 1 （综合后为1）
'z' 高阻态（综合后为三态缓冲器）
'W' "弱" 不确定值
'L' "弱" 0
'H' "弱" 1
'-' 不可能出现的情况
    
x <= "101111"  二进制数，47
x <= B"101111" 二进制数，47
x <= O"57" 		八进制数，47
x <= X"2F"		十六进制，47

** 指数运算
MOD 取模	-- 不可综合
REM 取余	-- 不可综合
ABS 取绝对值 -- 不可综合
/= 不等于
sll 逻辑左移		--数据左移，右端空出来的位置填充'0'
srl 逻辑右移		--数据右移，左端空出来的位置填充'0'
sla 算数左移		--算数左移，同时复制最右端的位，在数据左移操作后填充在右端空出的位置上
sra 算数右移		--算数右移，同时复制最左端的位，在数据右移操作后填充在左端空出的位置上
rol 循环逻辑左移	--数据左移，同时从左端移出的位依次填充到右端空出的位置上
ror 循环逻辑右移	--数据右移，同时从右端移出的位依次填充到左端空出的位置上

d'LOW 返回数组索引值的下限值
d'HIGH 返回数据索引值的上限值
d'LEFT 返回数据索引值的左边界值
d'RIGHT 返回数据索引值的右边界值
d'LENGTH 返回矢量的长度
d'RANGE 返回矢量的位宽范围
d'REVERSE_RANGE 按相反的次序，返回矢量的位宽范围
s'EVENT 如果s的值发生了变化，则返回值为布尔量TRUE,否则返回FALSE
s'STALBE 如果s保持稳定，没有发生变化，则返回值为布尔量TRUE,否则返回FALSE
并置运算 z <= x & "100000000" --x<=1,z = 1100000000.
		 z <= ('1','1','1','1') -- z <= "1111".
信号是全局的，变量是局部的。信号赋值用<=,变量赋值用:=.
使用IF语句可能会带来负面影响，因为IF/ELSE语句在综合时会产生不必要的优先级解码电路
IF WAIT CASE LOOP 语句时用于顺序代码的，只能用在PROCESS/FUNCTION/PROCEDURE中

FOR/LOOP 
	[label:] FOR 循环变量 IN 范围 LOOP
		(顺序描述语句)
	END LOOP [label] 
WHILE/LOOP : 循环执行直到某个条件不再满足
	[label:] WHILE 条件表达式LOOP 
		(顺序描述语句)
	END LOOP[label]

	EXIT:结束整个循环操作
	[label:] EXIT [label] [WHEN条件表达式]
	NEXT:跳出本次循环
	[label:] NEXT [loop_label] [WHEN条件表达式]

在代码中，如果在参考时钟的两个边沿（上升和下降沿）都可以触发对同一个信号的赋值操作，那么这样的代码通常是不可综合的
CONSTANT 常量名: type := 值；  --常量
SIGNAL name : type [range] [ := initial_value]; --信号 全局
VARIABLE name : type[range] [ := 初始值]；  --变量 局部
```

|          | 信号                                                         | 变量                 |
| -------- | ------------------------------------------------------------ | -------------------- |
| 赋值符号 | <=                                                           | :=                   |
| 功能     | 表示电路内部连接                                             | 表示局部信息         |
| 范围     | 全局                                                         | 局部                 |
| 行为     | 在顺序代码中，信号值的更新不是即时的，新的值要在进程、函数或过程完成以后才有效 | 即时更新             |
| 用途     | 用于包集、实体或结构体中。在实体中，所用端口默认为信号       | 仅用于顺序描述代码中 |

### VHDL 可综合类型

| 类                       | 可综合 | 类               | 可综合 |
| ------------------------ | ------ | ---------------- | ------ |
| enmueration(枚举)类型    | 是     | array(数组)      | 是     |
| integer(整数)类型        | 是     | record(记录)数组 | 是     |
| floating-point(浮点)类型 | 否     | access(存取)类型 | 否     |
| physical(物理)类型       | 否     | file(文件)类型   | 否     |

### VHDL 标准类型

| 类型                     | 类                       | 可综合 |
| ------------------------ | ------------------------ | ------ |
| boolean(布尔)            | enumeration(枚举)类型    | 是     |
| bit(位)                  | enumeration(枚举)类型    | 是     |
| character(字符)          | enumeration(枚举)类型    | 是     |
| severity_level(错误等级) | enumeration(枚举)类型    | 否     |
| integer(整数)            | integer(整数)类型        | 是     |
| natural(自然数)          | integer(整数)的子类型    | 是     |
| positive(正整数)         | integer(整数)的子类型    | 是     |
| real(实数)               | floating-point(浮点)类型 | 否     |
| time(时间)               | physical(物理)类型       | 否     |
| string(字符串)           | 字符数组                 | 是     |
| bit_vector(位矢量)       | 位数组                   | 是     |

### 标准操作符

+ 布尔： not, and, or, nand, nor, xor, xnor;
+ 比较 ： =，/=，<，<=，>，>=；
+ 位移：sll, srl, sla, sra, rol, ror;
+ 算数：符号+，符号-，abs，+，-， \*，/，mod，rem，**；
+ 拼接： &；

### 时序VHDL

VHDL 包括两个编程域：并发域和时序域

进程基本结构：

```vhdl
process senitivity list
	declaration part
begin
	statment part
end process
```

+ 只有进程(process)才能为寄存器建模，而子程序不能描述进程，因为子程序只有顺序执行的语句，所以子程序只能描述组合逻辑（子程序与进程完全不同，进程能描述时序逻辑，而子程序只能描述组合逻辑），因此，子程序通常仅限于小的不可分割的操作。
+ 函数其实就是可在表达式中调用的子程序。

