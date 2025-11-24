## 6. Psychophysical Experiment（心理物理实验）

### 6.1 Participants（受试者）

X healthy participants will be recruited from the university community (N = XX; XX female; age range XX–XX years, mean ± SD: XX ± XX years).  
（将从我身边的朋友中招募 10 名健康受试者（总人数10人，其中女性2名；年龄范围 18-25岁之间）

All participants will be right-handed and report no known neurological, motor, or severe visual impairments.  
（所有受试者均为右撇子，并且自述无神经系统疾病、运动功能障碍或严重视力问题。）

Participants will provide written informed consent and will receive course credit or a small voucher as compensation.  
（所有受试者在实验前将签署知情同意书，并通过课程学分或小额礼券获得适当补偿。）

The experimental protocol will follow institutional ethical guidelines for human-subject research.  
（实验方案将遵守学院关于人体研究的伦理规范。）

> TODO: Replace “X, XX” with your actual numbers.（待办：将 X、XX 替换为实际人数和年龄信息。）

---

### 6.2 Apparatus（实验装置）

Participants will interact with virtual deformable objects using a desktop force-feedback haptic device (Geomagic Touch / TouchX, 3-DOF positional input and 3-DOF force output).  
（受试者通过桌面型力反馈装置（Geomagic Touch / TouchX，具有 3 自由度位置输入与 3 自由度力输出）与虚拟可变形物体进行交互。）

The device is placed on a table in front of the participant, aligned approximately with the body midline, and operated with the dominant (right) hand.  
（设备放置在受试者面前的桌面上，位置大致与身体中线对齐，由其优势手（右手）握持笔尖操作。）

The low-level haptic servo loop runs in C++ at approximately 1 kHz and communicates with the device through the vendor’s SDK.  
（底层触觉伺服循环由 C++ 程序实现，更新频率约为 1 kHz，并通过官方 SDK 与设备通信。）

At each servo cycle, the current stylus position is sent to a Python-based simulation module, which implements a mass–spring model of a deformable strip and returns the reaction force to be rendered at the end-effector.  
（在每个伺服周期中，当前笔尖位置会发送到基于 Python 的仿真模块，该模块实现可变形条带的质量–弹簧模型，并返回需要在末端执行器上呈现的反作用力。）

During the main experiment, participants wear an eye mask and over-ear headphones playing broadband noise to block visual cues and mask device sounds; only the experimenter can see the terminal display.  
（在正式实验过程中，受试者佩戴眼罩和包耳式耳机，耳机中播放宽带噪声以屏蔽视觉线索并掩盖设备噪音；只有实验员可以看到终端界面。）

---

### 6.3 Stimuli（刺激）

#### 6.3.1 Deformable object model（可变形物体模型）

All stimuli are based on the same virtual deformable object: a one-dimensional strip represented by a serial chain of five point masses connected by stretching and bending elements.  
（所有刺激都基于同一类虚拟可变形物体：一条一维虚拟条带，由五个点质量通过拉伸和弯曲元件串联连接而成。）

The masses are equally spaced along a straight line and share identical mass and damping parameters.  
（各点质量沿直线等距排布，并具有相同的质量与阻尼参数。）

One end of the strip is fixed in virtual space, while the other end is attached to the haptic stylus via a virtual coupling.  
（条带的一端在虚拟空间中固定，另一端通过虚拟耦合连接到触觉笔尖。）

Stretching stiffness \(k_s\) controls the resistance to changes in the distance between neighbouring masses, whereas bending stiffness \(k_b\) controls the resistance to changes in the angle between adjacent segments.  
（拉伸刚度 \(k_s\) 决定相邻质点间距离变化所受的阻力，而弯曲刚度 \(k_b\) 决定相邻段之间夹角变化时的抗弯阻力。）

All parameters other than \(k_s\) and \(k_b\) are kept constant across stimulus conditions.  
（除 \(k_s\) 和 \(k_b\) 外，其他模型参数在所有刺激条件下保持不变。）

#### 6.3.2 Stiffness parameter sets（刚度参数集合）

Ten virtual objects are defined by systematically varying stretching and bending stiffness within predefined ranges.  
（通过在预设范围内系统性调节拉伸刚度和弯曲刚度，共定义了 10 个虚拟物体。）

Let the unscaled stiffness values be:  
（未缩放的刚度取值定义为：）

- `stretching_val_list = [50, 287.5, 525, 762.5, 1000]`  
  （拉伸刚度列表：`[50, 287.5, 525, 762.5, 1000]`。）  
- `bending_val_list    = [0, 0.025, 0.05, 0.075, 0.1]`  
  （弯曲刚度列表：`[0, 0.025, 0.05, 0.075, 0.1]`。）

For numerical stability, these values may be scaled internally (e.g., by a factor of 1000) in the simulation, but we report the conceptual values here.  
（为数值稳定性考虑，这些数值在仿真中可能会进行内部缩放（例如乘以 1000），但在本文中仅报告其概念上的原始范围。）

Two 5-level stiffness continua are constructed as follows:  
（据此构造两个包含 5 个水平的刚度连续体：）

- **Objects 1–5 (Stretching set)**:  
  \(k_s \in \{50, 287.5, 525, 762.5, 1000\}\) with \(k_b = 0.05\) held constant.  
  （物体 1–5（拉伸组）：\(k_s\) 依次取 \{50, 287.5, 525, 762.5, 1000\}，弯曲刚度 \(k_b\) 固定为 0.05。）

- **Objects 6–10 (Bending set)**:  
  \(k_b \in \{0, 0.025, 0.05, 0.075, 0.1\}\) with \(k_s = 525\) held constant.  
  （物体 6–10（弯曲组）：\(k_b\) 依次取 \{0, 0.025, 0.05, 0.075, 0.1\}，拉伸刚度 \(k_s\) 固定为 525。）

Thus, Objects 1–5 form a continuum in stretching stiffness with constant bending stiffness, and Objects 6–10 form a continuum in bending stiffness with constant stretching stiffness.  
（因此，物体 1–5 构成在弯曲刚度恒定条件下的拉伸刚度连续体，而物体 6–10 构成在拉伸刚度恒定条件下的弯曲刚度连续体。）

---

### 6.4 Experimental Design（实验设计）

The experiment consists of two 2AFC discrimination blocks: a stretching block and a bending block.  
（实验由两个 2AFC 辨别任务 block 构成：拉伸 block 与弯曲 block。）

In both blocks, participants sequentially interact with two objects on each trial and must decide which of the two feels stiffer along the relevant dimension.  
（在两个 block 中，受试者在每个试次依次接触两个物体，并判断哪一个在该维度上感觉更“硬”。）

#### 6.4.1 Stretching block（拉伸 block）

In the stretching block, only stretching stiffness \(k_s\) varies while bending stiffness \(k_b\) is held constant.  
（在拉伸 block 中，仅拉伸刚度 \(k_s\) 发生变化，而弯曲刚度 \(k_b\) 保持不变。）

Stimuli are drawn from Objects 1–5.  
（刺激取自物体 1–5。）

All unordered pairs of the five objects are tested, yielding \( \binom{5}{2} = 10 \) distinct object pairs.  
（对这五个物体的所有无序组合进行测试，共得到 \( \binom{5}{2} = 10 \) 种不同的物体配对。）

Each object pair is repeated 4 times, resulting in 40 trials in the stretching block.  
（每个物体配对重复 4 次，因此拉伸 block 共包含 40 个试次。）

On each repetition, the order of the two objects (which one is first vs second) is randomized with equal probability.  
（在每次重复中，两物体的呈现顺序（先后）以相等概率随机决定。）

#### 6.4.2 Bending block（弯曲 block）

In the bending block, only bending stiffness \(k_b\) varies while stretching stiffness \(k_s\) is held constant.  
（在弯曲 block 中，仅弯曲刚度 \(k_b\) 发生变化，而拉伸刚度 \(k_s\) 保持不变。）

Stimuli are drawn from Objects 6–10, again producing 10 unique object pairs.  
（刺激取自物体 6–10，同样形成 10 个独特的物体配对。）

Each pair is repeated 4 times, for another 40 trials in the bending block.  
（每个配对重复 4 次，因此弯曲 block 也包含 40 个试次。）

As in the stretching block, the order of presentation within each pair is randomized on every trial.  
（与拉伸 block 类似，每个试次中两物体的呈现顺序均随机打乱。）

The order of the two blocks (stretching first vs bending first) may be counterbalanced across participants.  
（两个 block 的整体顺序（先拉伸后弯曲或相反）可以在被试之间进行平衡。）

---

### 6.5 Procedure（实验流程）

#### 6.5.1 Training phase（训练阶段）

Before the main experiment, participants receive verbal instructions and perform several practice trials without the eye mask and headphones.  
（在正式实验之前，受试者首先在不戴眼罩和耳机的情况下接受口头说明，并完成若干练习试次。）

The experimenter explains that in each trial the participant will interact with two virtual objects in sequence—“the first” and “the second”—and that the task is to choose which one feels stiffer along a specified dimension.  
（实验员向受试者说明：每个试次中将依次接触两个虚拟物体——“第一个”和“第二个”，任务是在指定维度上选出感觉更“硬”的一个。）

In the stretching block, the instruction is:  
（在拉伸 block 中，指令为：）

> “Which object feels harder to stretch, that is, less stretchable / more tightly stretched?”  
> （“哪一个更不容易被拉长，也就是更紧、更难拉伸？”）

In the bending block, the instruction is:  
（在弯曲 block 中，指令为：）

> “Which object feels stiffer when being bent?”  
> （“哪一个在弯的时候感觉更硬？”）

During practice, participants can see a simple on-screen prompt and receive feedback from the experimenter to ensure that they understand the meaning of “first” and “second” and the response format.  
（在练习阶段，受试者可以看到简单的屏幕提示，并从实验员处获得反馈，以确保其理解“第一个/第二个”的含义以及回答方式。）

Once the participant demonstrates that they can reliably respond “first” or “second” according to the instruction, the main experiment begins.  
（当受试者能够根据指令稳定地给出“第一个”或“第二个”的回答后，正式实验开始。）

#### 6.5.2 Main experiment（正式实验）

For the main experiment, participants wear the eye mask and over-ear headphones playing broadband noise to eliminate visual cues and mask device sounds.  
（在正式实验中，受试者佩戴眼罩和包耳式耳机，耳机中播放宽带噪声，以消除视觉线索并掩盖设备噪音。）

Only the experimenter can see the terminal display and control the experiment flow.  
（只有实验员可以看到终端界面并控制实验流程。）

Each trial proceeds as follows:  
（每个试次的具体流程如下：）

1. **Trial initialization（试次初始化）**  
   The control program selects the next trial from a pre-generated list, specifying the block type, the object pair (reference and comparison), and the randomized first/second presentation order.  
   （控制程序从预生成的列表中选取当前试次，包含所属 block、物体配对（参考与比较）以及随机化后的先后顺序。）

2. **First stimulus interval（第一刺激时段）**  
   The experimenter instructs the participant: “Now feel the first object.”  
   （实验员提示受试者：“现在请感受第一个物体。”）  
   The simulation switches to the corresponding object, and the participant explores the strip for approximately 5–8 s by stretching or bending it with the stylus.  
   （仿真切换到对应物体，受试者通过笔尖对条带进行拉伸或弯曲探索约 5–8 秒。）

3. **Inter-interval pause（间隔期）**  
   The haptic force is ramped down towards zero, and the participant briefly relaxes.  
   （触觉力逐渐减小至接近零，受试者短暂放松。）  
   The experimenter announces: “The first is finished, now get ready for the second.”  
   （实验员提示：“第一个结束，请准备第二个。”）

4. **Second stimulus interval（第二刺激时段）**  
   The simulation switches to the second object, and the participant explores it for another 5–8 s.  
   （仿真切换到第二个物体，受试者再次探索约 5–8 秒。）

5. **Response（作答）**  
   The experimenter reminds the participant of the decision:  
   （实验员提醒受试者做出判断：）  
   - Stretching block: “Which one was harder to stretch?”（拉伸 block：“哪一个更不容易被拉长？”）  
   - Bending block: “Which one felt stiffer when being bent?”（弯曲 block：“哪一个在弯的时候感觉更硬？”）  
   The participant responds verbally with “the first” or “the second”, and the experimenter records the response by pressing `1` or `2` on the terminal.  
   （受试者口头回答“第一个”或“第二个”，实验员在终端按下 `1` 或 `2` 进行记录。）

6. **Rest and continuation（休息与继续）**  
   Trials continue until all 40 trials in the current block are completed, with a short break offered roughly halfway through the block and between blocks.  
   （试次依次进行，直到当前 block 的 40 个试次完成，中途及两个 block 之间会安排短暂休息。）

---

### 6.6 Data Recording and Analysis（数据记录与分析）

#### 6.6.1 Behavioural data logging（行为数据记录）

After each trial, the control program automatically writes one row of behavioural data to a CSV file for the current participant and block.  
（每个试次结束后，控制程序会为当前被试和当前 block 自动向 CSV 文件写入一行行为数据。）

The following fields are stored:  
（CSV 中包含如下字段：）

- `participant_id`（被试编号）  
- `trial_index`（当前 block 内的试次序号）  
- `block_type`（"stretch" 或 "bend"）  
- `ref_object_id`, `comp_object_id`（参考物体与比较物体的 ID）  
- `k_stretch_ref`, `k_stretch_comp`（两物体的拉伸刚度）  
- `k_bend_ref`, `k_bend_comp`（两物体的弯曲刚度）  
- `first_object`, `second_object`（实际先呈现与后呈现的物体 ID）  
- `chosen_object`（被试选择“first”或“second”）  
- `is_correct`（是否选择了物理上更硬的物体：1=正确，0=错误）  
- `notes`（实验员备注，可留空）

For each trial, the physically stiffer object along the relevant dimension is defined as the correct answer:  
（在每个试次中，沿当前维度物理上更“硬”的物体被定义为正确答案：）

- In the stretching block, the object with larger \(k_s\) is considered harder to stretch.  
  （在拉伸 block 中，\(k_s\) 较大的物体被视为“更不容易被拉长”。）  
- In the bending block, the object with larger \(k_b\) is considered stiffer in bending.  
  （在弯曲 block 中，\(k_b\) 较大的物体被视为“在弯时更硬”。）

The program compares the chosen object to this target and sets `is_correct` accordingly.  
（程序将被试选择与目标物体进行比较，并据此设置 `is_correct` 的取值。）

#### 6.6.2 Planned analysis（计划分析）

For each participant and block, the proportion of correct responses is computed for each object pair.  
（对每名受试者和每个 block，首先计算每个物体配对的正确率。）

Object pairs can be grouped by stiffness difference (e.g., adjacent vs non-adjacent stiffness levels) to obtain performance under “small” and “large” stiffness differences.  
（物体配对可按刚度差大小进行分组（例如相邻 vs 非相邻刚度水平），以分别获得“较小差异”和“较大差异”条件下的表现。）

Group-level averages and standard errors are then computed across participants for each condition.  
（随后在被试间对各条件的正确率进行平均，并计算标准误。）

Where appropriate, psychometric functions may be fitted as a function of stiffness difference (Δ\(k_s\) or Δ\(k_b\)) to estimate discrimination thresholds (e.g., JND).  
（在适当情况下，可以将正确率作为刚度差（Δ\(k_s\) 或 Δ\(k_b\)）的函数拟合心理物理曲线，以估计辨别阈值（如 JND）。）

Finally, discrimination performance in the stretching and bending blocks will be compared to assess whether participants are more sensitive to changes in stretching stiffness or in bending stiffness within the tested ranges.  
（最后，将比较拉伸 block 与弯曲 block 的辨别表现，以评估在所测试的刚度范围内，受试者对拉伸刚度变化还是对弯曲刚度变化更为敏感。）

