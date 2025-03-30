# 模拟信号与ADC/DAC

## 1. 模拟信号基础
模拟信号是一种连续变化的信号，可以表示无限多个数值。在Arduino中，模拟信号常用于读取传感器数据（如温度、光照强度等）或控制模拟设备（如电机速度、LED亮度等）。

## 2. 模拟输入（ADC）
### 2.1 电位器读取
- **原理**：电位器通过分压原理输出模拟电压，Arduino的ADC（模数转换器）将其转换为数字值（0-1023）。
- **示例代码**：
  ```cpp
  void setup() {
    Serial.begin(9600);
  }

  void loop() {
    int sensorValue = analogRead(A0); // 读取A0引脚
    Serial.println(sensorValue);
    delay(100);
  }
  ```

### 2.2 光敏电阻读取
- **原理**：光敏电阻的阻值随光照强度变化，通过分压电路转换为模拟电压。
- **注意事项**：
  - 使用合适的电阻值匹配光敏电阻的范围。
  - 可能需要校准以获取准确的光照值。

## 3. PWM模拟输出（DAC模拟）
### 3.1 PWM基础
- **原理**：PWM（脉宽调制）通过快速开关数字信号模拟模拟输出，控制设备（如LED、电机）的平均电压。
- **Arduino支持**：引脚标注“~”支持PWM（如3, 5, 6, 9, 10, 11）。

### 3.2 LED调光
- **示例代码**：
  ```cpp
  void setup() {
    pinMode(9, OUTPUT);
  }

  void loop() {
    for (int brightness = 0; brightness <= 255; brightness++) {
      analogWrite(9, brightness); // 设置PWM值（0-255）
      delay(10);
    }
  }
  ```

### 3.3 电机调速
- **原理**：通过PWM控制电机的平均电压，实现调速。
- **注意事项**：
  - 需配合电机驱动模块（如L298N）使用。
  - 避免直接驱动大电流电机。

## 4. 模拟信号滤波
### 4.1 RC低通滤波电路
- **原理**：通过电阻和电容组成的电路滤除高频噪声。
- **设计公式**：截止频率 \( f_c = \frac{1}{2\pi RC} \)。
- **示例电路**：
  - 电阻：10kΩ
  - 电容：0.1μF
  - 截止频率：约160Hz

## 5. 项目示例：光控LED
```cpp
void setup() {
  pinMode(9, OUTPUT); // PWM引脚
}

void loop() {
  int lightValue = analogRead(A0); // 读取光敏电阻
  int brightness = map(lightValue, 0, 1023, 0, 255); // 映射到PWM范围
  analogWrite(9, brightness);
  delay(100);
}
```