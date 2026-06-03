# HAL_8_TIMEncoder

## 简介

本项目基于 STM32F103 系列 MCU 和 STM32 HAL 库，演示了 TIM1 编码器接口与 OLED 显示。
TIM1 以编码器模式读取外部旋转编码器信号，并在 OLED 上实时显示计数值。

## 主要功能

- 使用 STM32 HAL 初始化 STM32F103 MCU
- 配置 TIM1 为编码器模式（TIM1_CH1/CH2）
- 读取 TIM1 的 16 位计数器 `CNT` 并显示在 OLED 上
- 演示 `int16_t`/`uint16_t` 与 `int32_t` 计数表示的区别
- OLED 显示当前编码器计数值

## 说明

- `GetEncoderCounts()` 从 `TIM1->CNT` 获取计数值
- TIM1 硬件计数器为 16 位，计数范围是 `0~65535`
- 当计数器溢出后会回绕到 `0`，因此直接读取只能得到 16 位范围内的值

## 关键文件

- `CMakeLists.txt`：项目根 CMake 构建脚本
- `CMakePresets.json`：CMake 预设配置
- `config.ioc`：STM32CubeMX 项目配置
- `Core/Src/main.c`：主程序入口、OLED 显示和编码器读取
- `Core/Src/tim.c`：TIM1 编码器初始化与 GPIO 后初始化
- `Core/Src/gpio.c`：GPIO 初始化与 OLED 引脚配置
- `Core/Src/OLED.c`：OLED 控制逻辑与字符/数字显示实现
- `Core/Inc/OLED.h`：OLED 接口声明
- `Core/Inc/main.h`：外设句柄和函数声明
- `Drivers/`：STM32 HAL 库源文件和 CMSIS 头文件

## 构建环境与依赖

- CMake 3.22 及以上
- Ninja 构建器
- ARM GCC 交叉编译器（例如 `arm-none-eabi-gcc`）
- STM32 HAL 库（已包含在 `Drivers/` 目录中）

## 构建步骤

推荐使用 VS Code 的 CMake 工具或命令行：

```bash
cd d:/Electronics/HAL_Projects/HAL_8_TIMEncoder
cmake --preset Debug
cmake --build --preset Debug
```

## 运行与下载

1. 生成固件后，使用 ST-Link 或其他支持的下载工具将 ELF/HEX/BIN 文件烧录到目标板。
2. 重新上电后，OLED 会显示编码器计数值。

## 硬件说明

- 目标 MCU：STM32F103 系列
- 编码器信号引脚：
  - `PA8`：TIM1_CH1
  - `PA9`：TIM1_CH2
- OLED 显示引脚由项目 `GPIO` 初始化配置

## 许可

MIT License
