# Apple App Resizability Skill

**从 Xcode 27.1 提取的 App Resizability Agent Skill，方便 iOS 开发者查阅，并用于 AI 编程 Agent。**

[English](README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md)

> **非官方存档。** 本仓库由社区开发者独立维护，与 Apple 无隶属或背书关系。Skill 内容来自 Xcode；转载或修改前请自行确认 Apple 的适用条款。

## 这是什么？

Apple 在 Xcode 中提供了名为 **App Resizability** 的 Skill，用于指导 AI 编程 Agent 迁移旧代码，让 iOS App 能够适应**运行过程中动态变化的窗口尺寸**。

**iPhone Duo 适配**是它的重要使用场景，但并不是唯一场景。它同样涉及分屏多任务、Stage Manager、多窗口、Scene 生命周期以及布局 API 的现代化。

这是一套**写给 AI Agent 的操作指令和迁移参考文档**，不是 SDK 或运行时依赖，也不能仅凭执行这套指令就断定 App 已完成适配。

## 能解决哪些问题？

- 将 `UIScreen.main` / `mainScreen` 等旧式全局屏幕访问方式改为更贴近当前视图或窗口的 API。
- 重新检查依赖 `interfaceOrientation` 和 `userInterfaceIdiom` 的布局判断。
- 检查 Safe Area，特别是不对称安全区域的处理。
- 迁移依赖 Application 生命周期的旧逻辑，正确使用 Scene 生命周期。
- 排查固定屏幕尺寸、单窗口等假设。

Skill 为 **Swift 和 Objective-C** 项目提供具体的检测规则、迁移建议和验证清单。它强调**只修改与目标问题有关的代码**，避免借机进行无关重构。Agent 的修改仍需要代码审查，以及模拟器或真机上的窗口缩放测试。

## 仓库结构

```text
.
├── SKILL.md
└── references/
    ├── uiscreen-task.md
    ├── orientation-task.md
    ├── idiom-task.md
    ├── scene-lifecycle-task.md
    └── safe-area-task.md
```

`SKILL.md` 负责定义触发条件和执行流程；`references/` 保存各项任务的具体规则。**使用时请保留整个目录**，只复制 `SKILL.md` 会丢失它引用的参考文件。

## 如何使用？

**方法一：从本机 Xcode 导出（推荐）**

如果当前安装的 Xcode 支持 Skill 导出，可以运行：

```bash
xcrun agent skills export
```

根据命令输出和你使用的 Agent 文档安装导出的 Skill。这种方式更适合希望 Skill 与当前 Xcode / SDK 版本保持一致的开发者。

**方法二：把仓库复制到 Agent 的 Skill 目录**

例如，将完整目录放到项目内 Codex Skill 的常见位置：

```bash
git clone https://github.com/funsiyuan/apple-app-resizability-skill.git /tmp/apple-app-resizability-skill
mkdir -p .agents/skills/app-resizability
cp -R /tmp/apple-app-resizability-skill/SKILL.md /tmp/apple-app-resizability-skill/references .agents/skills/app-resizability/
```

不同版本、不同 Agent 的 Skill 搜索路径和加载规则可能有所差异，请以相应工具的文档为准。也可以直接让 Agent 阅读 `SKILL.md`，并在执行时参考其中引用的文件。

示例提示词：

> 使用 `app-resizability` Skill 检查当前 iOS 项目。找出屏幕尺寸、方向、Safe Area 和 Scene 生命周期相关的旧式用法，针对窗口动态缩放及 iPhone Duo 提出必要的代码修改，并指出需要人工测试的部分。

## 在 Xcode 中寻找原始文件

Xcode 内部可能包含对应的 `.idechatprompttemplate` 模板，具体路径和格式可能随版本变化。可以在自己的 Xcode 安装目录下搜索：

```bash
find "/Applications/Xcode.app/Contents" -name 'app-resizability.idechatprompttemplate' -o -name 'app-resizability'
```

如果安装的是其他名称或位置的 Xcode，请调整路径。需要可复用的 Skill 时，建议优先使用支持版本提供的官方导出命令。

## 来源与声明

- **文件来源：** Xcode 27.1 自带的 App Resizability Skill。本仓库是一个快照，不会随 Xcode 更新自动同步。
- **Apple 官方文档：** [Preparing your app for iPhone Duo](https://developer.apple.com/documentation/technologyoverviews/preparing-your-app-for-iphone-duo)。
- **非官方项目：** 本仓库不由 Apple 维护。Apple、Xcode 等名称是 Apple Inc. 的商标。
- **版权与许可：** 本仓库不主张拥有 Apple 原始内容的版权，也不意味着获得了额外的再分发授权。使用或转载时请遵守原始文件适用的条款。

欢迎提交文档改进建议，或报告不同 Xcode 版本之间的变化。如果修改了原始 Skill 内容，建议清楚标记差异。
