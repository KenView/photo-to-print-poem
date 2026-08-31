# Photo to Print Poem 测试报告

测试日期：2026-08-31  
执行环境：Codex 内置图像生成工具  
安装 Skill：`photo-to-print-poem/`

## 结论

格式校验、6 组原图/效果图 Demo 和 1 项多图角色隔离测试均通过。6 张主效果图均为约 3:4 的竖版页面。

## 规范校验

| 检查 | 结果 |
| --- | --- |
| `quick_validate.py` | 通过：`Skill is valid!` |
| `agents/openai.yaml` YAML 解析 | 通过 |
| `assets/icon.svg` XML/SVG 解析 | 通过 |
| 必需文件与相对资源路径 | 通过 |
| 自动调用策略 | 保持启用 |

## 摄影素材来源

| Demo | 类型 | 作品与作者 | 许可 | 原作页面 |
| --- | --- | --- | --- | --- |
| 1 | 人像 | *Portrait photograph of an unidentified woman*，Arnold Genthe | Public domain | [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Portrait_photograph_of_an_unidentified_woman_LOC_agc.7a10272.jpg) |
| 2 | 街景 | *Street scene (...), 1918*，National Photo Company Collection | Public domain | [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Street_scene_%28...%29,_1918_LCCN2016827087.jpg) |
| 3 | 风景 | *Private lake*，Prindleman | Public domain，作者主动释出 | [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Private_lake.jpg) |
| 4 | 静物 | *Still life fruit*，Jon Sullivan | Public domain，作者主动释出 | [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Still_life_fruit.jpg) |
| 5 | 建筑 | *Door albayzin granada*，Jebulon | Public domain，作者主动释出 | [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Door_albayzin_granada.jpg) |
| 6 | 船景 | *A close up picture of boat on water*，Mills Tamara / U.S. Fish and Wildlife Service | Public domain，美国联邦政府作品 | [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:A_close_up_picture_of_boat_on_water.jpg) |

## 6 组 Demo 验收

| Demo | 主要约束 | 验收结果 | 结论 |
| --- | --- | --- | --- |
| 1 人像 | 身份、侧脸、帽子、卷发、肩部轮廓；无文字 | 关键特征可辨，留白与颗粒成立，无额外物件 | 通过 |
| 2 街景 | 路灯、人群、建筑透视；精确英文 | 空间关系保留；`the city moves in soft ink.` 逐字准确 | 通过 |
| 3 山湖 | 湖面、V 形山谷、山峰、树带、前景石块 | 大结构与蓝灰强调色成立，无新增主体 | 通过 |
| 4 静物 | 铜色滤篮、红色油桃、黄色香蕉；无文字 | 器物轮廓和水果组合保留，红黄强调色受控 | 通过 |
| 5 建筑 | 双开门、四块面板、圆窗；精确中文 | 对称关系保留；`门外，风很轻。` 逐字准确 | 通过 |
| 6 船景 | 船体、驾驶舱、栏杆、倒影、雾山；无文字 | 首次结果有伪文字；一次定向修正后全部清除，其余结构保持 | 通过 |

## Demo 文件对应关系

| Demo | 原图 | 效果图 |
| --- | --- | --- |
| 1 | `source-images/portrait.jpg` | `results/portrait-result.png` |
| 2 | `source-images/street.jpg` | `results/street-result.png` |
| 3 | `source-images/landscape.jpg` | `results/landscape-result.png` |
| 4 | `source-images/still-life.jpg` | `results/still-life-result.png` |
| 5 | `source-images/architecture.jpg` | `results/architecture-result.png` |
| 6 | `source-images/boat.jpg` | `results/boat-result.png` |

## 多图角色隔离附加测试

输入角色：Image 1 为人像目标图；Image 2 为山湖色彩和情绪参考，只允许使用蓝灰、松绿和花岗岩灰。

结果中人物身份、姿态、帽子与轮廓得到保留，参考图只影响色板，没有出现山、湖、树或岩石。结论：**通过**。

![目标图与风格参考隔离结果](results/multi-image-role-result.png)

## 测试原则

- 每张主 Demo 使用一次独立的内置图像生成调用。
- 发现关键约束失败时，只做一次针对性修正并重复必须保留项。
- 不把视觉偏好写成对所有用户请求的硬性要求。
- 验收关注可观察结果：主体和空间关系、风格转换、有限色板、留白、文字准确性和额外元素。
