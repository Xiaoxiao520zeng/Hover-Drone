# 参考资料

资料用于支持选型、接口设计和验证，不代表对应方案已经在本项目通过实测。下面的官方入口于 2026-11-21 核对；具体固件、JetPack 或依赖版本以项目实际设备登记和锁定文件为准。

## 官方资料索引

| 资料 | 来源 | 用途与使用边界 | 维护角色 |
| --- | --- | --- | --- |
| X25 EVO 产品与接口文档 | [CUAV 官方文档](https://doc.cuav.net/controller/x25-evo/) | 项目确定飞控；查安装、端口、供电和固件支持，不代替实物版本核对 | 无人机组 |
| X25 EVO 固件入口 | [CUAV 下载中心](https://dl.cuav.net/controller/x25-evo.html) | 登记实际采用的固件类型与版本，配套保存参数和适配记录 | 无人机组 |
| Jetson Orin 平台资料 | [NVIDIA Jetson Orin](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/) | 项目确定机载平台系列；用于查子型号、载板和模块文档，不自行假定具体型号 | 无人机组 |
| JetPack 软件栈 | [NVIDIA JetPack](https://developer.nvidia.com/embedded/jetpack) | 按实际模块及载板选择兼容系统，登记已安装版本后验证原型依赖 | 无人机组 |
| ArduCopter GUIDED 运动接口 | [ArduPilot 开发文档](https://ardupilot.org/dev/docs/copter-commands-in-guided-mode.html) | 解释现有原型的飞控控制接口；实际固件、定位和模式条件需联调验证 | 无人机组 |
| 非 GPS 外部位置估计 | [ArduPilot 开发文档](https://ardupilot.org/dev/docs/mavlink-nongps-position-estimation.html) | 研究采用 ArduPilot 时的外部位姿接入；不能把目标相对深度直接当整机定位 | 无人机组 |
| 夹爪控制接口 | [ArduPilot Grippers](https://ardupilot.org/copter/docs/common-gripper-landingpage.html) | 核对原型闭爪命令与执行端配置，区分命令处理和实体夹持结果 | 两组共同 |
| DepthAI 相机坐标 | [Luxonis Coordinate Systems](https://docs.luxonis.com/software-v3/depthai/depthai-components/coordinate-systems) | OAK 原型的坐标与外参参考；实际相机是否采用由视觉验证决定 | 无人机组 |

## 已有本地资料与归档状态

2026-11-22 补充核对的板卡针脚、电源和版本资料，集中索引于[板卡接口登记](../../drone/design/board-interfaces.md)。其中包含 CUAV 原厂针脚图、NVIDIA P3768 载板规范、PMU 2 Lite、Matek BEC、Holybro 电调与 Luxonis Y-Adapter 的官方来源。P3768 是条件参考，项目实际载板尚未确定。

原始文件尚未批量导入仓库。文件名用于定位团队已有输入，进入仓库后由对应组补充来源版本和实际文件链接。

| 已有输入 | 已知内容 | 归档责任与目标 | 当前状态 |
| --- | --- | --- | --- |
| 多套 `树枝无人机_*_STL` 方案 | 模型生成脚本、结构预览、装配与走线说明、STL | 无人机组整理到 `drone/design/` | 候选设计，装配尺寸按实物复核 |
| 板级接线图、整机供电流程图 | DXF 源文件和 PNG 预览 | 无人机组整理到 `drone/design/wiring/`，机械臂组核对组间部分 | 原资料输入，不能直接视为最终接线版 |
| `rod_perching_test` | OAK 固定标记横杆视觉、目标锁定、对准、触碰与闭爪请求 | 无人机组整理到 `drone/code/` | 有本地原型，仓库版本尚无运行记录 |
| `rod_perching_test/switch_reader` | Arduino 风格开关去抖和 USB 串口发送示例 | 机械臂组整理到 `robotic-arm/code/` | 示例代码，MCU 板型和接线需要设备登记 |
| 外观概念图、刚性悬挂机构图片 | 概念表达和文献参考 | 公共参考资料由两组维护 | 先核对来源；不能确认转载条件的只保留链接与阅读笔记 |

新资料按“标题、来源、项目用途、适用版本、维护组”补充。按主题建立子目录，避免反复上传整包 ZIP；原作者、许可证和必要引用说明随资料保留。姓名与账号由提供者自行登记。
