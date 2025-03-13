---
{"dg-publish":true,"permalink":"/SystemVerilog/SystemVerilog Loops/"}
---

## 循环
- 循环是一段代码，它会反复执行。
- 通常在循环中包含一个条件语句，以便在条件满足时终止循环。如果循环无限运行，仿真将无限挂起。

## 不同类型的循环结构
### `forever`
- 无限循环，类似于 `while (1)`。
- 注意：除非在 `forever` 块内包含时间延迟以推进仿真时间，否则仿真会挂起。
*==Example：==*:
  ```systemverilog
  module tb;
    initial begin
      forever begin
        #5 $display("Hello World!");
      end
    end
    initial
      #50 $finish;
  endmodule
  ```

### `repeat`
- 重复给定的语句块指定次数。
*==Example：==*
  ```systemverilog
  module tb;
    initial begin
      repeat(5) begin
        $display("Hello World!");
      end
    end
  endmodule
  ```

### `while`
- 只要给定的条件为真，就会重复执行块中的语句。
*==Example：==*
  ```systemverilog
  module tb;
    bit clk;
    always #10 clk = ~clk;
    initial begin
      bit [3:0] counter;
      $display("Counter = %0d", counter); // Counter = 0
      while (counter < 10) begin
        @(posedge clk);
        counter++;
        $display("Counter = %0d", counter); // Counter increments
      end
      $display("Counter = %0d", counter); // Counter = 10
      $finish;
    end
  endmodule
  ```

### `for`
- 类似于 Verilog/C，允许在一行中指定起始值、条件和增量表达式。
*==Example：==*
  ```systemverilog
  module tb;
    bit clk;
    always #10 clk = ~clk;
    initial begin
      bit [3:0] counter;
      $display("Counter = %0d", counter); // Counter = 0
      for (counter = 2; counter < 14; counter = counter + 2) begin
        @(posedge clk);
        $display("Counter = %0d", counter); // Counter increments
      end
      $display("Counter = %0d", counter); // Counter = 14
      $finish;
    end
  endmodule
  ```

### `do while`
- 先执行代码，然后再检查条件以确定是否应再次执行代码。
*==Example：==*
  ```systemverilog
  module tb;
    bit clk;
    always #10 clk = ~clk;
    initial begin
      bit [3:0] counter;
      $display("Counter = %0d", counter); // Counter = 0
      do begin
        @(posedge clk);
        counter++;
        $display("Counter = %0d", counter); // Counter increments
      end while (counter < 5);
      $display("Counter = %0d", counter); // Counter = 5
      $finish;
    end
  endmodule
  ```

### `foreach`
- 主要用于遍历数组变量，无需手动设置索引。
*==Example：==*
  ```systemverilog
  module tb_top;
    bit [7:0] array[8]; // 创建一个固定大小的数组
    initial begin
      // 为数组的每个位置分配一个值
      foreach (array[index]) begin
        array[index] = index;
      end
      // 遍历每个位置并打印当前位置的值
      foreach (array[index]) begin
        $display("array[%0d] = 0x%0d", index, array[index]);
      end
    end
  endmodule
  ```

