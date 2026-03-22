---
name: siuser-xhs-content-studio
description: |
  小红书内容工作流 Skill：生成科技效率向文案、规划配图、输出生图提示词或批量出图，并在用户确认后发布到小红书。
  适用场景：AI 工具推荐、产品测评、效率方法论、开发者工具分享、行业趋势内容。
  同时支持：测试浏览器、搜索笔记、评论互动、内容数据抓取。
metadata:
  trigger: 小红书文案 + 配图 + 发布
  source: custom/siuser-xhs-content-studio
---

# 小红书文案与配图工作室

你要把一个请求尽量走完，默认支持 5 类工作：

1. 内容生成：根据主题、素材或 URL 生成科技效率向小红书文案
2. 配图规划：决定需要几张图、每张图讲什么、图上逐字写什么
3. 图片生成：输出 Prompt Pack，或用 APIMart 批量出图
4. 自动发布：把最终标题、正文和图片发布到小红书
5. 内容检索与互动：搜索、详情、评论、点赞、收藏、数据抓取

---

## 触发判断

优先按以下顺序判断用户意图：

1. 只有主题、关键词、URL、素材，没有完整文案：进入内容生成流程
2. 已有文案，但没有图片，或明确说“顺便配图 / 生成图片 / 出图提示词”：进入文案 + 配图流程
3. 已有标题、正文、图片：直接进入图文发布流程
4. 已有标题、正文、视频：直接进入视频发布流程
5. 明确说“测试浏览器 / 检查登录 / 获取二维码 / 不要发布”：进入测试浏览器流程
6. 明确说“搜索笔记 / 查看详情 / 评论 / 点赞 / 收藏 / 数据”：进入检索互动流程

如果信息不足，先补齐目标受众、内容角度、是否需要图片、图片来源方式。

---

## 写作规范

基调：理性干货风

- 开头优先用数据、对比、反常识判断切入
- 结构清楚，用分点和短段，不写大段空话
- 专业但不装腔，术语首次出现可补括号解释
- 尽量给数字和场景，不用“很强、很方便、很厉害”这类虚词
- emoji 只在标题或小标题点缀，控制在 `3-5` 个
- 结尾用一句话收束价值，并引导评论互动

禁止：

- “在当今数字化时代”
- “让我们一起来看看”
- “不得不说”
- “真的绝了”
- 家人们、姐妹们等口语称呼

标题公式任选一种：

1. 数字 + 痛点 + 解法
2. 对比冲突
3. 具体场景 + 结果
4. 反常识判断

标签规则：

- 正文最后一行放 `4-6` 个标签
- 第一个标签必须最精准
- 科技效率常用：`#效率工具 #AI工具推荐 #程序员日常 #科技分享 #自动化 #提效神器 #开发者工具 #工具测评 #生产力`

---

## 内容生成流程

当用户给出主题但没有完整文案时：

1. 确认主题、产品、工具名和目标读者
2. 确认角度：测评、教程、对比、清单、趋势、案例
3. 如有 URL，先提取内容；如需参考站内内容，可执行 `search-feeds`
4. 生成：
   - 标题：`<= 38` 个字符预算，中文/中文标点按 2，英文数字按 1
   - 正文：建议 `500-800` 字
   - 标签：`4-6` 个，放最后一行
   - 配图建议：说明每张图展示什么
5. 内容生成后必须先让用户确认，不能直接发布

---

## 配图与图片生成流程

当用户要求“配图 / 生成图片 / 出图提示词 / 直接生成可发图文素材”时：

1. 先读取 `references/image-workflow.md`
2. 默认小红书图文使用 `3:4` 竖版；若用户明确要横版信息图再切 `16:9`
3. 先输出图清单：
   - 需要几张
   - 每张图讲什么
   - 建议版式
4. 图清单确认后，再输出每张图的 Copy Spec：
   - 图类型
   - 文字预算
   - 图上逐字文案
   - 图标或隐喻
5. Copy Spec 确认后，再输出 Prompt Pack
6. 如果用户要“直接出图”，再读取 `references/image-style-guide.md`，生成 `out/apimart.requests.jsonl`
7. 若本地存在 `scripts/apimart.env`，执行：

```bash
python3 scripts/apimart_batch_generate.py \
  --config scripts/apimart.env \
  --input out/apimart.requests.jsonl
```

8. 若用户只要提示词，不调用 API
9. 若用户要发图文，把批量出图得到的本地图片路径作为 `--images` 传给发布脚本

图片生成时必须遵守：

- 一张图只表达一个核心信息
- 不得擅自改掉用户已确认的 Copy Spec
- 除指定文案外，不要让模型额外补字
- 如果没有 `scripts/apimart.env` 或 API 不可用，退回“输出提示词 + JSONL 请求包”

## 测试浏览器流程

1. 启动测试浏览器，默认有窗口
2. 如用户要求静默运行，再使用无头模式
3. 可选执行登录状态检查
4. 若用户要求，返回二维码或关闭浏览器

## 图文发布流程

1. 最终确认标题、正文、标签和图片
2. 如需文件输入，先写 `title.txt` 与 `content.txt`
3. 图文发布必须有图片；没有图片不得发布
4. 默认无头模式；若未登录，切换有窗口模式登录
5. 成功后回传关键信息

## 视频发布流程

1. 最终确认标题、正文和视频
2. 视频和图片不能混用
3. 视频发布前要确认文件路径可访问
4. 成功后回传关键信息

## 检索互动流程

1. 先检查小红书主页登录状态
2. 首页推荐流：`list-feeds`
3. 关键词搜索：`search-feeds`
4. 详情：从搜索结果里拿 `feed_id + xsec_token` 再跑 `get-feed-detail`
5. 评论：`post-comment-to-feed`
6. 回复评论：`respond-comment`
7. 点赞/收藏：`note-upvote` / `note-bookmark` 等
8. 用户主页：`profile-snapshot` / `notes-from-profile`
9. 通知：`get-notification-mentions`
10. 数据：`content-data`

---

## 必做约束

- 内容生成后必须让用户确认，不能直接发布
- 图片生成前必须先确认图清单或至少确认整体方向
- 批量 API 出图前，至少先给出 Prompt Pack 或说明将直接执行
- 发布前必须确认最终标题、正文和图片/视频
- 图文必须有图片，视频必须有视频，二者不能混用
- 文件路径优先使用绝对路径；相对路径先转换成绝对路径
- 如果发布页异常，优先检查 `scripts/cdp_publish.py` 中的选择器、上传等待和发布按钮逻辑

## 常用命令

### 参数顺序提醒

全局参数放在子命令前，子命令参数放在子命令后，避免 `unrecognized arguments`。

示例（正确）：

```bash
python scripts/cdp_publish.py --reuse-existing-tab search-feeds --keyword "AI工具" --sort-by 最新 --note-type 图文
```

### 测试浏览器

```bash
python scripts/chrome_launcher.py
python scripts/chrome_launcher.py --headless
python scripts/cdp_publish.py check-login
python scripts/cdp_publish.py --reuse-existing-tab check-login
python scripts/cdp_publish.py --host 10.0.0.12 --port 9222 check-login
python scripts/cdp_publish.py get-login-qrcode
python scripts/chrome_launcher.py --restart
python scripts/chrome_launcher.py --kill
```

### 发布

```bash
printf '%s\n' '这里是标题' > /abs/path/title.txt
printf '%s\n' '这里是正文' > /abs/path/content.txt

python scripts/publish_pipeline.py --headless \
  --title-file /abs/path/title.txt \
  --content-file /abs/path/content.txt \
  --image-urls "https://example.com/1.jpg" "https://example.com/2.jpg"

python scripts/publish_pipeline.py --headless \
  --title-file /abs/path/title.txt \
  --content-file /abs/path/content.txt \
  --images "/abs/path/pic1.jpg" "/abs/path/pic2.jpg"
```

### 批量出图

```bash
python3 scripts/apimart_batch_generate.py \
  --config scripts/apimart.env \
  --input out/apimart.requests.jsonl

python3 scripts/apimart_batch_generate.py \
  --config scripts/apimart.env \
  --input out/apimart.requests.jsonl \
  --dry-run
```

### 检索互动

```bash
python scripts/cdp_publish.py list-feeds
python scripts/cdp_publish.py search-feeds --keyword "AI工具"
python scripts/cdp_publish.py get-feed-detail --feed-id FEED_ID --xsec-token XSEC_TOKEN
python scripts/cdp_publish.py post-comment-to-feed --feed-id FEED_ID --xsec-token XSEC_TOKEN --content "评论内容"
python scripts/cdp_publish.py content-data
```

## 失败处理

- 登录失败：提示重新扫码；远程场景可用 `get-login-qrcode`
- 图片 API 不可用：退回提示词与 JSONL 请求包
- 图片或视频下载失败：改用本地文件或替换 URL
- 本地路径不可用：优先改用绝对路径；WSL 场景可加 `--skip-file-check`
- 评论/回复目标未定位：补充 `comment_id`，或改用作者/片段定位
- 页面选择器失效：检查 `scripts/cdp_publish.py` 中的 `SELECTORS`
- 文案不满意：根据反馈调整角度、风格、详略后重生成
