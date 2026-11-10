# HAT 与 CoA-VLA 论文阅读索引

整理日期：2026-11-10。此目录保留固定版本的官方原文链接，便于复现引用。

**归档状态：本次已上传此阅读索引；两篇 PDF 文件尚未上传到仓库。** 本地 HTTPS 下载遭套接字权限拒绝，浏览器下载操作审批超时。下列 PDF 链接指向 arXiv 官方原文，不是仓库内的附件。

| 论文 | 固定版本 | 官方 PDF | 在线全文 |
| --- | --- | --- | --- |
| HAT：How to Transfer? Zero-Shot Object Recognition via Hierarchical Transfer of Semantic Attributes | arXiv:1604.00326v1，2016-04-01；原会议论文 WACV 2015 | [打开 PDF](https://arxiv.org/pdf/1604.00326v1) | [HTML](https://arxiv.org/html/1604.00326v1) |
| CoA-VLA：Improving Vision-Language-Action Models via Visual-Textual Chain-of-Affordance | arXiv:2412.20451v2，2025-07-31 | [打开 PDF](https://arxiv.org/pdf/2412.20451v2) | [HTML](https://arxiv.org/html/2412.20451v2) |

## 建议阅读位置

- **HAT：第 3.1–3.3 节。** 关注属性怎样沿既有类别层级向上传播、怎样学习不同抽象层级的属性分类器，以及怎样为未见类别选择可迁移属性。第 3.1 节的属性激活表示后代中存在该属性，并不表示所有子类必须具备该属性；论文评估的是视觉识别，不是机器人抓取。
- **CoA-VLA：第 4.1–4.3 节与附录第 7.1–7.3 节。** 关注物体、抓取、空间、运动四类可供性信息如何形成视觉和文本提示并接入动作网络；随后查看消融与新物体姿态实验。可供性信息进入动作策略，并不等于已实现本体关系迁移或完整的物理可行性验证。

本项目讨论的四个方向继续保留：从已有经验向上抽象、按任务选择迁移依据、在当前实例上验证共性、适配并执行。它们既可单独研究，也可连接为闭环；尚未完成实验验证。
