# Hover-Drone

面向 GPS 拒止环境的树枝停栖无人机项目。目标是通过自研机械臂抓取树枝等圆柱物，实现悬挂停留。团队由**项目总负责人 Air.Zhen** 统筹，下设**机械臂组**和**无人机组**；两组各自维护设计资料、代码和物料清单，具体人员见[分工表](docs/team.md)。

**当前阶段：项目规划与原型归档。** 项目已确定飞控为 **CUAV 雷迅 X25 Evo**、机载计算平台为 **NVIDIA Jetson Orin**。项目计划、职责、接口和实验方案已形成初稿，具体工作见[项目计划](docs/project-plan.md)，成员自行填写[分工表](docs/team.md)。已有机体结构方案、接线图及横杆对准试验代码的归档位置见[现有资料整理说明](docs/maintainer-setup.md#现有资料归档示例)；原始资料尚未批量导入，尚无本仓库版本的实机验证记录。

近期先完成固定标记横杆的识别、对准、触碰与实体闭爪验证，再验证承重、载荷转移与动力管理，逐步推进树枝停栖目标。已有横杆原型采用红色标记与深度观测，其结果不能直接代表自然树枝识别或整机停栖能力。

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
| 机械臂、夹爪的结构、装配、电路与接线 | [robotic-arm/design/](robotic-arm/design/README.md) | 源文件、导出件、适用版本 |
| 机械臂执行控制、开关读取与状态反馈代码 | [robotic-arm/code/](robotic-arm/code/README.md) | 依赖、运行方法、接口与验证结果 |
| 机械臂物料清单 | [robotic-arm/bom.csv](robotic-arm/bom.csv) | 型号、数量、设计版本与状态；本组维护、组负责人核对 |
| 无人机机体、搭载安装、供电与接线 | [drone/design/](drone/design/README.md) | 源文件、导出件、适用版本 |
| 飞控配置、机载视觉、对准与飞行任务代码 | [drone/code/](drone/code/README.md) | 环境、运行方法、输入输出与验证结果 |
| 无人机物料清单 | [drone/bom.csv](drone/bom.csv) | 型号、数量、设计版本与状态；本组维护、组负责人核对 |
| 实验方案、结果与数据索引 | [experiments/](experiments/README.md) | 硬件版本、代码提交、参数与结论 |

成员先按负责部分选择 `robotic-arm/` 或 `drone/`，再把图纸放到 `design/`、代码放到 `code/`。联合试验统一记录在 `experiments/`；两组的安装、供电和控制接口写在 [docs/interfaces.md](docs/interfaces.md)。每项内容只保留一个权威位置，通过链接引用。

```text
Hover-Drone/
├── README.md
├── CONTRIBUTING.md
├── robotic-arm/           机械臂部分
│   ├── design/            结构、夹爪、电路与接线
│   ├── code/              执行控制、开关读取与状态反馈
│   └── bom.csv            本组物料清单
├── drone/                 无人机部分
│   ├── design/            机体、搭载安装、供电与接线
│   ├── code/              飞控配置、机载视觉与飞行任务
│   └── bom.csv            本组物料清单
├── docs/                  项目计划、分工、接口与参考资料
├── experiments/           实验记录与数据索引
└── .github/               任务、问题和 PR 模板
```

## 团队约定

- `main` 保存已检查、可供团队使用的内容；日常修改通过分支和 PR 合入。
- 按机械臂和无人机两部分归档，组内按具体设计或程序建立子目录。
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
