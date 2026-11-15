# 我的 Agent Skill 备份：Anatomy + Machine

归档日期：2026-11-14 ｜ 账号：`WU7475` ｜ 用途：个人备份 + 上手速查

这个仓库把两个已安装到 WorkBuddy 的 Agent Skill 原样备份一份，防止本地技能目录被覆盖或丢失。两个项目都是 **MIT 许可**，LICENSE 原文件随各目录保留。

---

## 一、里面是什么

| 目录 | 技能名（调用时用） | 上游仓库 | 固定版本 | 文件 |
|---|---|---|---|---|
| `anatomy/` | `anatomy` | wheresryan22/anatomy | `1482e8469b0a9e0d8cc18ea2740a01423d29ae80` | 173 个 / 26.4 MB |
| `machine/` | `machine` | AirsupHQ/machine-skill | `e0bd066ac5beaf48512935de2fc0f0e55636853a` | 53 个 / 10.1 MB |
| `demo/` | — | 从上面两个 examples 挑出、可直接打开 | — | 3 个 HTML |

一句话区分：

- **anatomy** = 把抽象概念做成一台**等轴测（2.5D）精密仪器**的 SVG 插画，用来解释算法、机制、原理。适合写博客、课件、产品文档里的交互插图。输出是独立 HTML / SVG / React 组件，**零外部依赖**。
- **machine** = 把一句话变成**影棚级 3D 交互仿真**的单文件 HTML（Three.js），带真实尺寸、PBR 材质、剖切、爆炸视图、X 光、实时物理求解和 28 秒自动运镜。适合拆解真实硬件。

> 名字提醒：上游仓库叫 `machine-skill`，但安装后技能名是 **`machine`**（SKILL.md 里 `name: machine`）。在本机调用要写 `@skill:machine`，不是 `@skill:machine-skill`。

---

## 二、先看效果（不用装任何东西）

`demo/` 里三个 HTML 双击就能在浏览器打开：

| 文件 | 看什么 | 是否需要联网 |
|---|---|---|
| `anatomy-百分表-示例.html` | 等轴测百分表：滚花旋钮、弹簧前后遮挡、指针与实时读数联动 | 否，完全离线 |
| `anatomy-百分表-浅色.html` | 同上，浅色主题 | 否，完全离线 |
| `machine-涡扇发动机-3D.html` | 涡扇发动机 3D 数字孪生：鼠标拖动旋转、剖切、爆炸、X 光 | **是**，需加载 jsDelivr 的 three.js 与 Google Fonts |

想自己重新生成 anatomy 那个示例：

```bash
node anatomy/examples/dial-indicator/build.mjs
```

需要 Node.js 20.10+（本机 22.22.2）。更多完整示例在 `anatomy/examples/`（街机、猛禽发动机、桌面电脑、水波槽等）和 `machine/examples/`（涡扇、火箭发动机、压缩机、炼厂泵）。

---

## 三、在 WorkBuddy 里怎么调用（重点）

技能已经装在 `~/.workbuddy/skills/`，**不用每次安装**。有两种方式：

1. **显式点名**（推荐，最稳）：在对话里写 `@skill:anatomy` 或 `@skill:machine`，后面跟需求。
2. **自然语言**：直接说「画一张等轴测图解解释 XX」「做一台 XX 的 3D 仿真」，技能描述会自动匹配。

### 可直接复制的提示词

**Anatomy —— 解释抽象概念（照文章里的经典例子）**

```
@skill:anatomy 用一台等轴测精密仪器解释「令牌桶限流」：桶容量 10 个令牌、补充速率 2 个/秒、突发请求触发限流。要求可交互、浅色主题、输出单个独立 HTML 文件。
```

**Machine —— 拆解真实硬件**

```
@skill:machine 做一台行星滚柱丝杠执行器的 3D 交互仿真：8 根螺纹滚柱、铜定子绕组、钛合金壳体；要实时剖切平面、同轴/径向爆炸视图、X 光，以及过载热抱死的 Break-It 模式。输出单个 HTML。
```

**贴自己业务的用法（采购 / 供应链培训）**

```
@skill:anatomy 用一台等轴测精密仪表解释「安全库存与再订货点」：日均耗用 20 件、提前期 7 天、安全库存 60 件，做一个可拖动滑块看库存下降与触发补货的独立 HTML，浅色主题，适合放进培训课件。
```

```
@skill:machine 做一台自动化立体仓库堆垛机的 3D 交互仿真：真实尺寸、金属货架与立柱、可剖切看提升机构、爆炸视图、X 光，并给出由运行速度计算出的吞吐量和实时读数。
```

> 写提示词的诀窍（文章里的三条法则）：
> - Anatomy **只说概念和真实参数，别替它设计方框**，让它自己发明对应的物理机器。
> - Machine **点名硬件名称、剖切位置、过载失效边界**，物理数值要能由求解器算出来。
> - 两者都别要求「画一张图」——它们输出的是可运行、可交互的代码，不是位图。

### 规模与耗时（Anatomy 自带分级）

| 级别 | 零件数 | 说明 |
|---|---|---|
| Figure | 40–80 | 单一机构、SVG、最多一层 WebGL，直接做 |
| Hero | 80–150 | 多层 WebGL 或 3D 主体，约 2–4 小时，会先告知 |
| Epic | 更多 | 整车 / 整厂，6 小时以上，先给估算并建议缩成 Hero，不直接开工 |

### 已知限制

- Machine 生成的页面运行时从 `cdn.jsdelivr.net`、`fonts.googleapis.com` 取 three.js 与字体；**离线时 3D 可能不显示**。Anatomy 生成的页面无此问题。
- 截图与几何审计脚本需要本机装 Chrome / Chromium。
- 上游项目主要为 Claude Code 编写，在其他 Agent（含 WorkBuddy）里能读 SKILL.md 和跑脚本，但质量校验流程（audit / lines / capture）需要手动执行，效果不保证与官方画廊完全一致。文章里的 Demo 也是用 Gemini 生成的，作者已说明效果偏弱，要看真实水准请以上游官方 Demo 为准。

---

## 四、换电脑时怎么恢复

**方式 A：从这个仓库复制回来**（最稳，版本固定）

把 `anatomy/` 和 `machine/` 两个目录整个复制到 `~/.workbuddy/skills/` 下，保持目录名与 SKILL.md 里的 `name` 一致即可。WorkBuddy 会立即识别，**不需要重启**。

**方式 B：装上游最新版**

```bash
npx skills add wheresryan22/anatomy -g
npx skills add AirsupHQ/machine-skill -g
```

---

## 五、参考

- 入门文章（2026-10-16，BeforeAGI）：《告别「方框加箭头」：用 Anatomy 与 Machine-Skill，把一句话变成可交互的精密机械与 3D 数字孪生》 https://mp.weixin.qq.com/s/Al84ITpXR8RIQqi9zJ5hCw
- 上游仓库：https://github.com/wheresryan22/anatomy ｜ https://github.com/AirsupHQ/machine-skill
- 本机安全审查报告：`downloads/技能安装与安全审查报告.md`（工作区内，不随本仓库上传）

> 本仓库是**备份**，不是上游项目。请遵守各自 MIT 许可，保留 LICENSE 与版权声明。
