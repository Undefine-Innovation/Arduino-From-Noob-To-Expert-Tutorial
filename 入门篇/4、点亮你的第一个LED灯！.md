# 基础实验：点灯！
现在，你已经安装了Arduino IDE了！点亮开发板上的LED灯试试吧？

## 实验介绍
恭喜你已经安装好Arduino IDE！现在让我们开始第一个基础实验 - 点亮开发板上的LED灯。这是学习Arduino编程的第一步，也是最经典的入门实验。

## 所需材料
- Arduino开发板（如Arduino Uno）
- USB数据线（用于连接电脑和开发板）
- 已安装的Arduino IDE软件

## 实验步骤

### 1. 连接开发板
1. 使用USB数据线将Arduino开发板连接到电脑
2. 确保电脑识别到了设备（在设备管理器中可以查看）

### 2. 打开Arduino IDE
1. 启动Arduino IDE软件
2. 在"工具" > "开发板"菜单中，选择你使用的Arduino型号（如Arduino Uno）
3. 在"工具" > "端口"菜单中，选择正确的COM端口（通常会有Arduino标识）

### 3. 编写代码
1. 在代码编辑区输入以下代码：

```arduino
void setup() {
  // 初始化数字引脚13为输出模式
  // 大多数Arduino板上，引脚13连接了一个内置LED
  pinMode(13, OUTPUT);
}

void loop() {
  digitalWrite(13, HIGH);   // 点亮LED（高电平）
  delay(1000);              // 等待1秒
  digitalWrite(13, LOW);    // 熄灭LED（低电平）
  delay(1000);              // 等待1秒
}
```

### 4. 上传程序
1. 点击工具栏中的"验证"按钮（✓）检查代码是否有错误
2. 确认无误后，点击"上传"按钮（→）将程序上传到开发板
3. 等待上传完成（状态栏会显示"上传完成"）

### 5. 观察结果
- 开发板上的"L"标记的LED应该开始以1秒的间隔闪烁
- 如果使用其他型号的开发板，可能需要查找板载LED连接的引脚号（有些是13，有些可能是其他数字）

## 代码解析
- `pinMode(13, OUTPUT)`：设置13号引脚为输出模式，可以控制外部设备
- `digitalWrite(13, HIGH)`：给13号引脚输出高电平（5V），点亮LED
- `digitalWrite(13, LOW)`：给13号引脚输出低电平（0V），熄灭LED
- `delay(1000)`：程序暂停1000毫秒（1秒）

## 扩展实验
1. 尝试修改delay的时间参数，改变LED闪烁的频率
2. 尝试使用其他数字引脚（如2-12）连接外部LED进行控制
3. 尝试编写不同的闪烁模式（如摩尔斯电码）

## 常见问题
Q: LED没有闪烁怎么办？
A: 检查开发板是否正确连接，确认选择了正确的开发板型号和端口，检查代码是否有错误。

Q: 如何知道我的开发板LED连接的是哪个引脚？
A: 查阅你的开发板说明书，大多数Arduino Uno板是13号引脚，有些开发板可能有不同。

恭喜你完成了第一个Arduino实验！接下来你可以尝试更复杂的项目了。

# 进阶实验：流水灯

## 所需材料
- Arduino Uno ×1
- 面包板 ×1
- LED ×6
- 220Ω电阻 ×6

## 连接示意图
图2-23为本实验的连接示意图，在各LED正极和Arduino Uno引脚之间，串联了一个限流电阻，并将LED负极和Arduino的GND相连。

> 本书中大多示意图都是使用Fritzing制作，可以在以下网址下载该软件：  
> [http://fritzing.org/download/](http://fritzing.org/download/)

![图2-23 流水灯实验连接示意图](https://arduino.me/storage/v1/object/public/image/2ee1a898ab371d874c4dfaa5e5874149.webp)

## 原理图
对应原理图见图2-24。

![图2-24 流水灯实验原理图](https://arduino.me/storage/v1/object/public/image/1a7cb3bebcc7aa0d018cc8cc3356d5dc.webp)

## 示例程序
示例程序代码如下：

```arduino
/*
Arduino Uno制作流水灯
https://arduino.me/
*/

void setup() 
{
  // 初始化I/O口
  for(int i=2;i<8;i++)
    pinMode(i,OUTPUT);
}

void loop() 
{
  // 从引脚2到引脚6，逐个点亮LED，等待1秒再熄灭LED
  for(int i=2;i<7;i++)
  {
    digitalWrite(i,HIGH);
    delay(1000);
    digitalWrite(i,LOW);   
  }
  
  // 从引脚7到引脚3，逐个点亮LED，等待1秒再熄灭LED
  for(int i=7;i>2;i--)
  {
    digitalWrite(i,HIGH);
    delay(1000);
    digitalWrite(i,LOW);   
  } 
}
