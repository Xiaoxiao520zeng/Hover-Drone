# 软件

按实际运行位置组织代码，每个可独立运行的模块保留自己的环境、启动和验证说明。

| 内容 | 位置 |
| --- | --- |
| 机载计算机上的感知、控制、规划及通信程序 | [onboard/](onboard/README.md) |
| 电脑端的监控、操作、调试及分析工具 | [ground-station/](ground-station/README.md) |

算法代码放在实际部署端的模块内；共用代码在明确存在复用需求后再提取，避免同时维护多份实现。固件放在 [firmware/](../firmware/README.md)，仿真内容放在 [simulation/](../simulation/README.md)。

技术栈和接口方案：待填写。
