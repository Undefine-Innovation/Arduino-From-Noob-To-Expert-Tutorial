# 扩展IO与LED矩阵控制

## 1. 扩展IO的必要性
Arduino开发板的IO引脚数量有限，当需要控制更多设备时，可以通过扩展IO芯片（如74HC138、74HC595）来增加可用引脚数量。

## 2. 74HC138译码器原理
### 2.1 3-8线译码器
- **功能**：将3个输入信号转换为8个输出信号，实现IO扩展。
- **引脚说明**：
  - A0-A2：输入引脚
  - Y0-Y7：输出引脚
  - E1-E3：使能引脚

### 2.2 示例电路
- **连接方式**：
  - A0-A2连接Arduino的数字引脚（如D2-D4）。
  - Y0-Y7连接目标设备（如LED）。
- **代码示例**：
  ```cpp
  void setup() {
    pinMode(2, OUTPUT); // A0
    pinMode(3, OUTPUT); // A1
    pinMode(4, OUTPUT); // A2
  }

  void loop() {
    for (int i = 0; i < 8; i++) {
      digitalWrite(2, i & 0x01); // A0
      digitalWrite(3, (i >> 1) & 0x01); // A1
      digitalWrite(4, (i >> 2) & 0x01); // A2
      delay(500);
    }
  }
  ```

## 3. LED矩阵控制
### 3.1 8x8 LED点阵
- **原理**：通过行列扫描方式控制多个LED，分为静态扫描和动态扫描。
  - **静态扫描**：逐行或逐列点亮LED。
  - **动态扫描**：快速切换行列，利用视觉暂留效应显示图案。

### 3.2 驱动方式
- **直接驱动**：使用Arduino引脚直接控制行列（需较多引脚）。
- **移位寄存器驱动**：使用74HC595减少引脚占用。

## 4. 74HC595移位寄存器
### 4.1 工作原理
- **功能**：通过串行输入、并行输出扩展IO。
- **引脚说明**：
  - DS：串行数据输入
  - SHCP：时钟输入
  - STCP：锁存输入
  - Q0-Q7：并行输出

### 4.2 示例代码
```cpp
void setup() {
  pinMode(8, OUTPUT); // DS
  pinMode(9, OUTPUT); // SHCP
  pinMode(10, OUTPUT); // STCP
}

void shiftOutData(byte data) {
  digitalWrite(10, LOW);
  shiftOut(8, 9, LSBFIRST, data);
  digitalWrite(10, HIGH);
}

void loop() {
  shiftOutData(0xFF); // 点亮所有LED
  delay(1000);
  shiftOutData(0x00); // 关闭所有LED
  delay(1000);
}
```

## 5. 项目：滚动文字显示
### 5.1 实现步骤
1. 使用74HC595驱动LED点阵。
2. 设计字符的点阵数据（如字母、数字）。
3. 通过动态扫描实现滚动效果。

### 5.2 示例代码
```cpp
byte font[5] = {0x3E, 0x51, 0x49, 0x45, 0x3E}; // 字母"A"的点阵数据

void displayColumn(byte column) {
  shiftOutData(column);
}

void loop() {
  for (int i = 0; i < 5; i++) {
    displayColumn(font[i]);
    delay(100);
  }
}
```