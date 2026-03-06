# China Remote ID  Parser - WiFi 远程识别信标解析器


## 项目概述

CRID Parser 是一个基于 ESP-IDF 的 WiFi 信标解析器，专门用于解析 WiFi 帧中的中国远程识别（CRID）信标。该项目支持 GB42590 和 GB46750(后续支持) 中国 RID 协议，目标平台为 ESP32、ESP32-S3，能够实时捕获和解析无人机远程 ID 信息。

### 主要功能

- ✅ **多协议支持**：支持 GB42590(兼容ASTM F4311-22a) 和 GB46750 两种中国 RID 协议
- ✅ **实时捕获**：通过 WiFi 嗅探器实时捕获 Beacon 帧中的 RID 信息
- ✅ **多种输出格式**：支持文本和 JSON 两种输出格式
- ✅ **设备管理**：自动管理多个 RID 设备，支持超时清理
- ✅ **高性能**：优化的数据处理流程，支持高并发处理

## 支持的标准

| 协议 | 描述 | 生效时间 |
|------|------|----------|
| GB42590 | 无人驾驶航空器远程身份识别系统技术要求 | 已生效 |
| GB46750 | 无人驾驶航空器远程身份识别系统通用要求 | 2026年5月1日 |

## 硬件要求

- **开发板**：ESP32、ESP32S3 开发板
- **WiFi**：支持 2.4GHz WiFi
- **内存**：至少 2MB RAM
- **存储**：至少 4MB Flash

## 使用方法
### 烧录方式
1.图形化工具
通过ESP 官方提供的图形化工具 ESP Flash Download Tool（仅 Windows）
下载地址：https://www.espressif.com/zh-hans/support/download/other-tools

2.ESP-IDF 官方烧录工具：esptool.py
格式：esptool.py --chip 芯片型号 --port 串口 写操作 地址1 文件1 地址2 文件2 ...
```bash
# ESP32 烧录示例（主固件+引导程序+分区表，地址是 ESP32 标准地址）
esptool.py --chip esp32 --port /dev/ttyUSB0 --baud 921600 write_flash \
0x1000  build/bootloader/bootloader.bin \
0x8000  build/partition_table/partition-table.bin \
0x10000 build/你的工程名.bin
```
### 基本使用

1. **启动设备**：系统会自动开始扫描 WiFi 信道
2. **捕获信标**：自动检测包含 RID 信息的 Beacon 帧
3. **解析输出**：实时输出解析后的 RID 信息

### 输出格式

#### 文本格式
```
========================================
无人机 Remote ID 信息
协议版本: GB42590
数据长度: 256 bytes
MAC: AA:BB:CC:DD:EE:FF
SSID: DRONE_001
RSSI: -65 dBm

[基本识别信息]
  ID类型: 序列号
  UA类型: 自驾驶航空器
  UAS ID: ABC123456789

[位置信息]
  运行状态: 0
  航迹角: 90 °
  水平速度: 5.25 m/s
  垂直速度: 2.50 m/s
  纬度: 39.9042000 °
  经度: 116.4074000 °
  距地高度: 100 m
  时间戳: 123456789

[自识别信息]
  运行描述: 无人机巡检任务

[系统信息]
  坐标系类型: 0
  控制站纬度: 39.9042000 °
  控制站经度: 116.4074000 °
========================================
```

#### JSON 格式
```json
{
  "mac": "AABBCCDDEEFF",
  "rssi": -65,
  "ssid": "DRONE_001",
  "version": "GB42590",
  "data_length": 256,
  "id_type": "序列号",
  "ua_type": "自动驾驶航空器",
  "uas_id": "ABC123456789",
  "status": 0,
  "direction": 90,
  "speed_h": 5.25,
  "speed_v": 2.50,
  "latitude": 39.9042000,
  "longitude": 116.4074000,
  "height": 100,
  "timestamp": 123456789,
  "description": "无人机巡检任务",
  "coord_type": 0,
  "operator_latitude": 39.9042000,
  "operator_longitude": 116.4074000
}
```


重要配置项：
- **Serial flasher config** - 设置串口端口
- **Application configuration** - 应用程序配置
- **ESP-specific** - ESP 特定配置


## 常见问题

### 1. 设备无法连接
- 检查串口配置
- 确认驱动已正确安装
- 验证 USB 转串线连接

### 2. 无法捕获 RID 帧
- 确认设备在正确的信道上
- 检查 WiFi 信号强度
- 验证设备是否发送 Beacon 帧

### 3. 解析结果异常
- 检查数据完整性
- 验证协议版本支持
- 查看调试日志信息

### 4. 输出格式问题
- 确认输出格式配置
- 检查串口波特率设置
- 验证数据解析逻辑

## 性能优化

### 1. 内存优化
- 限制最大设备数量
- 定期清理不活跃设备
- 使用静态分配减少动态内存

### 2. 处理优化
- 过滤无关帧类型
- 优化数据解析流程
- 减少不必要的日志输出

### 3. 系统优化
- 调整任务优先级
- 优化堆内存使用
- 配置合适的超时时间




## 作者
- crazyZhang

## 联系方式
lytsfeng@gmail.com

## 致谢
- ESP-IDF 团队 - 提供优秀的开发框架
- 中国民航局 - 制定 RID 标准
- 开源社区 - 提供技术支持和反馈
