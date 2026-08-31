---
name: photo-to-print-poem
description: 将用户提供的照片转化为暖白纸张、有限色板和手工印刷颗粒感的极简诗意版画，并按需加入短句。适用于照片版画化、risograph、monoprint、诗集或独立杂志插画；不用于普通照片增强、写实修复或仅添加滤镜。
license: MIT
---

# Photo to Print Poem

把照片重新提炼成具有明显留白、纸张触感和手工套色误差的编辑型艺术版画。保留原图可识别的主体、姿态、透视与视觉锚点，但主动舍弃摄影级细节。

## 输入判断

先为每张输入图片标注角色：

- **目标照片**：需要被转换的主体图。
- **风格参考**：只提供色彩、纸张、构图或印刷语言，不复制其人物和场景。
- **辅助参考**：用于服装、物件或局部细节。

只有一张目标照片时直接执行。多图时遵守以下边界：

- 一张目标照片加若干参考图：只转换目标照片，并在生成指令中逐张写明角色。
- 用户明确要求批量：逐张独立生成，不混合不同照片的主体和场景。
- 存在多张可能的目标照片，且用户未要求批量、无法可靠判断主图：先询问一次。
- 没有可用目标照片：请用户上传；不要凭空虚构其本人或特定照片内容。

## 默认结果

- 输出一张最终艺术图，不输出提示词、过程图、相框 mockup 或前后对比拼图。
- 默认 3:4 竖版；用户指定比例时覆盖默认值。
- 暖白或米白纸张，大面积留白，3–6 色有限色板。
- 主体通常占画布约 35%–65%，根据原图构图自适应，不为满足比例破坏识别度。
- 默认可加入一行 4–10 个英文单词的克制短句；用户要求无文字时完全移除。
- 用户给出文案时逐字使用，不改写、不增加署名或其他文字。

## 执行流程

1. 识别主体类型、最重要的 1–3 个视觉锚点、1–2 个强调色，以及适合保留留白的方向。
2. 把用户要求整理成结构化图像编辑指令，明确目标照片、每张参考图的角色、必须保留项和禁止项。
3. 调用当前环境可用的图像生成或编辑工具完成转换；在 Codex 中优先使用内置图像生成工具。除非用户明确只要提示词，否则不要停在提示词阶段。
4. 检查主体识别度、风格转换、色板、留白、文字准确性和额外元素。
5. 若关键约束失败，只做一次针对性重试，并重复说明必须保留项。仍失败时交付较好的版本并坦诚说明限制，不连续盲目重试。

## 视觉语言

综合使用而非机械堆砌这些特征：

- warm off-white textured paper
- minimalist poetic editorial print
- risograph, monoprint or screenprint texture
- distressed ink grain and imperfect coverage
- muted limited palette with one or two accents
- simplified hand-printed shapes
- generous negative space
- calm, quiet, contemplative mood

避免摄影写实、光滑数字绘画、3D 渲染、干净矢量感、粗重描边、过度精细的五官，以及与原图无关的新人物、物件或文字。

## 按主体调整

### 人像

- 锁定人物身份特征、姿势、头部朝向、手势、发型和服装轮廓。
- 用克制的块面和颗粒表达五官，不把人物换成另一张脸。
- 背景只保留最能说明场景的少量结构；必要时用单色横条、窗框线或几何块作视觉锚点。

### 城市与街景

- 保留透视与最有识别度的建筑、车辆或路标轮廓。
- 将人群和车辆简化为小块面，但不改变关键物体的位置关系。
- 优先保留原图最强的高饱和色，其余颜色统一到灰、米白、炭黑、赭黄等低饱和色系。

### 风景、水面与静物

- 保留水面、树、墙体、建筑或桌面物件的大结构。
- 用浅灰蓝、灰绿或淡青的颗粒刷印表达水面和空气感，不模拟摄影级反光。
- 允许画面边缘呈现自然缺墨，不做生硬的规则矩形印刷边界。

## 色彩

默认以暖白纸张、炭黑、米灰为基础，从原图选择赭黄、砖红、灰蓝或淡青等一至两个强调色。参考色值只用于方向判断，不要求生成工具机械匹配：

- 暖白纸张：约 `#F3EAD8`
- 炭黑或深灰
- 石灰或米灰
- 赭黄或芥末黄
- 暗红或砖红
- 灰蓝或淡青

不要让所有颜色同时鲜艳。

## 文案

未提供文案而且用户没有要求无文字时，可先生成一句与画面相关的短句，再把它作为逐字文本写入生成指令：

- 英文：4–10 个单词，通常使用小写。
- 中文：用户明确要求中文时使用 6–18 个汉字。
- 语气安静、具体、克制，避免与画面无关的宏大表达和陈词滥调。
- 使用小号 serif 或 typewriter-like 字体，放在底部中央或左下，并避开主体。
- 明确要求只出现给定短句，不添加水印、签名或其他字符。

如果文字拼写是关键约束，检查逐字准确性；失败时只针对文字做一次重试。若用户优先要画面而非文字，宁可交付无字版本，也不要交付乱码。

## 结构化生成指令

根据输入动态填写，不向用户展示，除非用户明确要求提示词：

```text
Use case: style-transfer
Asset type: minimalist editorial art print
Input images: Image 1 is the edit target; Image 2... are style references only.
Primary request: reinterpret the target photo as a poetic hand-printed illustration on warm off-white textured paper.
Preserve: subject identity, pose, silhouette, perspective, composition, and the 1–3 named visual anchors.
Style/medium: risograph / monoprint / screenprint, distressed grain, subtle paper fibers, imperfect ink coverage, simplified flat shapes.
Composition: generous intentional negative space; irregular softly distressed printed edges.
Color palette: 3–6 muted colors; preserve the named accent color(s); reduce the rest toward charcoal, warm gray, beige, muted ochre, dusty red, pale blue-gray or pale aqua.
Text (verbatim): "[CAPTION]" or no text.
Constraints: change the rendering style only; keep the target subject and spatial relationships recognizable; use references only for visual language.
Avoid: photorealism, glossy digital painting, 3D, clean vector art, heavy outlines, detailed facial rendering, extra people or objects, watermark, mockup, frame, before/after split screen.
Output: only the final stylized artwork.
```

## 用户覆盖规则

自然语言要求始终优先于默认值：

- “更像版画” → 增强颗粒、缺墨和块面。
- “更像水彩” → 保留纸张与留白，减少硬块面，加入轻淡水洗。
- “人物更像原图” → 加强身份、脸型、发型、姿态和服装轮廓锁定，不恢复摄影写实质感。
- “留白更多” → 适当缩小主体，同时保持识别度。
- “不要文字”“不要英文”“不要自动文案” → 完全无文字。
- “用中文” → 使用中文短句。
- “做成 1:1 / 9:16 / 保持原比例” → 使用用户指定画幅。

## 交付检查

- 主体、姿势、透视与关键锚点是否仍可辨认？
- 是否真正成为手工印刷插画，而非简单滤镜？
- 暖白纸张、有限色板、颗粒和留白是否成立？
- 风格参考是否只影响视觉语言，没有替换目标主体？
- 是否没有凭空新增人物、物件、水印、边框、mockup 或前后拼图？
- 若有文字，是否逐字准确、足够克制且不遮挡主体？
- 是否只交付最终艺术图，符合用户指定的数量与比例？
