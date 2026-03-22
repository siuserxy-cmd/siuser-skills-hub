# 配图风格与批量出图

## 默认视觉风格

- 风格基准：奶油色纸张底 + 彩铅线稿 + 淡水彩上色
- 氛围：趣味但干净，允许轻涂鸦装饰
- 可读性：中文必须清晰可读，大字号、少字、短句
- 负面约束：不要扁平矢量海报风、不要 3D、不要摄影写实、不要英文乱码、不要水印、不要额外小字

## Prompt 必带约束

- 明确画幅：小红书默认 `3:4`
- 明确用途：封面 / 内容图 / 信息图
- 明确版式：封面、双卡、三卡、路线图、通用信息图
- 明确“只允许出现指定文字”
- 明确“除指定文案外不要自动补字”

## APIMart 配置

真实密钥只放本地文件 `scripts/apimart.env`，不要写进聊天记录或仓库。

配置示例：

```text
API_URL=https://api.apimart.ai/v1/images/generations
MODEL=gemini-3-pro-image-preview
TOKEN=<YOUR_TOKEN>
RESOLUTION=2K
SIZE=3:4
N=1
PAD_URL=
```

## JSONL 请求包

一行一张图：

```jsonl
{"id":"01","prompt":"<PROMPT_CONTENT>","size":"3:4","n":1,"resolution":"2K","model":"gemini-3-pro-image-preview","pad_url":""}
```

## 批量出图命令

正常执行：

```bash
python3 scripts/apimart_batch_generate.py \
  --config scripts/apimart.env \
  --input out/apimart.requests.jsonl
```

本地 dry-run：

```bash
python3 scripts/apimart_batch_generate.py \
  --config scripts/apimart.env \
  --input out/apimart.requests.jsonl \
  --dry-run
```

## 出图后的下一步

- 如果用户只要提示词，到此结束
- 如果用户要直接发小红书，把生成后的本地图片路径传给 `publish_pipeline.py --images`
- 发布前仍然必须让用户确认最终标题、正文和图片
