## 6. Psychophysical Experiment

This chapter details the design and implementation of the psychophysical experiment aimed at evaluating subjects' perception of the mechanical properties of virtual deformable objects.

### 6.1 Participants

Ten healthy volunteers were recruited from the author's social circle (7 males, 3 females; aged 18–25). All participants are right-handed and reported no history of neurological disorders, motor impairments, or significant visual deficits. The experimental protocol adheres to the college's ethical guidelines for human research.

*(Note: The demographic details are based on the current plan and subject to confirmation.)*

---

### 6.2 Experimental Setup

Participants interact with the virtual deformable objects using a desktop force-feedback interface (Geomagic Touch / TouchX), which provides 3-DOF positional input and 3-DOF force output. The device is positioned on a desk in front of the participant, who operates the stylus using their dominant hand (right hand).

The underlying haptic rendering is implemented in C++ with a servo loop frequency of approximately 1 kHz, communicating with the device via the official SDK. In each servo cycle, the current position of the stylus tip is sent to the Python simulation module. The simulation updates the state of the virtual strip based on a mass-spring-bending model and returns the reaction force, which is then rendered by the C++ program to the end effector.

During the formal experiment, participants wear a blindfold and noise-cancelling headphones (Apple AirPods Pro 3). White noise is played through the headphones to mask visual cues and environmental sounds, such as keyboard strokes. The experimenter sits to the side to provide verbal instructions, control the software, and input the participants' responses into the terminal. Participants receive no visual feedback; they rely solely on haptic cues to perceive the dynamic response of the objects and answer binary choice questions.

---

### 6.3 Stimuli

#### 6.3.1 Virtual Deformable Object Model

All stimuli are based on the same class of virtual deformable object: a 1D virtual "sausage" (referred to as a strip or beam). The strip consists of 5 point masses connected in series. Adjacent masses are connected by tensile elements (springs), and bending elements are incorporated between adjacent segments to represent flexural rigidity. The mass, spacing, and damping parameters of the particles remain constant across all conditions.

One end of the strip is fixed in the virtual space, while the other end is connected to the haptic stylus via a virtual coupling. When the participant moves the stylus, the first virtual node is displaced, causing the strip to stretch and bend. The simulation calculates the reaction force based on the current deformation and feeds it back to the handle.

Two key parameters define the mechanical properties of the object:
1.  **Stretching Stiffness (k_s):** Determines the resistance when the strip is elongated.
2.  **Bending Stiffness (k_b):** Determines the resistance to bending.
Apart from these, all other physical parameters are fixed.

#### 6.3.2 Stiffness Parameter Settings (10 Virtual Objects)

Ten virtual objects were defined by systematically varying the stretching and bending stiffnesses. The unscaled stiffness values are as follows:

* **Stretching stiffness list:** `stretching_val_list = [50, 287.5, 525, 762.5, 1000]`
* **Bending stiffness list:** `bending_val_list = [0, 0.025, 0.05, 0.075, 0.1]`

*Note: These values represent the raw conceptual range. In the simulation, they may be scaled by a constant factor (e.g., 1000) for numerical stability.*

Based on these lists, two stiffness continua (each with 5 levels) were constructed:

* **Objects 1–5: Stretching Set.**
    k_s \in \{50, 287.5, 525, 762.5, 1000\}, with k_b fixed at 0.05.
* **Objects 6–10: Bending Set.**
    k_b \in \{0, 0.025, 0.05, 0.075, 0.1\}, with k_s fixed at 525.

Thus, Objects 1–5 form a stretching stiffness continuum (constant bending stiffness), and Objects 6–10 form a bending stiffness continuum (constant stretching stiffness).

---

### 6.4 Experimental Design

The study employs a **Two-Alternative Forced Choice (2AFC)** psychophysical paradigm, consisting of two blocks:

1.  **Stretching Block:** Only stretching stiffness (k_s) varies; k_b is fixed.
2.  **Bending Block:** Only bending stiffness (k_b) varies; k_s is fixed.

In both blocks, participants sequentially contact two virtual objects in each trial and make a binary judgment ("First" or "Second") based on the specific instruction.

#### 6.4.1 Stretching Block

Stimuli are selected from Objects 1–5 (k_b = 0.05). The independent variable is k_s.
All **unordered pairs** of the 5 objects are tested, resulting in 10 unique combinations. Each pair is repeated 4 times, totaling 40 trials for this block. To avoid order bias, the presentation order of the two objects within a pair is randomized (approximately 50% probability for each order).

#### 6.4.2 Bending Block

Stimuli are selected from Objects 6–10 (k_s = 525). The independent variable is k_b.
Similar to the stretching block, all unordered pairs are tested (10 pairs × 4 repetitions = 40 trials). The randomization method mirrors that of the stretching block. The order of the two blocks (Stretching first vs. Bending first) is counterbalanced across participants.

---

### 6.5 Procedure

#### 6.5.1 Training Phase

Before the main experiment, participants complete several practice trials without the blindfold or headphones to familiarize themselves with the task. The experimenter explains that in each trial, they will feel two objects—"the First" and "the Second"—and must judge which one feels "stiffer" in a specific dimension:

* **Stretching Block Question:** "Which one is harder to elongate? (i.e., tighter, harder to stretch)"
* **Bending Block Question:** "Which one feels stiffer when bending?"

Feedback is provided during training to ensure participants understand the procedure.

#### 6.5.2 Main Experiment

Participants wear the blindfold and AirPods Pro 3 playing broadband noise. They cannot see the screen or hear the keyboard; the experimenter controls the flow via the terminal.

The procedure for each trial is as follows:

1.  **Initialization:** The program selects the current trial (Block type, Reference vs. Comparison, Order).
2.  **First Stimulus:**
    * Experimenter cue: *"Please feel the first object."*
    * The program renders the `first_object`.
    * Participant explores for 5–8 seconds (pulling for stretching block, moving sideways for bending block).
3.  **Interval:** Force output fades to zero. Experimenter cue: *"First ended, ready for the second."*
4.  **Second Stimulus:** The program renders the `second_object`. Participant explores for 5–8 seconds.
5.  **Response:**
    * Experimenter asks: *"Which one was harder to stretch?"* (or *"Which was stiffer to bend?"*)
    * Participant responds verbally ("First" or "Second").
    * Experimenter records the choice by pressing `1` or `2`.
6.  **Rest:** A short break is provided after the 40 trials of the first block before proceeding to the second block.

---

### 6.6 Data Recording and Analysis

#### 6.6.1 Data Logging

After each trial, behavioral data is automatically logged to a CSV file.
* In the **Stretching Block**, the object with the higher k_s is defined as the correct answer.
* In the **Bending Block**, the object with the higher k_b is defined as the correct answer.
The program calculates and records an `is_correct` flag based on these definitions.

#### 6.6.2 Planned Analysis

Accuracy rates will be calculated for each participant and each block. Based on the recorded data, pairs will be categorized into "small difference" (e.g., adjacent stiffness levels) and "large difference" groups to analyze average accuracy.

The mean accuracy and standard error will be computed across participants to describe group-level performance. If feasible, psychometric curves will be fitted to estimate the discrimination thresholds (Just Noticeable Difference, JND). Finally, overall performance and sensitivity in small-difference conditions will be compared between the Stretching and Bending blocks to determine which mechanical property participants are more sensitive to.