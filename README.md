# Hover-Drone

面向 GPS 拒止环境的树枝停栖无人机项目。目标是通过自研机械臂抓取树枝等圆柱物，实现悬挂停留；本仓库统一保存项目资料、硬件设计、软件代码和实验记录。

**当前阶段：仓库初始化。** 具体技术路线、成员分工和验收指标由团队在[项目计划](docs/project-plan.md)与[分工表](docs/team.md)中确认。已有机体结构方案、接线图及横杆对准试验代码的归档位置见[现有资料整理说明](docs/maintainer-setup.md#现有资料归档示例)；原始资料尚未批量导入，本仓库暂未包含可运行的无人机系统。

## 新成员从这里开始

1. 阅读[协作指南](CONTRIBUTING.md)，确认你要上传的内容放在哪个目录。
2. 在 [Issues](https://github.com/Xiaoxiao520zeng/Hover-Drone/issues) 建立任务，写明交付物和验收标准，由一位成员负责。
3. 在自己的分支上传资料或代码，发起 Pull Request（PR，合并请求），由另一位成员检查后合入 `main`。

只会网页上传也可以参与，具体步骤见[网页上传说明](CONTRIBUTING.md#通过网页上传)。

## 内容放在哪里

| 内容 | 位置 | 应同时提供 |
| --- | --- | --- |
| 项目目标、计划、分工、接口 | [docs/](docs/README.md) | 负责人、状态、验收方式 |
| 论文、手册、调研资料 | [docs/references/](docs/references/README.md) | 来源、用途、资料链接 |
| 会议纪要与决策 | [docs/meetings/](docs/meetings/README.md) | 日期、结论、后续任务 |
| 结构、CAD、STL、装配资料 | [hardware/mechanical/](hardware/mechanical/README.md) | 可编辑源文件、导出件、版本说明 |
| 原理图、PCB、接线、供电 | [hardware/electronics/](hardware/electronics/README.md) | 可编辑源文件、预览、接口说明 |
| 采购与物料清单 | [hardware/bom.csv](hardware/bom.csv) | 型号、数量、所属模块与版本 |
| 飞控、单片机、底层驱动 | [firmware/](firmware/README.md) | 环境、构建与烧录说明 |
| 机载计算、感知、规划代码 | [software/onboard/](software/onboard/README.md) | 依赖、启动方式、输入输出 |
| 上位机、地面站、调试工具 | [software/ground-station/](software/ground-station/README.md) | 依赖、使用方法、验证步骤 |
| 仿真模型、场景和仿真脚本 | [simulation/](simulation/README.md) | 仿真软件版本、运行方法 |
| 实验方案、结果与数据索引 | [experiments/](experiments/README.md) | 硬件版本、代码提交、参数与结论 |

算法按实际运行位置放入 `software/onboard/` 或对应模块；纯仿真算法放入 `simulation/`。每项内容只保留一个权威位置，通过链接引用。

```text
Hover-Drone/
├── README.md
├── CONTRIBUTING.md
├── docs/                  计划、分工、接口、参考资料、会议
├── hardware/              机械、电子、物料清单
├── firmware/              飞控与嵌入式代码
├── software/              机载端与上位机代码
├── simulation/            仿真环境与模型
├── experiments/           实验记录与数据索引
└── .github/               任务、问题和 PR 模板
```

## 团队约定

- `main` 保存已检查、可供团队使用的内容；日常修改通过分支和 PR 合入。
- 按模块归档，不按成员姓名建立长期目录，不把资料堆在仓库根目录。
- 代码提交源文件；图纸提交可编辑源文件和必要导出件；不反复上传整个项目的 ZIP。
- 实验结论要能追溯到硬件版本、代码提交和参数，使用[实验模板](experiments/TEMPLATE.md)。
- 大视频、数据集和原始日志放团队共享存储，仓库记录访问链接、版本和校验信息。

## 项目入口

- [项目计划与验收标准](docs/project-plan.md)
- [成员分工](docs/team.md)
- [模块接口约定](docs/interfaces.md)
- [任务列表](https://github.com/Xiaoxiao520zeng/Hover-Drone/issues)
- [待评审的修改](https://github.com/Xiaoxiao520zeng/Hover-Drone/pulls)
- [仓库管理员设置清单](docs/maintainer-setup.md)

许可证与对外发布范围待项目负责人确认；第三方资料保留来源和原有许可说明。
