---
layout: page
title: Human-centered Risk Assessment & Decision Making
description: From human-like risk perception to physics-informed risk assessment, occupant injury prediction, and injury-aware safety decisions.
importance: 1
category: research
giscus_comments: false # 关闭评论
related_publications: true
permalink: /projects/human-centered-risk-assessment/

# Optional project-card image. Uncomment after adding the image to assets/img/.
# img: assets/img/Research_1/human_centered_risk_assessment.jpg

# Public resources. Update the empty URLs when datasets/code are released.
resources:
  ada_code: "https://github.com/ShunGan/ADA"
  vehicle_crash_database: "https://github.com/wangqf1997/Vehicle-Crash-Database"
  piram_dataset: ""
  piram_code: ""
  mfpgrnet_dataset: ""
  mfpgrnet_code: ""
  unified_safety_dataset: ""
  unified_safety_code: ""
---

> **Research objective.** We develop human-centered methods for understanding, predicting, and mitigating risk in safety-critical driving scenarios. Rather than defining vehicle safety solely by whether a collision occurs, our research connects **human risk perception, traffic interaction, collision likelihood, impact severity, occupant injury, and safety decision making** within a unified pre-crash-to-injury framework.

Our work combines **human behavioral data, vehicle dynamics, injury biomechanics, driving simulation, and data-driven modeling** to study safety across three coupled scales:

**Traffic-level interaction** → **vehicle-level collision** → **occupant-level injury**

This research direction is organized around four closely connected components:

1. **Human-centered risk perception** — understanding what human drivers attend to and how latent hazards are perceived.
2. **Physics-informed risk assessment** — jointly quantifying collision probability and collision severity under dynamic traffic interactions.
3. **Occupant injury risk prediction** — predicting biomechanical consequences efficiently across different levels of model fidelity.
4. **Safety-critical and injury-aware decision support** — integrating collision occurrence and occupant injury into a unified safety evaluation framework and providing the basis for injury-aware automated-driving decisions.

---

## Research framework

The central idea is to move beyond collision-only safety assessment and connect the complete chain from **risk emergence to human injury consequence**:

```text
Human & traffic environment
        ↓
Human-like risk perception
        ↓
Physics-informed driving risk assessment
        ↓
Collision probability & collision severity
        ↓
Occupant injury prediction
        ↓
Unified safety evaluation
        ↓
Injury-aware decision support & integrated safety
```

In this framework, collision avoidance remains the primary objective. When a collision cannot be fully avoided, the system should also estimate and mitigate the potential human consequences of alternative outcomes. This provides a common basis for coordinating **active safety, automated-driving decisions, and occupant protection**.

---

## 1. Human-centered risk perception

### Adaptive driver attention prediction across heterogeneous driving scenes

Human drivers do not process all visual information equally. Their attention is selectively allocated to traffic elements that are relevant to the current driving task, including potential hazards and interacting road users. Understanding this process provides an important behavioral basis for human-centered intelligent driving systems.

We developed an **Adaptive Driver Attention (ADA)** model to predict driver visual attention across heterogeneous driving environments. The model is inspired by the two principal mechanisms of human visual attention:

- **Bottom-up attention**, in which salient visual stimuli attract attention automatically.
- **Top-down attention**, in which drivers adjust perceptual priorities according to the current driving task and traffic context.

The ADA framework combines generic feature encoders with scene-adaptive attention modules to reproduce human-like perceptual patterns while addressing substantial domain shifts among driver-attention datasets. The model incorporates **domain-specific batch normalization, Gaussian priors, smoothing filters, spatial attention, channel attention, and domain-specific focal loss** to mitigate heterogeneity arising from different video sources, gaze-collection protocols, scene distributions, and saliency-map generation procedures.

The model was jointly trained on four public driver-attention datasets — **BDD-A, DADA-2000, DReyeVE, and EyeTrack** — and further evaluated on **PSAD**, which was not used for joint training. The results show that the model can generalize across heterogeneous traffic scenes and reproduce characteristic human attention patterns in cruising, turning, conflict, and accident-related situations.

Importantly, the predicted attention maps can shift toward **latent conflict regions and relevant interacting vehicles**, providing a basis for identifying where human drivers are likely to allocate attention before and during safety-critical events.

**Research focus:** driver attention prediction · human-like perception · latent hazard identification · multi-source domain adaptation

**Representative publication**  
S. Gan et al., *Multisource Adaption for Driver Attention Prediction in Arbitrary Driving Scenes*, IEEE Transactions on Intelligent Transportation Systems, 2022.  
[Paper](https://doi.org/10.1109/TITS.2022.3177640) · [Code & pretrained models]({{ page.resources.ada_code }})

---

## 2. Physics-informed risk assessment

### Integrated assessment of collision probability and collision severity

Conventional surrogate safety indicators often characterize only one dimension of a hazardous interaction. For example, time-to-collision can indicate temporal urgency in longitudinal conflicts but may be less informative for complex lateral interactions. More importantly, two situations with similar collision probability can lead to substantially different collision consequences.

We therefore define driving risk by jointly considering **the probability of collision and the potential severity of the collision**:

$$
\mathrm{Risk} = \mathrm{Collision\ Probability} \times \mathrm{Collision\ Severity}.
$$

To operationalize this concept, we developed a **Physics-Informed Integrated Risk Assessment Model (PIRAM)**. PIRAM predicts collision time and collision velocity from vehicle-interaction histories and map information, then transforms these predictions into collision probability and collision severity and combines them into an **Integrated Driving Risk (IDR)** metric:

$$
IDR = R_p \times R_s,
$$

where $R_p$ represents collision probability and $R_s$ represents collision severity.

### Physics-informed learning

PIRAM combines a data-driven neural network with a physics-based prediction model:

- **Interaction and map encoding.** Historical trajectories of the ego vehicle and hazard-triggering vehicle are represented together with surrounding lane geometry.
- **Cross-attention and self-attention.** Attention mechanisms capture spatial and temporal dependencies among interacting vehicles and the road environment.
- **Multi-task prediction.** The neural network jointly predicts collision time and collision velocity.
- **Vehicle-dynamics constraints.** A dynamic bicycle model and drivable-area prediction provide physical constraints during training, reducing predictions that conflict with feasible vehicle motion.

This hybrid design is intended to preserve the representation capability of data-driven models while improving **prediction stability, physical consistency, and reliability** in safety-critical conditions.

### Integrated Driving Risk Prediction Dataset (IDRPD)

To support this work, we constructed the **Integrated Driving Risk Prediction Dataset (IDRPD)** from driver-in-the-loop experiments in a high-fidelity driving simulator. The dataset contains **1,879 safety-critical interaction events** involving:

- cut-in conflicts,
- merging conflicts,
- emergency-braking conflicts,
- driver collision-avoidance maneuvers,
- non-collision and collision outcomes,
- collision timing, and
- collision velocity.

Data were recorded at 20 Hz around the hazard-triggering event. A sliding-time-window augmentation strategy was further used to represent the temporal evolution from normal driving to escalating risk and eventual collision or successful avoidance.

### Main findings

Compared with baseline physics-based and data-driven approaches, PIRAM improved the prediction of both collision probability and collision severity. The study reported improvements in prediction accuracy of approximately **7.9% for collision probability** and **3.2% for collision severity**, together with improved temporal stability. Case studies further showed that the model can identify the escalation of integrated driving risk earlier than conventional approaches, providing additional time for safety intervention and occupant-protection strategies.

**Research focus:** physics-informed learning · collision probability · collision severity · dynamic risk evolution · active-passive safety integration

**Representative publication**  
T. Lu et al., *A physics-informed attention model for integrated driving risk assessment*, Accident Analysis & Prevention, 2025.  
[Paper](https://doi.org/10.1016/j.aap.2025.108266)

{% if page.resources.piram_dataset != "" or page.resources.piram_code != "" %}
**Open resources:**
{% if page.resources.piram_dataset != "" %}[Dataset]({{ page.resources.piram_dataset }}){% endif %}
{% if page.resources.piram_dataset != "" and page.resources.piram_code != "" %} · {% endif %}
{% if page.resources.piram_code != "" %}[Code]({{ page.resources.piram_code }}){% endif %}
{% else %}
**Open resources:** `IDRPD dataset · coming soon` · `PIRAM code · coming soon`
{% endif %}

---

## 3. Occupant injury risk prediction

Avoiding collisions remains the primary goal of vehicle safety systems, but residual and unavoidable collisions cannot be eliminated completely. In these conditions, intelligent vehicles require the ability to estimate potential occupant injury rapidly and reliably.

Our research connects **vehicle-level crash dynamics** with **occupant-level biomechanical outcomes**, with particular emphasis on computational efficiency, data scarcity, model fidelity, and engineering applicability.

### 3.1 Near-real-time occupant injury prediction

We developed a data-driven framework for predicting occupant injury severity from vehicle crash information in near real time.

A large-scale numerical database containing **28,000 frontal collision cases** was constructed by combining finite-element, multi-body, and lumped-parameter simulation models. The database covers variations in:

- vehicle crash pulse,
- occupant gender,
- seatbelt use,
- airbag use,
- head acceleration,
- chest displacement,
- neck force, and
- neck moment.

The resulting occupant kinematic responses were converted to body-region-specific injury severity using established injury criteria and the Abbreviated Injury Scale (AIS).

#### Deep-learning prediction and interpretable feature extraction

The study first used sequence models to learn the nonlinear relationship between vehicle crash pulses and occupant kinematic responses. A convolutional neural network achieved high prediction performance but remained too computationally expensive for time-critical onboard applications.

To reduce model complexity, network visualization was used to examine how the high-accuracy model processed crash-pulse information. A two-layer pooling procedure then compressed the original **120-dimensional crash pulse into three kinematic features**. These features were combined with occupant and restraint information in a lightweight machine-learning model.

The final model achieved:

- **85.4% accuracy** for head injury severity on the numerical dataset,
- **78.7% accuracy** on an independent **192-case real-world crash dataset**, and
- approximately **1.2 ± 0.4 ms** prediction time.

This work demonstrates that biomechanically relevant crash information can be condensed into low-dimensional kinematic features while retaining useful injury-prediction capability, making the model suitable as a decision reference for **pre-crash trajectory planning and adaptive restraint control**.

**Research focus:** occupant injury biomechanics · near-real-time prediction · interpretable feature extraction · onboard safety decision support

**Representative publication**  
Q. Wang et al., *A data-driven, kinematic feature-based, near real-time algorithm for injury severity prediction of vehicle occupants*, Accident Analysis & Prevention, 2021.  
[Paper](https://doi.org/10.1016/j.aap.2021.106149) · [Public vehicle-crash database]({{ page.resources.vehicle_crash_database }})

---

### 3.2 Knowledge-guided multi-fidelity injury prediction

High-fidelity finite-element crash simulations provide detailed biomechanical information, but their computational cost makes it difficult to generate sufficiently large datasets, particularly during early-stage vehicle development. Low-fidelity multi-body simulations are substantially cheaper but contain systematic bias because they simplify tissue deformation, contact mechanics, and local anatomical responses.

To exploit the complementary strengths of both fidelity levels, we developed **MF-PGRNet**, a knowledge-guided multi-fidelity residual learning framework for occupant injury prediction under limited high-fidelity supervision.

Rather than treating low-fidelity simulations simply as additional training samples, the framework uses them as a source of **structured physics priors**. The high-fidelity prediction task is decomposed into:

$$
y_{HF}
= f_{trend}(x)
+ f_{residual}(x),
$$

where the first term represents a transferable physical trend learned from abundant low-fidelity simulations, and the second term corrects the fidelity-specific biomechanical gap using sparse high-fidelity observations.

### Core methodological components

- **Shared multi-modal encoding** for bi-axial crash pulses, restraint parameters, and occupant characteristics.
- **Learnable physics prototype library** that represents characteristic patterns in the low-fidelity crash-response manifold.
- **Prototype cross-attention** that retrieves query-relevant low-fidelity priors for an individual high-fidelity sample without requiring paired LF-HF cases.
- **Physics-prior guided residual learning** that separates transferable physical trends from fidelity-specific numerical bias.
- **Adaptive gated fusion** that controls how strongly the model relies on retrieved priors when LF-HF discrepancies vary.
- **Relational knowledge distillation** that preserves useful topological structure across fidelity domains and improves learning under sparse high-fidelity data.

### Multi-fidelity crash datasets

The framework was evaluated using:

- **4,849 low-fidelity cases**, generated through vehicle crash-pulse simulation and multi-body occupant dynamics; and
- **3,598 high-fidelity finite-element cases**, with each high-fidelity simulation requiring approximately **45 h on 24 CPU cores** on average.

The simulations include crash configuration parameters, restraint-system variables, occupant characteristics and posture, crash-pulse histories, and injury metrics such as **HIC, Nij, and maximum chest deflection**.

Under limited high-fidelity supervision, MF-PGRNet reduced prediction errors by **more than 16%** relative to representative baselines and achieved performance comparable to models trained with substantially more high-fidelity data. The results show that cross-fidelity physics knowledge can reduce reliance on expensive finite-element simulations while maintaining useful injury-prediction performance for virtual safety assessment.

**Research focus:** multi-fidelity learning · physics-prior learning · occupant injury prediction · virtual safety assessment · data-efficient occupant protection

**Representative publication**  
T. Lu et al., *A knowledge-guided multi-fidelity residual learning framework for occupant injury prediction from crash simulations*, Advanced Engineering Informatics, 2026.  
[Paper](https://doi.org/10.1016/j.aei.2026.105117)

{% if page.resources.mfpgrnet_dataset != "" or page.resources.mfpgrnet_code != "" %}
**Open resources:**
{% if page.resources.mfpgrnet_dataset != "" %}[Dataset]({{ page.resources.mfpgrnet_dataset }}){% endif %}
{% if page.resources.mfpgrnet_dataset != "" and page.resources.mfpgrnet_code != "" %} · {% endif %}
{% if page.resources.mfpgrnet_code != "" %}[Code]({{ page.resources.mfpgrnet_code }}){% endif %}
{% else %}
**Open resources:** `Multi-fidelity dataset · coming soon` · `MF-PGRNet code · coming soon`
{% endif %}

---

## 4. Safety-critical and injury-aware decision support

### From collision avoidance to unified safety evaluation

Vehicle safety is often assessed primarily by collision frequency or crash rate. These indicators are necessary but incomplete: they do not describe the consequences for vehicle occupants when collision avoidance fails.

We therefore developed a **unified experimental framework** that evaluates automated-driving safety through both **collision occurrence and occupant injury severity** under comparable safety-critical scenarios.

The framework uses a high-fidelity driving simulator and parameterized hazard-triggering algorithms to generate three representative highway conflict types:

- **Braking:** a leading vehicle performs sudden emergency braking;
- **Cut-in:** a surrounding vehicle abruptly enters the ego vehicle's lane; and
- **Merging:** a surrounding vehicle competes for the ego vehicle's target lane during a lane change.

Vehicles representing **attentive manual driving (L0 reference), SAE Level 2, Level 3, and Level 4 automation** were evaluated under comparable road scenarios, similar urgency levels, and consistent metrics.

### Experimental dataset

The study collected:

- **30 valid participants**,
- **1,859 safety-critical vehicle interactions**, and
- **337 collisions**.

The framework records the progression from normal driving through hazard triggering, decision making, driver intervention or disengagement, collision avoidance, and residual collision.

### Occupant injury estimation

For each collision, occupant injury severity was estimated using a logistic-regression model derived from NASS/CDS crash data. Collision conditions were first reconstructed using a planar rigid-body momentum model to estimate vehicle delta-v. The resulting crash information was then mapped to the probability of serious occupant injury:

$$
IR = P(MAIS3+).
$$

A threshold of $P(MAIS3+) \geq 0.2$ was used to identify severe-injury cases in the study.

### Unified Safety Benefit

To jointly represent collision occurrence and occupant injury, the study proposed a **Safety Benefit (SB)** metric:

$$
SB_i = 1 - \frac{P_{col,i} \times IR_i}{0.2},
$$

where $P_{col,i}$ is the collision rate and $IR_i$ is the average occupant injury risk for automation level $i$.

This formulation extends safety assessment from a collision-only perspective toward an integrated measure that incorporates the consequences of residual crashes.

### Main findings

Under the designed safety-critical scenarios, the study reported:

| Automation level | Collision rate | Probability of severe occupant injury |
| --- | ---: | ---: |
| Level 2 | 24.3% | 11.1% |
| Level 3 | 21.4% | 21.6% |
| Level 4 | 14.1% | 9.2% |

The results show that **a lower collision rate does not necessarily translate directly into a proportionally higher overall safety benefit**. In the experiment, Level 3 automation reduced collision occurrence relative to Level 2 but exhibited higher injury severity when collisions occurred, resulting in comparable unified safety benefits. Level 4 achieved a higher overall safety benefit primarily through a lower collision rate, while the reduction in residual-collision injury severity was more limited.

These findings motivate a broader definition of automated-driving safety:

> **Collision avoidance is necessary, but collision avoidance alone is not sufficient to characterize the safety performance of an automated vehicle.**

The current published framework establishes the experimental and quantitative basis for injury-aware decision support. A fully closed-loop controller that directly optimizes automated-driving trajectories using predicted injury outcomes is an ongoing research direction rather than a claim of the present study.

**Research focus:** safety-critical scenarios · automated-driving safety evaluation · collision-injury integration · injury-aware decision support · integrated safety

**Representative publication**  
J. Shen et al., *A unified experimental framework for estimating collision rates and occupant injury severity across different levels of driving automation*, Accident Analysis & Prevention, 2025.  
[Paper](https://doi.org/10.1016/j.aap.2025.108273)

{% if page.resources.unified_safety_dataset != "" or page.resources.unified_safety_code != "" %}
**Open resources:**
{% if page.resources.unified_safety_dataset != "" %}[Dataset]({{ page.resources.unified_safety_dataset }}){% endif %}
{% if page.resources.unified_safety_dataset != "" and page.resources.unified_safety_code != "" %} · {% endif %}
{% if page.resources.unified_safety_code != "" %}[Evaluation code]({{ page.resources.unified_safety_code }}){% endif %}
{% else %}
**Open resources:** `Experimental dataset · coming soon` · `Evaluation code · coming soon`
{% endif %}

---

## From pre-crash risk to in-crash injury

Together, these studies form a continuous research chain across perception, prediction, biomechanics, and safety evaluation:

| Research stage | Main question | Representative method |
| --- | --- | --- |
| **Human risk perception** | Which elements in a complex traffic scene are behaviorally relevant to human drivers? | Adaptive Driver Attention (ADA) |
| **Driving risk assessment** | How likely is a collision, how severe may it be, and how does the risk evolve over time? | PIRAM + IDRPD |
| **Near-real-time injury prediction** | If a collision occurs, what injury may the occupant sustain and can it be predicted fast enough for onboard use? | Kinematic feature-based injury prediction |
| **High-fidelity injury prediction** | How can reliable injury models be trained when high-fidelity crash simulations are scarce and expensive? | MF-PGRNet |
| **Unified safety evaluation** | How should vehicle safety be evaluated when both collision occurrence and occupant injury matter? | Collision rate + injury risk + Safety Benefit |
| **Injury-aware decision support** | How can predicted pre-crash risk and in-crash injury consequences jointly inform safer automated-driving actions? | Ongoing integrated decision framework |

The long-term objective is to establish a quantitative link between candidate vehicle actions and their expected human consequences:

$$
a
\rightarrow P(\mathrm{collision}\mid a)
\rightarrow C_{crash}(a)
\rightarrow P(\mathrm{injury}\mid C_{crash}, a),
$$

where $a$ denotes a candidate evasive or protective action and $C_{crash}$ denotes the resulting collision condition if avoidance is unsuccessful.

This formulation provides a basis for selecting actions that consider not only **whether a collision can be avoided**, but also **how severe the resulting human injury may be when complete avoidance is infeasible**.

---

## Ongoing and future directions

The following directions extend the published work above and represent continuing research rather than completed claims.

### Pre-crash-to-injury integrated safety

We aim to connect active safety and passive safety within a common decision framework. Instead of evaluating collision avoidance and occupant protection independently, candidate maneuvers can be evaluated through their downstream collision conditions and predicted biomechanical consequences.

### Human-informed risk representation

Driver attention, hazard perception, and evasive behavior can provide behavioral priors for intelligent risk models. Future work will investigate whether human gaze and decision patterns can be used as supervision, regularization, or interpretability references for automated-driving risk assessment.

### Personalized injury-aware safety

Occupant injury depends on more than collision severity. Occupant body size, posture, restraint state, and protection-system configuration can substantially change the resulting biomechanical response. We are extending injury prediction toward **occupant-specific risk estimation and personalized protection**.

### Uncertainty-aware safety decision making

Safety-critical systems require not only point predictions but also reliable estimates of predictive uncertainty. Future risk and injury models should distinguish between **low predicted risk** and **high model uncertainty**, allowing conservative intervention when the model operates outside well-supported regions of the data distribution.

### Human-centered automotive safety benchmark

By organizing driver-attention data, safety-critical interaction data, collision-condition data, multi-fidelity crash simulations, and occupant-injury information under consistent interfaces, we aim to establish an open benchmark for research on:

1. driver attention prediction,
2. collision probability prediction,
3. collision severity prediction,
4. occupant injury prediction,
5. multi-fidelity injury learning,
6. automated-driving safety evaluation, and
7. injury-aware decision making.

---

## Open data & code

Reproducibility and open research are integral parts of this research program. Public resources will be released progressively with standardized documentation, variable definitions, train/validation/test splits, model inputs and outputs, and minimal reproduction examples.

| Project | Paper | Dataset | Code / model | Status |
| --- | :---: | :---: | :---: | --- |
| Adaptive Driver Attention (ADA) | [Paper](https://doi.org/10.1109/TITS.2022.3177640) | Public source datasets | [Code & weights]({{ page.resources.ada_code }}) | Available |
| Physics-Informed Integrated Risk Assessment (PIRAM / IDRPD) | [Paper](https://doi.org/10.1016/j.aap.2025.108266) | {% if page.resources.piram_dataset != "" %}[Dataset]({{ page.resources.piram_dataset }}){% else %}Coming soon{% endif %} | {% if page.resources.piram_code != "" %}[Code]({{ page.resources.piram_code }}){% else %}Coming soon{% endif %} | Preparing release |
| Near-real-time Occupant Injury Prediction | [Paper](https://doi.org/10.1016/j.aap.2021.106149) | [Vehicle-crash database]({{ page.resources.vehicle_crash_database }}) | Database repository | Available |
| Multi-fidelity Occupant Injury Prediction (MF-PGRNet) | [Paper](https://doi.org/10.1016/j.aei.2026.105117) | {% if page.resources.mfpgrnet_dataset != "" %}[Dataset]({{ page.resources.mfpgrnet_dataset }}){% else %}Coming soon{% endif %} | {% if page.resources.mfpgrnet_code != "" %}[Code]({{ page.resources.mfpgrnet_code }}){% else %}Coming soon{% endif %} | Preparing release |
| Unified Safety Evaluation across Automation Levels | [Paper](https://doi.org/10.1016/j.aap.2025.108273) | {% if page.resources.unified_safety_dataset != "" %}[Dataset]({{ page.resources.unified_safety_dataset }}){% else %}Coming soon{% endif %} | {% if page.resources.unified_safety_code != "" %}[Evaluation code]({{ page.resources.unified_safety_code }}){% else %}Coming soon{% endif %} | Preparing release |

### Recommended contents for each public release

To facilitate reuse and reproducibility, each released resource will ideally include:

- **Dataset** — processed data, metadata, version number, and data-use license;
- **Data dictionary** — variable definitions, units, coordinate systems, and sampling frequencies;
- **Code** — preprocessing, training, inference, and evaluation scripts;
- **Model weights** — pretrained checkpoints where applicable;
- **Benchmark split** — fixed training, validation, and test partitions;
- **Reproduction example** — a minimal example reproducing representative results;
- **Citation information** — DOI, BibTeX, and recommended citation format; and
- **Changelog** — version history and revisions to released data or code.

---

## Selected publications

1. **Gan, S., Pei, X., Ge, Y., et al.** (2022). Multisource Adaption for Driver Attention Prediction in Arbitrary Driving Scenes. *IEEE Transactions on Intelligent Transportation Systems, 23*(11), 20912–20925. https://doi.org/10.1109/TITS.2022.3177640
2. **Wang, Q., Gan, S., Chen, W., Li, Q., & Nie, B.** (2021). A data-driven, kinematic feature-based, near real-time algorithm for injury severity prediction of vehicle occupants. *Accident Analysis & Prevention, 156*, 106149. https://doi.org/10.1016/j.aap.2021.106149
3. **Lu, T., Kuang, G., Xu, D., et al.** (2025). A physics-informed attention model for integrated driving risk assessment. *Accident Analysis & Prevention, 223*, 108266. https://doi.org/10.1016/j.aap.2025.108266
4. **Shen, J., Qin, D., He, Z., et al.** (2025). A unified experimental framework for estimating collision rates and occupant injury severity across different levels of driving automation. *Accident Analysis & Prevention, 223*, 108273. https://doi.org/10.1016/j.aap.2025.108273
5. **Lu, T., He, Z., Chang, J., et al.** (2026). A knowledge-guided multi-fidelity residual learning framework for occupant injury prediction from crash simulations. *Advanced Engineering Informatics, 76*, 105117. https://doi.org/10.1016/j.aei.2026.105117
