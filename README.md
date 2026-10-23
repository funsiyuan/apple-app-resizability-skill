# Apple App Resizability Skill

**An App Resizability agent skill extracted from Xcode 27.1, preserved here for reference and use with AI coding agents.**

[English](README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md)

> **Unofficial archive.** This repository is maintained independently and is not affiliated with or endorsed by Apple. The skill content originates from Xcode; check Apple's applicable terms before redistributing or adapting it.

## What is this?

Apple includes an **App Resizability** skill in Xcode to help coding agents modernize iOS app code for **windows that can change size while the app is running**. Preparing for **iPhone Duo** is one of its use cases, but the guidance is broader: it also covers split-screen multitasking, Stage Manager, scene-based lifecycle, and context-aware layout APIs.

This is a **set of instructions and migration references for an AI coding agent**, not an SDK, a runtime library, or a guarantee that an app is compatible with a particular device.

## What does it cover?

- Replace legacy `UIScreen.main` / `mainScreen` usage with context-appropriate APIs.
- Revisit interface orientation and `userInterfaceIdiom` assumptions.
- Review safe-area handling, including asymmetric insets.
- Migrate application-lifecycle assumptions toward scene-based behavior.
- Audit code that assumes a fixed screen size or a single window.

The skill contains task-specific detection patterns, migration guidance, and validation checklists for **Swift and Objective-C**. Its stated goal is *targeted modernization* rather than broad refactoring. Always review proposed code changes and test resizing behavior on real devices or simulators.

## Repository contents

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

`SKILL.md` defines when and how the agent should perform the migration. `references/` contains the detailed task instructions it refers to. Keep the directory together; copying `SKILL.md` alone will omit those references.

## Use it with a coding agent

**Option 1 — Export from your installed Xcode (recommended).** If your Xcode version supports agent skill export:

```bash
xcrun agent skills export
```

Check the command's output/help and install the exported skill according to your agent's instructions. This is preferable when you want the skill that matches your current Xcode/SDK version.

**Option 2 — Copy this repository into your agent's skill directory.** For example, to add the complete folder to a project's Codex skills location:

```bash
git clone https://github.com/funsiyuan/apple-app-resizability-skill.git /tmp/apple-app-resizability-skill
mkdir -p .agents/skills/app-resizability
cp -R /tmp/apple-app-resizability-skill/SKILL.md /tmp/apple-app-resizability-skill/references .agents/skills/app-resizability/
```

The target directory and skill discovery rules vary between agent versions and tools. Consult your agent's current documentation. You can also simply point your agent at `SKILL.md` and ask it to follow the linked task references.

Example request:

> Review this iOS project using the `app-resizability` skill. Identify legacy screen, orientation, safe-area, and scene-lifecycle assumptions. Propose targeted changes for resizable windows and iPhone Duo, and explain anything that requires manual testing.

## Finding the skill in Xcode

The original Xcode-specific prompt template may be bundled in Xcode's internal resources. Its location and file format can change between releases. If you want to inspect your installation, search within the Xcode app you have installed:

```bash
find "/Applications/Xcode.app/Contents" -name 'app-resizability.idechatprompttemplate' -o -name 'app-resizability'
```

If Xcode has a different app name or location, adjust the path. For a reusable skill package, prefer the official `xcrun agent skills export` command when available.

## Source and caveats

- **Source:** App Resizability skill bundled with **Xcode 27.1** (repository snapshot; not automatically updated when Apple releases new Xcode versions).
- **Official background:** [Preparing your app for iPhone Duo](https://developer.apple.com/documentation/technologyoverviews/preparing-your-app-for-iphone-duo).
- **Not an Apple-maintained project.** Apple and Xcode are trademarks of Apple Inc.
- **No independent license grant is implied.** This repository does not claim ownership of Apple's source content or confer permission to redistribute it. Consult the terms that apply to the original files.

Contributions that improve the documentation or report differences across Xcode releases are welcome. Please avoid altering the upstream skill content without documenting the changes.
