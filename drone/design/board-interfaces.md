# 飞控与板卡接口登记

更新日期：2026-11-22。X25 Evo 和 Jetson Orin 系列已经确定，**Jetson 载板型号尚未确定**。本页登记厂商接口定义与项目连接方案；所有连接均尚无实物核验记录。对应的板间连接表为 [wiring.csv](wiring.csv)。CSV 按信号网络和接口束级记录；并联电源与成品 USB 线保留束级，不虚构单芯编号，不能替代最终线束制造图。

## 板卡与适用范围

位号沿用本地 `UAV-E-002 REV D` 候选资料，便于对照，不表示已采用整套旧方案。

| 位号 | 板卡 | 当前采用状态 |
| --- | --- | --- |
| A2 | CUAV 雷迅 X25 Evo | 已确定飞控；实物硬件修订、固件与参数仍需登记 |
| A5 | NVIDIA Jetson Orin | 已确定平台系列；具体模块、载板与系统版本需设备登记 |
| A5-C | Jetson 载板 | 型号未选；下文 P3768 仅为条件参考，不能据此制作最终线束 |
| A1 | PMU 2 Lite 15V 版 | 旧方案候选，区别于 5V 版 |
| A3 | Holybro Tekko32 F4 4-in-1 50A，旧资料为 SKU31102 V1.5 | 候选；必须核对 PCB 与固件修订，不能用同 SKU 现售版本替代 |
| A4 | Matek BEC12S-PRO | 候选；旧方案拟设 12V，并非模块默认输出 |
| A6 | 相机稳压模块 | 型号未选；只登记供电功能，不填写板端针号 |
| A7 | Luxonis Y-Adapter | 旧方案候选，区分 DATA、POWER 与 OUT |
| A8 | CAN 扩展板 | 板型未选；只登记 CAN 网络，不推定板端针号 |
| A12 | XT60 并联分线板 | 板型未选；P0～P4 是旧方案命名，不是已核实的厂家端口 |
| MCU-SW | 开关读取 MCU | 板型未选；D2 仅见于现有 Arduino 风格示例 |

## X25 Evo 接口定义

依据 [CUAV 原厂接口图](https://doc.cuav.net/assets/controller/x25/pinouts.jpg)、[X25 Evo 规格](https://doc.cuav.net/controller/x25-evo/)和[官方快速布线指南](https://doc.cuav.net/en/controller/x25-evo/quick-start-x25-evo.html)。

**读表方式：** 原厂图没有为下列插座标数字针号。本文的“线序 1、2……”从图中的 `5V/VIN` 端向 `GND` 端排列，属于仓库线序标记；它不是厂家数字针号，也不是插头线侧观察时的左右顺序。认针时对照原厂图的锁扣和信号标识，插头线侧与插座侧可能镜像。NVIDIA 的 J12 数字则是其官方物理排针编号，两种编号方式不能混用。

### POWER C1 / POWER C2：8 芯供电与 CAN

| 本文线序 | POWER C1 信号 | POWER C2 信号 | 相对飞控的作用 |
| --- | --- | --- | --- |
| 1～3 | VIN / VIN / VIN | VIN / VIN / VIN | 电源输入；图中标 `16V IN`，规格允许范围为 10～18V |
| 4 | CAN1_H | CAN2_H | 对应 CAN 总线，高线，双向 |
| 5 | CAN1_L | CAN2_L | 对应 CAN 总线，低线，双向 |
| 6～8 | GND / GND / GND | GND / GND / GND | 电源地 |

规格列出的 POWER 接插件型号对为 `5055670871 / 5055650801`。旧方案拟用 PMU 2 Lite **15V 版**原装 8 芯线接 POWER C1，POWER C2 保留。PMU 的 20～70V 是上游输入范围，不是 X25 POWER 口的输入范围；不将普通 5V 电源模块或原始电池电压直接套入本接口。

### TELEM1 / TELEM2：6 芯串口

| 本文线序 | 信号 | 相对飞控的方向 | 本项目 TELEM2 逻辑分配 |
| --- | --- | --- | --- |
| 1 | 5V+ | 外设电源输出 | 留空，Jetson 独立供电 |
| 2 | TX | 飞控 -> 外设 | 接实际载板的 UART RX |
| 3 | RX | 外设 -> 飞控 | 接实际载板的 UART TX |
| 4 | CTS | 外设 -> 飞控 | 三线 UART 方案留空，并关闭硬件流控 |
| 5 | RTS | 飞控 -> 外设 | 三线 UART 方案留空，并关闭硬件流控 |
| 6 | GND | 信号参考地 | 接实际载板的信号 GND |

TELEM2 分配给机载 MAVLink 是当前接线方案，TELEM1 保留。**所核对的 X25 专用规格和针脚图未明确给出 TELEM 信号电压。** 载板 UART 的电平必须与飞控匹配：先取得厂家电气确认并核对实物，再决定直连或电平适配。不能从 PWM 可选 3.3V/5V 推断 UART 电平，也不能把 TELEM 的 5V 电源脚当成 5V 逻辑说明。

固件配置登记实际端口、协议、波特率和流控设置；不要由 `TELEM2` 名称直接猜 ArduPilot 参数序号或 Linux 设备名。两端设置一致后记录链路验证结果。

### CAN 与 PWM / 舵机输出

| 接口 | 线序 / 排针标识 | 本项目分配与约束 |
| --- | --- | --- |
| CAN1 独立插座 ×3 | 从 5V 端起：5V、CAN1_H、CAN1_L、GND | 同一 CAN1 总线的多个引出口；POWER C1 也带 CAN1 |
| CAN2 独立插座 ×2 | 从 5V 端起：5V、CAN2_H、CAN2_L、GND | 候选用于 A8 扩展；板型未选，不填 A8 针号；POWER C2 也带 CAN2 |
| M1～M16 的 `S` | 各通道信号输出 | 旧方案拟 M1～M4 控制四合一电调，其余通道按执行器和固件资源再分配 |
| M1～M16 的 `-` | GND | 电调及机械臂执行器的信号地与飞控参考地对应连接，实际公共地路径按接线图核对 |
| M1～M16 的 `+` | 共用舵机电源轨 | 飞控不为舵机供电；执行器需要适配的独立 BEC，不能把该排当成 5V 稳压输出 |

官方说明 PWM 信号可配置 3.3V/5V，正负电源轨分别相通。舵机具体输出通道尚未分配，不能据“Gripper 1”推定为物理 M1。若使用 PX4，[对应控制器文档](https://docs.px4.io/main/en/flight_controller/cuav_x25-evo)注明 M1～M8 支持 DShot、M9～M16 不支持，并有定时器分组限制；这些是 PX4 说明，不能无条件当成其他固件的配置结论。

### 其他接口登记

| 接口 | 厂商功能 / 信号 | 当前用途 |
| --- | --- | --- |
| UART4 | 从 5V 端起：5V、TX、RX、GND；无 CTS/RTS | 保留串口，不与 TELEM2 机载链路重复占用 |
| USB Type-C | USB 通信；USB 供电输入 4.75～5.25V | 刷写、日志和台架连接；不承担 Jetson 供电 |
| GPS & SAFETY | GPS 串口、I2C、安全开关、蜂鸣器等，按原厂 10 芯图 | 外设未选，保留 |
| GPS2 | GPS 串口与 I2C，按原厂 6 芯图 | 外设未选，保留 |
| I2C1 / I2C2 / I2C3 | 5V、SCL、SDA、GND | 外设未选，保留；信号电气条件另核对 |
| RC IN | 5V、RC IN、GND | 接收机未选，保留；串口协议接收机按固件说明使用 UART |
| ETH | TXP、TXN、RXP、RXN | 保留专用以太网接口，按厂家连接方案使用 |
| DEBUG | 调试串口与 SWD | 保留调试用途，不代作已核实的 TELEM 电气规格 |
| SPI6、RSSI、ADC3V3、ADC6V6 | 对应原厂图的扩展与模拟输入 | 保留；模拟输入名称的量程不能与旁边的 5V 电源混淆 |
| TF 卡 | 日志存储 | 卡型和固件日志配置随设备登记 |

## Jetson 载板：未选型时的连接需求

机载端需承接 MAVLink 通信、相机数据、开关 MCU 数据及独立供电。先核对实际载板是否提供匹配电平的 UART、USB 主机连接、可用电源入口及散热空间，再固定连接器名称和针号。相机和 USB MCU 若同时采用，需要两个可用 USB 主机连接或经核对的扩展方案。

### 仅当采用 NVIDIA P3768 时的条件参考

用户尚未确定载板。下表摘取官方 `SP-11324-001_v1.3`（2024-12）的相关定义，不把候选 P3768-A04 或 Orin NX 16GB 组合写成最终选型。

| P3768 接口 | 官方定义 | 与旧方案的关系 |
| --- | --- | --- |
| J12 p8 | UART1_TXD，3.3V 信号 | 条件接 X25 TELEM2 RX；还需核对 X25 电平和引脚复用 |
| J12 p10 | UART1_RXD，3.3V 信号 | 条件接 X25 TELEM2 TX；还需核对 X25 电平和引脚复用 |
| J12 p9 | GND | 条件连接信号地 |
| J12 p11 / p36 | RTS / CTS，对应功能需复用配置 | 旧方案为三线 UART，流控不连接；按系统和设备树核对 |
| J16 | 9～20V DC；5.5×2.5mm，中心正，插入段 9.5mm，插座最大 3.5A | 旧方案 12V 是候选设定，不是所有 Orin 载板通用电压 |
| J6 / J7 | 两个双层 USB-A，共四个端口，仅主机模式 | 条件用于相机 DATA 与开关 MCU |
| J5 USB-C | 数据、主机/设备及 Recovery；不作为开发板供电口 | 不能因 USB-C 外形就按 PD 电源口接线 |
| J14 | 含调试 UART 与控制信号 | 与 J16 电源不同；本项目不分配调试针脚 |

依据：[NVIDIA 载板规范](https://developer.nvidia.com/downloads/assets/embedded/secure/jetson/orin_nano/docs/jetson_orin_nano_devkit_carrier_board_specification_sp.pdf)，正文 21～23 页图3-1/表3-3、26～27 页图3-2/表3-8；USB 见正文7～10页。另见[官方接口布局](https://docs.nvidia.com/jetson/orin-nano-devkit/user-guide/latest/hardware_layout.html)和[Jetson-IO 复用说明](https://docs.nvidia.com/jetson/archives/r36.5/DeveloperGuide/HR/ConfiguringTheJetsonExpansionHeaders.html)。不预填 `/dev/ttyTHS*` 名称；模块、载板、系统和 pinmux 应成套登记。

## 其他板卡的输入、输出与连接

以下均按旧资料整理为候选，尚未放行为实物接线。

| 板卡 | 输入接口 | 输出 / 下游 | 核对重点 |
| --- | --- | --- | --- |
| PMU 2 Lite 15V 版 | BATT-IN+ / BATT-IN-，厂商输入范围 20～70V | BATT-OUT+ / - 是主电源通路；POWER 给飞控 15V/6A 与 CAN | 核对版本和原装 8 芯线；不能把 BATT-OUT 当成 15V 稳压输出 |
| Matek BEC12S-PRO | IN / G，输入 9～55V | OUT / G，默认 5.2V，可选 8V/12V | 12V 输出要求输入至少14V；连续5A/峰值9A是厂商条件额定，整机另测；不能直接沿用旧图焊盘设定 |
| Tekko32 V1.5 候选 | BAT+ / BAT- 为主电源；M1～M4 为控制信号输入，控制口见对应修订的原厂图 | 各电机三相输出 | 无 BEC 输出；本地8针图示为 VBAT/TLM/GND/CURRENT/M1/M2/M3/M4，不把图示顺序当任意朝向的数字针号 |
| Luxonis Y-Adapter | DATA 接 USB 主机，POWER 接独立5V | OUT 接 OAK 相机 USB-C | 不提供从原始电池电压降到5V的功能；DATA/POWER不能互换 |
| A6 相机稳压模块 | 原始电池侧输入，具体范围随型号核定 | 匹配相机和 Y-Adapter 的合规5V供电 | 型号未选，板端针号不分配 |
| A8 CAN 扩展板 | CAN_H、CAN_L、GND与适用供电 | 下游 CAN 外设 | 型号未选，端接与板端针号按实物确认，不把五个 X25 CAN 插座当五条总线 |
| A12 并联分线板 | 旧方案 P0 接原始电池网络 | P1/P2 等支路仍为原始电池网络 | 本身不稳压；端口名和极性须对实物核对 |
| 开关读取 MCU | 现有示例用 D2/INPUT_PULLUP，开关 COM 接 GND、NO 接 D2 | USB 串口发送 0/1 状态，原型115200 | MCU板型未选，D2仅为代码示例，不作为X25或Jetson针脚 |

厂商依据：[PMU 手册](https://manual.cuav.net/power-module/PMU2_Lite_manual_cn.pdf)、[Matek BEC](https://www.mateksys.com/?portfolio=bec12s-pro)、[Holybro V1.5 规格](https://cdn.shopify.com/s/files/1/0604/5905/7341/files/Holybro_Tekko32_F4_4in1_50A_ESC_spec_7f900ab6-7c29-483f-86a9-260c69b669fd.pdf?v=1665394698)、[Luxonis Y-Adapter](https://docs.luxonis.com/hardware/products/Y-Adapter)。旧方案 W03 只使用 M1～M4 信号和 GND，控制口的 VBAT、TLM、CURRENT 保留不接。同 SKU 的 Holybro 现售页已有不同固件/PCB配置，需核对[版本与电流标度说明](https://docs.holybro.com/esc/current-sensor-scale)。

## 接线核对与记录

- 按板卡实物型号、修订、插座标识和官方图逐端核对；线色不能替代信号名。记录采用的手册版本和接头观察方向。
- 信号表与电源表分开检查：尤其区分原始电池、飞控 VIN、Jetson 输入、相机5V及PWM舵机电源轨。未选载板或未核电平的 UART 连接保持未放行。
- 台架检查前断电核对极性、短路、导通与稳压设定，电机相关检查按既有流程拆桨；不会在本次文档更新中给实机上电或执行命令。
- 实测读数、配置和检查结果关联[组间接口](../../docs/interfaces.md)与[实验记录](../../experiments/README.md)。操作者姓名和账号由本人登记。
