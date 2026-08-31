# Photo to Print Poem 测试报告

测试日期：2026-08-31  
执行环境：Codex 内置图像生成工具  
Skill 路径：`photo-to-print-poem/`

## 结论

规范校验和四项真实图片测试全部通过。所有生成结果均为 1086 × 1448 像素（3:4），没有进行失败重试。

## 规范校验

| 检查 | 结果 |
| --- | --- |
| `quick_validate.py` | 通过：`Skill is valid!` |
| `agents/openai.yaml` YAML 解析 | 通过 |
| 必需文件与相对资源路径 | 通过 |
| 自动调用策略 | 保持启用 |

## 测试素材

测试只使用 Wikimedia Commons 上许可信息明确的公共领域摄影作品。

| 类型 | 作品与作者 | 许可 | 原作页面 | 本地文件 |
| --- | --- | --- | --- | --- |
| 人像 | *Portrait photograph of an unidentified woman*，Arnold Genthe | Public domain | [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Portrait_photograph_of_an_unidentified_woman_LOC_agc.7a10272.jpg) | `source-images/portrait.jpg` |
| 街景 | *Street scene (...), 1918*，National Photo Company Collection | Public domain | [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Street_scene_%28...%29,_1918_LCCN2016827087.jpg) | `source-images/street.jpg` |
| 风景 | *Private lake*，Prindleman | Public domain，作者主动释出 | [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Private_lake.jpg) | `source-images/landscape.jpg` |

## 测试 1：人像身份与姿态保持

用户级请求：

> 保留人物帽子、侧脸、卷发和肩部轮廓，留白更多，不要文字，输出 3:4。

验收结果：

- 同一人物的侧向头部、帽子、卷发和暗色肩部轮廓可辨认。
- 暖白纸张、颗粒、缺墨与有限色板成立。
- 留白明显，无文字、水印、边框或额外物件。

结果：**通过**

![人像测试结果](results/portrait-result.png)

## 测试 2：复杂街景简化与精确文字

用户级请求：

> 保留左侧路灯、中央人群和建筑透视，使用砖红强调色，底部加入小字 “the city moves in soft ink.”。

验收结果：

- 路灯、人群层次和建筑窗格透视得到保留。
- 密集人物被简化为印刷块面，但仍保持原有街流关系。
- 指定英文完整、大小写和句号准确，无额外字符。
- 画面没有 mockup、拼图或新增车辆。

结果：**通过**

![街景测试结果](results/street-result.png)

## 测试 3：风景大结构与强调色

用户级请求：

> 保留蓝灰湖面、V 形山谷、花岗岩山峰、远岸松树和前景石块，不要文字。

验收结果：

- 湖面、山谷、山峰、树带和前景石块的空间关系可辨认。
- 湖面成为唯一强蓝灰强调色，其余颜色受控。
- 纸张、颗粒、印刷边缘与顶部呼吸区成立。
- 没有新增人物、船、建筑或装饰符号。

结果：**通过**

![风景测试结果](results/landscape-result.png)

## 测试 4：目标图与风格参考图隔离

输入角色：

- Image 1：人像目标图。
- Image 2：风景色彩与安静氛围参考，只允许使用蓝灰、松绿、花岗岩灰和情绪。

验收结果：

- 最终主体仍是 Image 1 中的人物，身份、姿态、帽子与轮廓得到保留。
- Image 2 只影响色板和氛围。
- 没有把山、湖、树或岩石复制进人像画面。

结果：**通过**

![多图角色隔离测试结果](results/multi-image-role-result.png)

## 总体判断

Skill 已满足以下关键目标：

- 可安装格式规范。
- 能实际调用图像工具，而非只返回提示词。
- 能保留人像、城市和风景的关键结构。
- 用户关于比例、文字、留白与强调色的覆盖指令有效。
- 多图场景中可明确区分目标图和风格参考图。
- 对关键失败最多进行一次针对性重试，避免无边界生成。
