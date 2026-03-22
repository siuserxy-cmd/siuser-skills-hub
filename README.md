# siuser小伟 Skills Hub

一个面向 Codex / Claude Code / 通用 Skill Loader 的本地技能仓库，覆盖产品设计、写作、项目协作，以及小红书内容生成与发布。

## 仓库内容

当前仓库包含 21 个 skills：

- `backlog-manager`
- `design-exploration`
- `git-push`
- `github-repo-search`
- `image-assistant`
- `issue-triage`
- `lesson-builder`
- `memory-init`
- `prd-doc-writer`
- `priority-judge`
- `product-naming`
- `project-map-builder`
- `req-change-workflow`
- `thinking-partner`
- `thought-mining`
- `ui-design`
- `version-planner`
- `vision-exploration`
- `weekly-report`
- `writing-assistant`
- `xhs-content-image-studio`

其中 `xhs-content-image-studio` 位于 [xhs-content-image-studio](./xhs-content-image-studio)，用于：

- 小红书内容生成
- 配图规划与 Copy Spec
- Prompt Pack / APIMart 批量出图
- 浏览器测试、登录检查、内容检索与互动
- 图文自动发布

## 快速安装

克隆整个仓库：

```bash
git clone git@github.com:siuserxy-cmd/siuser-skills-hub.git
cd siuser-skills-hub
```

如果你的 Skill Loader 支持“按目录发现”，直接把需要的子目录复制到 skills 目录即可。

例如安装小红书 skill：

```bash
cp -r xhs-content-image-studio ~/.claude/skills/xhs-content-image-studio
```

## 推荐入口

- 内容生成与配图：见 [xhs-content-image-studio/SKILL.md](./xhs-content-image-studio/SKILL.md)
- 变更控制：见 [req-change-workflow/SKILL.md](./req-change-workflow/SKILL.md)
- 设计探索：见 [design-exploration/SKILL.md](./design-exploration/SKILL.md)
- 写作流程：见 [writing-assistant/SKILL.md](./writing-assistant/SKILL.md)
- GitHub 推送：见 [git-push/SKILL.md](./git-push/SKILL.md)

## 已测试案例

见 [EXAMPLES.md](./EXAMPLES.md)。这次合并额外补充了一个真实案例：

- 公开上传前的来源痕迹扫描与清理

## 合规说明

本仓库包含基于第三方 MIT 项目整理和改造的内容。

- 根仓库第三方说明：见 [THIRD_PARTY_NOTICES.md](./THIRD_PARTY_NOTICES.md)
- 小红书 skill 第三方说明：见 [xhs-content-image-studio/THIRD_PARTY_NOTICES.md](./xhs-content-image-studio/THIRD_PARTY_NOTICES.md)

公开分发时请保留相关许可证与说明文件。
