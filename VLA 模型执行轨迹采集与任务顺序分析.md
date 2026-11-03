
## 一、研究背景与目标

当前 VLA（Vision-Language-Action）模型主要通过模仿学习完成机器人操作任务，但训练示范中的子任务执行顺序不一定是最高效的。

例如，在 LIBERO-Long 的“将汤罐和番茄罐放入篮子”任务中，两个物体没有严格的先后放置要求，但模型可能由于训练示范的影响，始终按照固定顺序执行。

本任务旨在使用官方 OpenVLA-OFT 模型，统计其在 LIBERO-Long 数据集上的实际执行行为，重点分析：

1. 模型每次操作物体的先后顺序。
    
2. 机械臂执行任务的运动轨迹。
    
3. 每次抓取、放置操作消耗的 Action Steps。
    
4. 不同初始环境下，模型是否会主动调整任务顺序。
    

**本阶段不需要训练或修改 VLA 模型，只需运行官方模型、采集轨迹并分析数据。**

## 二、前置学习

|学习内容|需要了解的知识|
|---|---|
|LIBERO|机器人操作基准、任务集合、仿真环境、任务成功判断|
|LIBERO-Long|10 个长时序任务及其操作目标|
|OpenVLA-OFT|模型基本原理、官方评测代码和推理流程|
|MuJoCo / robosuite|获取机械臂、夹爪与物体的状态|
|Python|NumPy、Pandas、Matplotlib、CSV 数据处理|

**参考资料：**

- LIBERO：[https://github.com/Lifelong-Robot-Learning/LIBERO](https://github.com/Lifelong-Robot-Learning/LIBERO)
    
- OpenVLA-OFT：[https://github.com/moojink/openvla-oft](https://github.com/moojink/openvla-oft)
    
- 官方 LIBERO 评测说明：[https://github.com/moojink/openvla-oft/blob/main/LIBERO.md](https://github.com/moojink/openvla-oft/blob/main/LIBERO.md)
    

## 三、具体实验步骤

### Step 1：环境搭建与模型运行

按照 OpenVLA-OFT 官方说明搭建环境。

使用 LIBERO-Long 对应的官方预训练模型：

`moojink/openvla-7b-oft-finetuned-libero-10`

先成功运行一个 LIBERO-Long 任务，并生成完整的任务执行视频。

### Step 2：增加轨迹记录功能

在官方评测脚本 `run_libero_eval.py` 中增加数据记录功能。

每个仿真执行步骤应尽可能记录：

- Action：模型输出并实际执行的动作。
    
- EEF Pose：机械臂末端位置与姿态。
    
- Gripper State：夹爪状态。
    
- Object Pose：相关物体的位置与姿态。
    
- Step ID：当前执行步数。
    

每次 Episode 结束后，记录是否成功完成任务，以及总 Action Steps。

注意：Action Steps 指实际执行的环境控制步数，不是模型推理次数；不包含初始化等待步骤。

### Step 3：标注抓取顺序

结合轨迹和执行视频，判断：

- 第一个抓取的物体是什么？
    
- 第二个抓取的物体是什么？
    
- 每个物体分别在哪一步被抓取、放置？
    
- 有没有出现重复抓取、失败重试或顺序改变？
    

初期可以人工查看视频并标注，不强制要求自动识别算法。

### Step 4：批量实验

先选择 LIBERO-Long 中一个双物体抓取任务进行测试，例如 Soup + Tomato → Basket。

初期运行 5 次，确认数据记录正确。

之后使用约 20 个固定初始状态进行正式测试，记录每次执行结果。

完成后逐步扩展至其他适合分析执行顺序的 LIBERO-Long 任务。

## 四、数据统计模板

### 表 1：Episode 执行统计表

每次实验记录一行。

|Task ID|Init State ID|Episode ID|抓取顺序|Success|Total Steps|EEF Path Length|
|---|---|---|---|---|---|---|
|Task_01|S001|E001|A→B|1|—|—|
|Task_01|S002|E002|A→B|1|—|—|
|Task_01|S003|E003|B→A|0|—|—|

表中仅为格式示例，实际数据由实验填写。若任务失败或顺序无法确定，需如实标注。

### 表 2：子任务事件统计表

|Episode ID|Object|Grasp Step|Place Step|Subtask Success|
|---|---|---|---|---|
|E001|A|—|—|—|
|E001|B|—|—|—|

### 表 3：模型任务行为汇总

|Task ID|Episodes|Success Rate|A→B 比例|B→A 比例|成功轨迹平均 Steps|
|---|---|---|---|---|---|
|Task_01|20|—|—|—|—|
|Task_02|20|—|—|—|—|

还需保存逐步轨迹文件，包含时间步、动作、EEF 坐标、夹爪状态和物体状态等信息。

## 五、最终交付内容

1. **运行代码：** 在官方评测代码基础上增加轨迹记录功能，并提供运行说明。
    
2. **原始数据：** 每个 Episode 的完整轨迹文件（CSV 或 NPZ）。
    
3. **统计数据：** 上述三类统计表格，汇总为 Excel 或 CSV。
    
4. **视频：** 每次任务执行的视频，标明任务和初始状态编号。
    
5. **可视化：** 绘制机械臂末端 XYZ 运动轨迹图，标注主要抓取与放置事件。
    
6. **分析报告：** 简要总结模型的执行顺序偏好、成功率和步数分布。
    

## 六、验收要求

**基础目标：**

- 官方 OpenVLA-OFT 模型能够正常运行。
    
- 完成至少一个双物体任务、20 次不同初始状态的评测。
    
- 保存完整轨迹、视频和执行顺序。
    
- 提交统计表与轨迹可视化。
    

**进阶目标：**

- 扩展到多个 LIBERO-Long 任务。
    
- 自动识别抓取、放置事件。
    
- 分析物体初始位置变化是否影响模型执行顺序。
    

## 七、最终需要回答的问题

本任务最终需要用实验数据回答：

**现有 VLA 模型在可自由选择子任务执行顺序时，是否倾向于采用固定顺序？不同场景布局下，它的执行顺序和轨迹会发生怎样的变化？**

本实验只负责真实模型的行为分析，不需要研究最优路径规划算法。