# HAL_7_EXTIReadingPWM

## 简介

本项目基于 STM32F103 系列 MCU 和 STM32 HAL 库，演示了 TIM2 PWM 输出与外部中断计数结合 OLED 显示。
主程序使用 `TIM2_CH1` 在 `PA0` 上输出 PWM 波形，同时通过 `PA6` 的 EXTI 上升沿输入计数外部 PWM 脉冲。
OLED 屏幕用于显示实时计数结果和固定文本。

## 主要功能

- 使用 STM32 HAL 初始化 STM32F103 MCU
- 配置 TIM2 为 PWM 模式，并在 `PA0` 输出 1 kHz PWM 信号（TIM2_CH1）
- PWM 初始占空比为 30%（`Pulse = 30`, `Period = 100`）
- 通过 `PA6` 的外部中断（`EXTI9_5_IRQn`）统计上升沿次数
- 每秒读取并清零计数值，显示在 OLED 上
- OLED 采用软件 I2C 驱动，支持清屏、字符和数字显示

## 关键文件

- `CMakeLists.txt`：项目根 CMake 构建脚本
- `CMakePresets.json`：CMake 预设配置
- `config.ioc`：STM32CubeMX 项目配置
- `Core/Src/main.c`：主程序入口，PWM 输出与 OLED 显示逻辑
- `Core/Src/tim.c`：TIM2 PWM 初始化与 GPIO 后初始化
- `Core/Src/gpio.c`：GPIO 和 EXTI 初始化，包含 OLED 软件 I2C 引脚配置
- `Core/Src/stm32f1xx_it.c`：中断服务程序与 `Get_PWM_Count()` 实现
- `Core/Inc/main.h`：引脚定义与应用常量
- `Core/Src/OLED.c`：OLED 控制逻辑与软件 I2C 实现
- `Core/Inc/OLED.h`：OLED 接口声明
- `Core/Inc/OLED_Font.h`：OLED 字库数据
- `Drivers/`：STM32 HAL 库源文件和 CMSIS 头文件

## 构建环境与依赖

- CMake 3.22 及以上
- Ninja 构建器
- ARM GCC 交叉编译器（例如 `arm-none-eabi-gcc`）
- STM32 HAL 库（已包含在 `Drivers/` 目录中）

## 构建步骤

推荐使用 VS Code 的 CMake 工具或命令行：

```bash
cd d:/Electronics/HAL_Projects/HAL_7_EXTIReadingPWM
cmake --preset Debug
cmake --build --preset Debug
```

## 运行与下载

1. 生成固件后，使用 ST-Link 或其他支持的下载工具将 ELF/HEX/BIN 文件烧录到目标板。
2. 重新上电后，OLED 会显示固定说明文本和每秒更新的 PWM 脉冲计数值。

## 硬件说明

- 目标 MCU：STM32F103 系列
- OLED 软件 I2C 默认引脚：
  - `PB8`：SCL
  - `PB9`：SDA
- PWM 输出引脚：
  - `PA0`：TIM2_CH1
- 外部中断输入引脚：
  - `PA6`：EXTI 输入，用于计数上升沿脉冲

## 许可

MIT License
