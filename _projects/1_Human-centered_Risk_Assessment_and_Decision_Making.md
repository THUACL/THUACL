---
layout: page
title: Human-centered Risk Assessment & Decision Making
description: This page presents our group's work on full-process risk quantification and safety decision-making in hazardous scenarios, with partial datasets and code released to facilitate further research in the community.
importance: 1
category: research
giscus_comments: false # 关闭评论
related_publications: false # 关闭引用文献区块
img: assets/img/Research_1/injury risk prediction.jpg

# Public resources. Update the empty URLs when datasets/code are released.
resources:
  ada_code: "https://github.com/ShunGan/ADA"
  vehicle_crash_database: "https://github.com/wangqf1997/Vehicle-Crash-Database"
  piram_code: ""
  piram_dataset: ""
  mfpgrnet_code: ""
  mfpgrnet_dataset: ""
---

> **Research objective.** We develop human-centered methods that integrate **risk perception, physics-informed risk assessment, occupant injury prediction, and injury-aware decision making**, bridging pre-crash collision avoidance with in-crash injury mitigation to support safer and more integrated intelligent vehicle systems. 

This research direction is organized around four components:

1. **Human-centered risk perception** — understanding what human drivers attend to and how latent hazards are perceived.
2. **Physics-informed risk assessment** — jointly quantifying collision probability and collision severity under dynamic traffic interactions.
3. **Occupant injury risk prediction** — predicting biomechanical consequences efficiently across different levels of model fidelity.
4. **Injury-aware decision making** — integrating collision occurrence and occupant injury into a unified safety evaluation framework and providing the basis for injury-aware automated-driving decisions.

---

## Background

The rapid development of intelligent vehicles is reshaping how road safety is assessed and managed. Conventional vehicle safety research has largely treated active safety and passive safety as separate problems which becomes increasingly restrictive in safety-critical scenarios. **A comprehensive safety framework therefore requires intelligent vehicles to understand how the risk develops, how severe the resulting impact may be, and what consequences different outcomes may have for occupants.** Addressing this problem requires linking human risk perception, vehicle interaction dynamics, collision probability and severity, and occupant biomechanics within a unified framework. **Our research investigates this complete pre-crash-to-injury process by combining human behavioral data, physics-informed modeling, driving simulation, injury biomechanics, and data-driven learning.** The objective is to establish a human-centered representation of driving risk that connects traffic-level interactions with vehicle-level collision dynamics and occupant-level injury outcomes, thereby providing a quantitative basis for safety assessment, automated-driving decision support, and the coordinated design of active and passive safety systems.


<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Research_1/background.jpg" title="Temporal evolution of risk in hazardous scenarios" class="img-fluid rounded z-depth-1" %} 
    </div>
</div>
<div class="caption">
    Temporal evolution of risk in hazardous scenarios
</div>

---

## 1. Human-centered risk perception

### Adaptive driver attention prediction across heterogeneous driving scenes

Human drivers do not process all visual information equally. Their attention is selectively allocated to traffic elements that are relevant to the current driving task, including interacting road users and potential hazards. Understanding this process provides an important behavioral basis for human-centered intelligent driving systems.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Research_1/paper_gan_2022_1.jpg" title="Illustration of adaptive driver attention prediction in different driving scenes collected from multiple datasets" class="img-fluid rounded z-depth-1" %} 
    </div>
</div>
<div class="caption">
    Illustration of adaptive driver attention prediction in different driving scenes collected from multiple datasets
</div>

We developed an **Adaptive Driver Attention (ADA)** model to predict driver visual attention across heterogeneous driving environments. The model is inspired by the two principal mechanisms of human visual attention:

- **Bottom-up attention**, in which salient visual stimuli attract attention automatically.
- **Top-down attention**, in which drivers adjust perceptual priorities according to the current driving task and traffic context.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Research_1/paper_gan_2022.jpg" title="Overview of Adaptive Driver Attention" class="img-fluid rounded z-depth-1" %} 
    </div>
</div>
<div class="caption">
    Overview of Adaptive Driver Attention. The gray blocks refer to generic modules for all datasets; the gray blocks with red border lines depict that the domain-specific batch normalization was embedded into the generic module; the colored blocks refer to adaptive modules for specific datasets
</div>

The ADA framework combines generic feature encoders with scene-adaptive attention modules to reproduce human-like perceptual patterns while addressing substantial domain shifts among driver-attention datasets. The model incorporates **domain-specific batch normalization, Gaussian priors, smoothing filters, spatial attention, channel attention, and domain-specific focal loss** to mitigate heterogeneity arising from different video sources, gaze-collection protocols, scene distributions, and saliency-map generation procedures.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Research_1/paper_gan_2022_2.jpg" title="Qualitative evaluation on four driver attention datasets" class="img-fluid rounded z-depth-1" %} 
    </div>
</div>
<div class="caption">
    Qualitative evaluation on four driver attention datasets
</div>

The model was jointly trained on four public driver-attention datasets — **BDD-A, DADA-2000, DReyeVE, and EyeTrack** — and further evaluated on **PSAD**, which was not used for joint training. The results show that the model can generalize across heterogeneous traffic scenes and reproduce characteristic human attention patterns in cruising, turning, conflict, and accident-related situations.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Research_1/paper_gan_2022_3.jpg" title="Performance comparision on EyeTrack and DADA-2000" class="img-fluid rounded z-depth-1" %} 
    </div>
</div>
<div class="caption">
</div>

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Research_1/paper_gan_2022_4.jpg" title="Performance comparision on BDD-A, DReyeVE and PSAD" class="img-fluid rounded z-depth-1" %} 
    </div>
</div>
<div class="caption">
    Performance comparision on four public driver-attention datasets (using saliency evaluation metrics. Symbol ↑ expects a large value and ↓ expects a smaller value)
</div>

Importantly, the predicted attention maps can shift toward **latent conflict regions and relevant interacting vehicles**, providing a basis for identifying where human drivers are likely to allocate attention before and during safety-critical events.

<div style="
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 18px;
  width: 100%;
  max-width: 900px;
  margin: 24px auto 0 auto;
">

  <img
    src="{{ '/assets/img/Research_1/paper_gan_2022_5.gif' | relative_url }}"
    alt="Driver gaze saliency prediction example 1"
    style="
      width: 100%;
      height: auto;
      display: block;
      border-radius: 8px;
    "
  >

  <img
    src="{{ '/assets/img/Research_1/paper_gan_2022_7.gif' | relative_url }}"
    alt="Driver gaze saliency prediction example 3"
    style="
      width: 100%;
      height: auto;
      display: block;
      border-radius: 8px;
    "
  >

</div>

<div class="caption" style="text-align: center; margin-top: 12px;">
  Driver gaze saliency based on model prediction
</div>

[Paper](https://doi.org/10.1109/TITS.2022.3177640) · [Code]({{ page.resources.ada_code }})

---



## 2. Physics-informed risk assessment

### Integrated assessment of collision probability and collision severity

Conventional surrogate safety indicators often characterize only one dimension of a hazardous interaction. For example, time-to-collision can indicate temporal urgency in longitudinal conflicts but may be less informative for complex lateral interactions. More importantly, two situations with similar collision probability can lead to substantially different collision consequences.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Research_1/paper_lu_2025_2.png" title="Quantification of integrated driving risk" class="img-fluid rounded z-depth-1" %} 
    </div>
</div>
<div class="caption">
    Quantification of integrated driving risk
</div>


We therefore define driving risk by jointly considering **the probability of collision and the potential severity of the collision**:

$$
\mathrm{Risk} = \mathrm{Collision\ Probability} \times \mathrm{Collision\ Severity}.
$$

To operationalize this concept, we developed a **Physics-Informed Integrated Risk Assessment Model (PIRAM)**. PIRAM predicts collision time and collision velocity from vehicle-interaction histories and map information, then transforms these predictions into collision probability and collision severity and combines them into an **Integrated Driving Risk (IDR)** metric.



### Physics-informed risk assessment

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Research_1/paper_lu_2025_1.jpg" title="Overall architecture of the PNN in physics-informed integrated risk assessment model" class="img-fluid rounded z-depth-1" %} 
    </div>
</div>
<div class="caption">
    Overall architecture of the PNN in physics-informed integrated risk assessment model
</div>

PIRAM combines a data-driven neural network with a physics-based prediction model:

- **Interaction and map encoding.** Historical trajectories of the ego vehicle and hazard-triggering vehicle are represented together with surrounding lane geometry.
- **Cross-attention and self-attention.** Attention mechanisms capture spatial and temporal dependencies among interacting vehicles and the road environment.
- **Multi-task prediction.** The neural network jointly predicts collision time and collision velocity.
- **Vehicle-dynamics constraints.** A dynamic bicycle model and drivable-area prediction provide physical constraints during training, reducing predictions that conflict with feasible vehicle motion.

This hybrid design is intended to preserve the representation capability of data-driven models while improving **prediction stability, physical consistency, and reliability** in safety-critical conditions.

### Integrated Driving Risk Prediction Dataset (IDRPD)

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Research_1/paper_lu_2025_3.png" title="Establishment of the integrated driving risk prediction dataset" class="img-fluid rounded z-depth-1" %} 
    </div>
</div>
<div class="caption">
    Establishment of the integrated driving risk prediction dataset
</div>

To support this work, we constructed the **Integrated Driving Risk Prediction Dataset (IDRPD)** from driver-in-the-loop experiments in a high-fidelity driving simulator. The dataset contains **1,879 safety-critical interaction events** involving:

- cut-in conflicts, merging conflicts, emergency-braking conflicts
- driver collision-avoidance maneuvers, non-collision and collision outcomes
- collision timing and collision conditions

Data were recorded at 20 Hz around the hazard-triggering event. A sliding-time-window augmentation strategy was further used to represent the temporal evolution from normal driving to escalating risk and eventual collision or successful avoidance.

### Main findings

Compared with baseline physics-based and data-driven approaches, PIRAM improved the prediction of both collision probability and collision severity. The study reported improvements in prediction accuracy of approximately **7.9% for collision probability** and **3.2% for collision severity**, together with improved temporal stability. Case studies further showed that the model can identify the escalation of integrated driving risk earlier than conventional approaches, providing additional time for safety intervention and occupant-protection strategies.

<div style="
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 20px;
  width: 100%;
  max-width: 1000px;
  margin: 24px auto 0 auto;
">

  <div style="text-align: center;">
    {% include figure.liquid
      loading="eager"
      path="assets/img/Research_1/paper_lu_2025_5.png"
      title="Integrated driving risk prediction performance of each model in the emergency avoidance scenario from driving simulator"
      class="img-fluid rounded z-depth-1"
    %}

    <div class="caption" style="text-align: center; margin-top: 8px;">
      Performance comparision in the emergency avoidance scenario from driving simulator
    </div>
  </div>

  <div style="text-align: center;">
    {% include figure.liquid
      loading="eager"
      path="assets/img/Research_1/paper_lu_2025_4.png"
      title="Integrated driving risk prediction performance of each model in a real collision case"
      class="img-fluid rounded z-depth-1"
    %}

    <div class="caption" style="text-align: center; margin-top: 8px;">
      Performance comparision in a real collision case
    </div>
  </div>

</div>

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

## 3. Occupant injury severity prediction

Accurate prediction of occupant injury severity is an important component of integrated vehicle safety, providing quantitative injury information for both pre-crash trajectory planning and in-crash occupant protection. However, occupant injury is governed by complex interactions among vehicle crash dynamics, occupant characteristics, and restraint conditions, making rapid and reliable injury assessment challenging. To address this problem, our research has progressively developed from large-scale simulation-based injury modeling to multi-fidelity learning that combines heterogeneous crash data with different levels of biomechanical fidelity.

We first established a large-scale numerical crash database covering diverse frontal impact conditions, with a particular focus on occupant kinematic and biomechanical responses. Deep learning architectures were initially employed to learn the nonlinear relationship between crash dynamics and occupant injury outcomes. To support near-real-time applications, we subsequently extracted compact and physically interpretable kinematic features from vehicle crash pulses and combined them with low-complexity machine-learning models. This substantially reduced computational cost while maintaining reliable injury prediction performance.

Building on this simulation-based framework, our subsequent work addressed a major limitation of occupant injury modeling: the scarcity and computational cost of high-fidelity crash data. High-fidelity simulations provide more realistic representations of human injury mechanisms but are expensive to generate at scale, whereas low-fidelity simulations are more accessible but contain systematic modeling biases. To exploit the complementary strengths of these data sources, we developed a knowledge-guided multi-fidelity learning framework that transfers structured physical knowledge from abundant low-fidelity simulations to sparse high-fidelity injury data. The framework retrieves relevant low-fidelity prototypes for each high-fidelity case and decomposes injury prediction into a transferable low-fidelity trend component and a high-fidelity residual correction. This approach improves sample efficiency and prediction accuracy under limited high-fidelity supervision, providing a more scalable basis for virtual safety assessment, occupant protection design, and injury-aware vehicle decision-making.


### 3.1 Near-real-time occupant injury severity prediction

The study first used sequence models to learn the nonlinear relationship between vehicle crash pulses and occupant kinematic responses. A convolutional neural network achieved high prediction performance but remained too computationally expensive for time-critical onboard applications. To reduce model complexity, network visualization was used to examine how the high-accuracy model processed crash-pulse information. A two-layer pooling procedure then compressed the original **120-dimensional crash pulse into three kinematic features**. These features were combined with occupant and restraint information in a lightweight machine-learning model. 

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Research_1/paper_wang_2021_1.png" title="Technical framework of near real-time occupant injury prediction" class="img-fluid rounded z-depth-1" %} 
    </div>
</div>
<div class="caption">
    Technical framework of near real-time occupant injury prediction
</div>

The final model achieved:
- **85.4% accuracy** for head injury severity on the numerical dataset,
- **78.7% accuracy** on an independent **192-case real-world crash dataset**, and
- approximately **1.2 ± 0.4 ms** prediction time.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Research_1/paper_wang_2021_3.png" title="Randomly selected examples of normalized head acceleration prediction." class="img-fluid rounded z-depth-1" %} 
    </div>
</div>
<div class="caption">
    Randomly selected examples of normalized head acceleration prediction.
</div>

The performance of the SVM-based prediction model was further evaluated using a 192-case real-world collision dataset. Considering the heterogeneity between the numerical database and the real-world dataset, we retrained the prediction model and adopted five-fold cross-validation to assess it comprehensively. The prediction accuracy, 
precision, recall, and AUC were 78.7 %, 0.636, 0.787, and 0.698, respectively.

#### Occupant injury severity prediction algorithm

The study was conducted in two stages: (1) developing a deep learning model for accurate occupant injury prediction across seven severity classes; and (2) using model visualization to identify simplified crash-dynamic features and develop a near-real-time injury prediction model that achieves accurate prediction with substantially reduced computational cost.

Specifically, the RNN-based model adopted a conventional encoder–decoder architecture with long short-term memory (LSTM) units, whereas the CNN-based model employed a temporal convolutional network (TCN) with causal and dilated convolutions to capture temporal dependencies using only past information. The models took vehicle crash pulses, occupant gender, and seatbelt and airbag use as inputs and predicted occupant kinematic and biomechanical responses, including head acceleration, chest displacement, neck force, and neck moment, which were subsequently converted into AIS injury levels. All input variables were embedded into high-dimensional representations through lookup tables before being fed into the hidden layers. Both models were trained using the adaptive moment estimation (Adam) optimizer with learning-rate decay. To reduce overfitting, we applied L2 regularization, dropout in the input and intermediate layers, and early stopping when the validation loss increased for five consecutive epochs. Hyperparameters for both models were selected using grid search.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Research_1/paper_wang_2021.png" title="Optimized architectures of RNN and CNN" class="img-fluid rounded z-depth-1" %} 
    </div>
</div>
<div class="caption">
    Optimized architectures of RNN and CNN
</div>

#### Large-scale numerical and real-world crash databases

The efficiency of the occupant injury prediction algorithm depends largely on the quality of the database, which is used for training and validation. A large-scale numerical database containing **28,000 frontal collision cases** was constructed by combining finite-element, multi-body, and lumped-parameter simulation models. The database covers occupant kinetics and injury responses with variations in vehicle crash pulse, occupant gender, and restraint configuration. Vehicle crash pulses with delta-v ranging from 40 km/h to 60 km/h with an interval of 10 km/h, and impact angles ranging from -20◦ to 10◦ with an interval of 10◦ were obtained from FE simulations.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Research_1/paper_wang_2021_2.png" title="Ratios of cases with different AIS levels in the numerical database" class="img-fluid rounded z-depth-1" %} 
    </div>
</div>
<div class="caption">
    Ratios of cases with different AIS levels in the numerical database
</div>

We also constructed a small-sized dataset of real-world MVCs to further validate the developed injury prediction model’s performance by screening vehicle crash cases from the National Automotive Sampling System/Crashworthiness Data System (NASS/CDS).  We then excluded crash cases with multiple impacts or with vehicle body types differing from the sedan model used in the numerical database, such as pickup, utility, and van. Finally, the validation dataset contained **192 frontal collision cases** with occupant injury AIS levels for the head, neck, and chest ranging from 0 to 6. Both the numerical training database and the real-world validation dataset are available.


[Paper](https://doi.org/10.1016/j.aap.2021.106149) · [vehicle-crash database]({{ page.resources.vehicle_crash_database }})

---

### 3.2 Knowledge-guided multi-fidelity injury prediction

High-fidelity finite-element crash simulations provide detailed biomechanical information, but their computational cost makes it difficult to generate sufficiently large datasets, particularly during early-stage vehicle development. Low-fidelity multi-body simulations are substantially cheaper but contain systematic bias because they simplify tissue deformation, contact mechanics, and local anatomical responses.

To exploit the complementary strengths of both fidelity levels, we further developed **MF-PGRNet**, a knowledge-guided multi-fidelity residual learning framework for occupant injury prediction under limited high-fidelity supervision. Rather than treating low-fidelity simulations simply as additional training samples, the framework uses them as a source of **structured physics priors**. The high-fidelity prediction task is decomposed into:

$$
y_{HF}
= f_{trend}(x)
+ f_{residual}(x),
$$

where the first term represents a transferable physical trend learned from abundant low-fidelity simulations, and the second term corrects the fidelity-specific biomechanical gap using sparse high-fidelity observations.

Under limited high-fidelity supervision, MF-PGRNet reduced prediction errors by **more than 16%** relative to representative baselines and achieved performance comparable to models trained with substantially more high-fidelity data. The results show that cross-fidelity physics knowledge can reduce reliance on expensive finite-element simulations while maintaining useful injury-prediction performance for virtual safety assessment.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Research_1/paper_lu_2026_1.png" title="Evaluation of models with different training set sizes" class="img-fluid rounded z-depth-1" %} 
    </div>
</div>
<div class="caption">
    Evaluation of models with different training set sizes
</div>

### Core modules of the proposed framework

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Research_1/paper_lu_2026.png" title="Training framework of the proposed MF-PGRNet for occupant injury prediction" class="img-fluid rounded z-depth-1" %} 
    </div>
</div>
<div class="caption">
    Training framework of the proposed MF-PGRNet for occupant injury prediction
</div>

- **Shared multi-modal encoding** for bi-axial crash pulses, restraint parameters, and occupant characteristics.
- **Knowledge representation and retrieval via learnable prototype library** to capture representative physical patterns from low-fidelity (LF) simulations, combined with prototype cross-attention to dynamically retrieve relevant physics priors for each high-fidelity (HF) query.
- **Physics-prior guided residual learning** that explicitly decomposes injury prediction into two subtasks: trend estimation and residual correction.
- **Loss function design** for joint optimization of predictive accuracy and geometric alignment.


### Multi-fidelity crash datasets

We constructed two large-scale numerical datasets with varying fidelity levels. Although both datasets span the same input parameter space, they differ significantly in model biofidelity and computational complexity. The simulations include crash configuration parameters, restraint-system variables, occupant characteristics and posture, crash-pulse histories, and injury metrics such as HIC, Nij, and maximum chest deflection.
- **Low-fidelity cases** generated through vehicle crash-pulse simulation and multi-body occupant dynamics; and
- **High-fidelity finite-element cases** with each high-fidelity simulation requiring approximately **45 h on 24 CPU cores** on average.


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

## 4. Injury-aware decision making and safety performance evaluation


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
[Paper](https://doi.org/10.1016/j.aap.2025.108273) 

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

---

## References

1. **Gan, S., Pei, X., Ge, Y., et al.** (2022). Multisource Adaption for Driver Attention Prediction in Arbitrary Driving Scenes. *IEEE Transactions on Intelligent Transportation Systems, 23*(11), 20912–20925. https://doi.org/10.1109/TITS.2022.3177640
2. **Wang, Q., Gan, S., Chen, W., Li, Q., & Nie, B.** (2021). A data-driven, kinematic feature-based, near real-time algorithm for injury severity prediction of vehicle occupants. *Accident Analysis & Prevention, 156*, 106149. https://doi.org/10.1016/j.aap.2021.106149
3. **Lu, T., Kuang, G., Xu, D., et al.** (2025). A physics-informed attention model for integrated driving risk assessment. *Accident Analysis & Prevention, 223*, 108266. https://doi.org/10.1016/j.aap.2025.108266
4. **Shen, J., Qin, D., He, Z., et al.** (2025). A unified experimental framework for estimating collision rates and occupant injury severity across different levels of driving automation. *Accident Analysis & Prevention, 223*, 108273. https://doi.org/10.1016/j.aap.2025.108273
5. **Lu, T., He, Z., Chang, J., et al.** (2026). A knowledge-guided multi-fidelity residual learning framework for occupant injury prediction from crash simulations. *Advanced Engineering Informatics, 76*, 105117. https://doi.org/10.1016/j.aei.2026.105117
