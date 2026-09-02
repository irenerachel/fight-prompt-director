# Fight Prompt Director

中文名：动作导演Skill

一个面向 AI 视频生成的动作戏提示词编排 Skill。它把角色、场景、图片、首尾帧或分镜素材整理成可直接使用的视频提示词，重点解决动作因果、角色差异、空间方向、镜头职责和跨镜连续性。

English summary: A bilingual prompt-directing skill for AI-generated fight and action scenes. It turns text, images, keyframes, or storyboard references into executable video prompts with readable action, spatial continuity, camera intent, and character-specific movement.

## 核心能力

- 为每名角色设计不同的优势距离、移动方式、攻击习惯和反制策略
- 将每轮交锋写成完整因果链，覆盖发起、接触、回应、位移和下一拍接口
- 支持近战、远攻、混合距离、人兽战、单人演武和风格化材质动作
- 为 Seedance 生成连续的粗时间线
- 为 MiniMax-H3 生成首尾衔接的精确时间码
- 输出高强度、慢节奏和中间型三套动作方案
- 提供标准版与极速版两档速度，极速版让三套强度方案同时变快
- 管理角色数量、武器、弹药、运动方向和场景损伤的跨镜连续性
- 提供动作、镜头和参考素材三类问题的单变量排错方法

## What it does

- Builds a distinct movement profile for every character
- Converts exchanges into visible cause-and-effect action
- Supports melee, ranged combat, mixed-distance encounters, creature fights, solo forms, and crafted visual styles
- Uses coarse continuous time blocks for Seedance
- Uses contiguous precise timecodes for MiniMax-H3
- Produces high-intensity, measured, and hybrid versions
- Offers a standard and an ultra-fast speed setting, where ultra-fast speeds up all three versions
- Tracks position, facing, weapons, projectiles, motion, and environmental damage across cuts
- Diagnoses failures by changing one variable group at a time

## 为什么动作提示词需要导演结构

动作视频同时依赖角色一致性、肢体关系、接触位置、屏幕方向、镜头衔接、道具稳定和环境记忆。只有“快速打斗”之类的概括性描述时，模型往往会缩减动作、隐藏接触或重置空间。

这个 Skill 会把动作拆成能够观察的关系：

1. 谁先发起
2. 动作指向哪里
3. 接触、格挡或落空
4. 对方怎样回应
5. 双方位置怎样变化
6. 什么状态让下一拍成立

它同时要求重要命中交代加速、接触、受力、位移和环境反馈，让力量通过画面结果呈现。

## Why director-style prompting helps

Action scenes ask a video model to maintain identity, body logic, contact, screen direction, props, camera continuity, and environmental memory at the same time. Vague intensity words often lead to reduced motion, hidden contact, or spatial resets.

This skill turns action into observable relationships. Each exchange identifies the initiator, target, contact outcome, reaction, displacement, and the state that enables the next beat.

## 支持的输入

- 纯文字剧情
- 单张角色或场景图
- 图片加文字要求
- 首帧图生视频
- 首尾帧生成
- 分镜图或故事板
- 动作参考视频
- 运镜参考视频

参考素材会按职责拆分。角色图锁定身份，场景图锁定空间，动作视频只控制身体轨迹，运镜视频只控制摄影机路径。

## Supported inputs

- Text-only scene briefs
- Character or environment images
- Image plus written constraints
- First-frame image-to-video
- First-and-last-frame generation
- Storyboards and shot boards
- Movement reference videos
- Camera reference videos

## 七类动作结构

| 类型 | 适合场景 | 主要问题 |
|---|---|---|
| 单人突围 | 角色穿过机关、环境威胁或追击区域 | 前进目标和障碍升级 |
| 双人攻防 | 两名角色近战、远攻或混合距离交锋 | 主动权与距离变化 |
| 远程能量对抗 | 法术、装置或能量路径对抗 | 释放逻辑和作用结果 |
| 一人对多人 | 主角面对多个主动威胁 | 敌人区分和倒地累积 |
| 人兽对决 | 体型与力量差明显 | 高度变化和受力尺度 |
| 单人演武 | 无对手的动作、兵器或能力展示 | 动作闭环和空间落点 |
| 风格化材质动作 | 剪纸、刺绣、木偶、像素等 | 媒介物理和机位限制 |

每一类都在 [`references/fight-design.md`](references/fight-design.md) 中提供一个全新虚构示例。示例用于解释结构，不绑定任何知名角色、品牌或外部案例。

## 输出方式

默认输出三套完整提示词：

1. 高强度版，强调连续交锋、快速位移与动作匹配切
2. 慢节奏版，强调完整发力过程、环境声音与长镜头
3. 中间型版，兼顾交锋密度和动作中的表情、手部或武器特写

用户指定单一强度时，只输出对应版本。

速度是独立于强度的一档参数。默认标准版；选择极速版时，三套方案会使用更短的时间块、更多的微动作、更快的起手和更短的特写，但仍然只输出这三套方案。

Speed is a separate axis from intensity. The default is standard; the ultra-fast setting shortens time blocks, adds micro-actions, starts the first effective action sooner, and cuts close-ups shorter across all three versions, without adding a fourth version.

## 安装

### Codex

```bash
git clone https://github.com/irenerachel/fight-prompt-director.git ~/.codex/skills/fight-prompt-director
```

### Claude Code

```bash
git clone https://github.com/irenerachel/fight-prompt-director.git ~/.claude/skills/fight-prompt-director
```

如果目标目录已经存在，请先自行备份或选择新的目录，避免覆盖已有修改。

## 使用示例

安装后，可以直接提出动作视频需求：

```text
使用动作导演Skill，把这张角色图编排成 15 秒 Seedance 双人近战。两人使用不同动作风格，结尾不要分出胜负。
```

```text
使用动作导演Skill，为弓手和持盾者设计一场 12 秒混合距离战斗。弓手需要换位和管理箭矢，持盾者通过掩体逼近。
```

```text
Use Fight Prompt Director to create a 10-second creature encounter for MiniMax-H3. Keep the camera low, preserve screen direction, and make the final impact readable.
```

## 目录结构

```text
fight-prompt-director/
├── SKILL.md
├── README.md
├── LICENSE
└── references/
    ├── fight-design.md
    ├── camera-guide.md
    └── diagnostics.md
```

- `SKILL.md`：入口、路由、输出规则和内容边界
- `fight-design.md`：七类动作结构与原创示例
- `camera-guide.md`：镜头职责、镜头语法和强度搭配
- `diagnostics.md`：失败诊断与单变量修正

## 设计原则

- 先明确完成条件，再决定攻防选择
- 每名角色拥有独立动作档案
- 每次攻击都需要回应和位置结果
- 每个镜头只承担一个主要职责
- 切镜继承位置、方向、速度、武器和场景状态
- 参考素材按职责拆分
- 示例保持虚构、通用和可替换

## Design principles

- Define a visible completion condition before choreographing
- Give every character a distinct movement profile
- Pair every attack with a response and a spatial result
- Assign one primary responsibility to each shot
- Carry position, direction, speed, weapons, and damage across cuts
- Separate the responsibilities of reference assets
- Keep examples fictional, generic, and replaceable

## 模型兼容性

本项目针对 Seedance 和 MiniMax-H3 提供不同时间组织方式，也可以作为其他文生视频或图生视频模型的通用动作编排框架。具体模型对时间码、负面描述、参考素材和声音字段的支持程度可能不同，使用时请根据目标模型调整。

## Model compatibility

The repository includes separate timing conventions for Seedance and MiniMax-H3. The choreography framework can also be adapted to other text-to-video and image-to-video systems. Timing, negative instructions, reference assets, and audio fields may behave differently across models.

## 原创与隐私说明

仓库中的示例场景均为本项目公开版重新设计的虚构内容。仓库不包含私人文章链接、个人资料、本地路径、内部素材或第三方完整提示词。

## Originality and privacy

All example scenarios in this public edition were newly written as fictional demonstrations. The repository contains no private article links, personal profile data, local paths, internal assets, or complete third-party prompts.

## 非官方声明

本项目是独立的提示词工作流，与 Seedance、MiniMax 及其他相关平台没有隶属、授权或背书关系。产品名称仅用于说明兼容性。

## Unofficial project notice

This is an independent prompt workflow. It is not affiliated with, authorized by, or endorsed by Seedance, MiniMax, or any other referenced platform. Product names are used only to describe compatibility.

## License

MIT License. See [`LICENSE`](LICENSE).
