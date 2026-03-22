# siuser小伟 小红书一键生成 Skill

这是一个单一用途的原创 Skill，用于一键生成小红书图文内容：

- 标题
- 正文
- 标签
- 图片规划
- 每张图的图上文案
- 生图提示词

## 适用场景

- AI 工具推荐
- 产品测评
- 效率方法分享
- 开发者工具介绍
- 方法论拆解
- 清单类内容

## 产出内容

默认一次生成以下内容：

1. 标题候选 3 个
2. 正文 1 篇
3. 标签 4-6 个
4. 配图方案 4-6 张
5. 每张图的 Copy Spec
6. 每张图的 Prompt

## 使用方式

把本目录作为 Skill 安装后，直接对 AI 说：

```text
帮我做一篇关于 AI 工具选型的小红书图文，要包含标题、正文、图片文案和出图提示词
```

或者：

```text
把这个主题直接做成可发的小红书图文内容：远程办公效率工具
```

## Skill 文件

- 核心指令见 [SKILL.md](./SKILL.md)
- 流程说明见 [references/workflow.md](./references/workflow.md)
- 风格规范见 [references/style-guide.md](./references/style-guide.md)
- 输出示例见 [examples/example-output.md](./examples/example-output.md)
