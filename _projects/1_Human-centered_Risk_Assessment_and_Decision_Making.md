---
layout: page
title: Human-centered Risk Assessment & Decision Making of Autonomous Vehicle
description: Human-centered methods that connect driver risk perception, occupant injury severity prediction, and safety performance evaluation of autonomous vehicles, bridging pre-crash collision avoidance and in-crash injury mitigation.
importance: 1
category: research
giscus_comments: false # 关闭评论
related_publications: false # 关闭引用文献区块
img: assets/img/Research_1/injury risk prediction.jpg

# Public resources. Update the empty URLs when datasets/code are released.
resources:
  ada_code: "https://github.com/ShunGan/ADA"
  vehicle_crash_database: "https://github.com/wangqf1997/Vehicle-Crash-Database"
  experimental_dataset: ""
---

<style>
/* 页面内隐藏 description，卡片列表和 SEO meta 不受影响 */
.post-description {
  display: none;
}
</style>

> **Research objective.** We develop human-centered methods that integrate **risk perception, physics-informed risk assessment, occupant injury severity prediction, and injury-aware decision making**, bridging pre-crash collision avoidance with in-crash injury mitigation to support safer and more integrated intelligent vehicle safety systems. 

1. **Human-centered risk perception** — understanding what human drivers attend to and how latent hazards are perceived. We developed an adaptive driver attention model that effectively addresses heterogeneity across multi-source datasets and can be readily generalized to arbitrary traffic scenarios through domain adaptation.
2. **Occupant injury severity prediction** — enabling timely and accurate prediction of occupant biomechanical consequences from initial crash conditions and occupant characteristics, including crash pulses, impact speed, restraint system parameters, and occupant attributes, with a prediction accuracy of approximately 85%. 
3. **Safety performance evaluation and injury-aware decision making** — integrating collision occurrence and occupant injury severity within a unified experimental framework to enable consistent safety performance evaluation and provide a quantitative basis for injury-aware decision-making in safety-critical scenarios.


| Paper | Data & Code |
| :--- | :---: |
| [**Gan, S., et al.** (2022). Multisource Adaption for Driver Attention Prediction in Arbitrary Driving Scenes. *IEEE Transactions on Intelligent Transportation Systems, 23*(11), 20912–20925. https://doi.org/10.1109/TITS.2022.3177640](https://doi.org/10.1109/TITS.2022.3177640) | {% if page.resources.ada_code and page.resources.ada_code != "" %}[ADA code]({{ page.resources.ada_code }}){% else %}ADA code{% endif %} |
| [**Wang, Q., et al.** (2021). A data-driven, kinematic feature-based, near real-time algorithm for injury severity prediction of vehicle occupants. *Accident Analysis & Prevention, 156*, 106149. https://doi.org/10.1016/j.aap.2021.106149](https://doi.org/10.1016/j.aap.2021.106149) | {% if page.resources.vehicle_crash_database and page.resources.vehicle_crash_database != "" %}[Vehicle-crash dataset]({{ page.resources.vehicle_crash_database }}){% else %}Vehicle-crash dataset{% endif %} |
| [**Shen, J., et al.** (2025). A unified experimental framework for estimating collision rates and occupant injury severity across different levels of driving automation. *Accident Analysis & Prevention, 223*, 108273. https://doi.org/10.1016/j.aap.2025.108273](https://doi.org/10.1016/j.aap.2025.108273) | {% if page.resources.experimental_dataset and page.resources.experimental_dataset != "" %}[Experimental dataset]({{ page.resources.experimental_dataset }}){% else %}Experimental dataset (preparing release){% endif %} |

---

## Background

The rapid development of intelligent vehicles is reshaping how road safety is assessed and managed. **A comprehensive safety framework requires intelligent vehicles to understand, as humans do, how risk develops, how severe the resulting impact may be, and what consequences different outcomes may have for occupants.** Addressing this problem requires integrating human risk perception, driving risk quantification, post-crash occupant injury severity evaluation, and injury-risk-minimizing safety decision-making within a unified framework. The objective of our research is to establish a human-centered representation of driving risk that connects traffic-level interactions with vehicle-level collision dynamics and occupant-level injury outcomes, thereby providing a quantitative basis for automated-driving decision support and safety assessment.


<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Research_1/background.jpg" title="Temporal evolution of risk in hazardous scenarios" class="img-fluid rounded z-depth-1" %} 
    </div>
</div>
<div class="caption">
    Temporal evolution of risk in hazardous scenarios
</div>

---

## 1. [Human-centered risk perception](https://doi.org/10.1109/TITS.2022.3177640)

### 1.1 Framework

Human drivers do not process all visual information equally. Their attention is selectively allocated to traffic elements that are relevant to the current driving task, including interacting road users and potential hazards. Understanding this process provides an important behavioral basis for human-centered intelligent driving systems.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Research_1/paper_gan_2022_1.jpg" title="Illustration of adaptive driver attention prediction in different driving scenes collected from multiple datasets" class="img-fluid rounded z-depth-1" %} 
    </div>
</div>
<div class="caption">
    Illustration of adaptive driver attention prediction in different driving scenes collected from multiple datasets
</div>

### 1.2 Methods

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

### 1.3 Results
The model was jointly trained on four public driver-attention datasets — **BDD-A, DADA-2000, DReyeVE, and EyeTrack** — and further evaluated on **PSAD**, which was not used for joint training. The results show that the model can generalize across heterogeneous traffic scenes and reproduce characteristic human attention patterns in cruising, turning, conflict, and accident-related situations. Importantly, the predicted attention maps can shift toward **latent conflict regions and relevant interacting vehicles**, providing a basis for identifying where human drivers are likely to allocate attention before and during safety-critical events.

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

---


## 2. [Occupant injury severity prediction](https://doi.org/10.1016/j.aap.2021.106149)

Accurate prediction of occupant injury severity is an important component of integrated vehicle safety, providing quantitative injury information for both pre-crash trajectory planning and in-crash occupant protection. However, occupant injury is governed by complex interactions among vehicle crash dynamics, occupant characteristics, and restraint conditions, making rapid and reliable injury assessment challenging. To address this problem, We first established a large-scale numerical crash database covering diverse frontal impact conditions, with a particular focus on occupant kinematic and biomechanical responses. Deep learning architectures were initially employed to learn the nonlinear relationship between crash dynamics and occupant injury outcomes. To support near-real-time applications, we subsequently extracted compact and physically interpretable kinematic features from vehicle crash pulses and combined them with low-complexity machine-learning models. This substantially reduced computational cost while maintaining reliable injury prediction performance.

### 2.1 Framework 
The study first used sequence models to learn the nonlinear relationship between vehicle crash pulses and occupant kinematic responses. A convolutional neural network achieved high prediction performance but remained too computationally expensive for time-critical onboard applications. To reduce model complexity, network visualization was used to examine how the high-accuracy model processed crash-pulse information. A two-layer pooling procedure then compressed the original **120-dimensional crash pulse into three kinematic features**. These features were combined with occupant and restraint information in a lightweight machine-learning model. 

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Research_1/paper_wang_2021_1.png" title="Technical framework of near real-time occupant injury prediction" class="img-fluid rounded z-depth-1" %} 
    </div>
</div>
<div class="caption">
    Technical framework of near real-time occupant injury prediction
</div>

### 2.2 Dataset
The efficiency of the occupant injury prediction algorithm depends largely on the quality of the database, which is used for training and validation. A large-scale numerical database containing **28,000 frontal collision cases** was constructed by combining finite-element, multi-body, and lumped-parameter simulation models. The database covers occupant kinetics and injury responses with variations in vehicle crash pulse, occupant gender, and restraint configuration. Vehicle crash pulses with delta-v ranging from 40 km/h to 60 km/h with an interval of 10 km/h, and impact angles ranging from -20◦ to 10◦ with an interval of 10◦ were obtained from FE simulations.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Research_1/paper_wang_2021_2.png" title="Ratios of cases with different AIS levels in the numerical database" class="img-fluid rounded z-depth-1" %} 
    </div>
</div>
<div class="caption">
    Ratios of cases with different AIS levels in the numerical database
</div>

We also constructed a small-sized dataset of real-world MVCs to further validate the developed injury prediction model’s performance by screening vehicle crash cases from the National Automotive Sampling System/Crashworthiness Data System (NASS/CDS).  We then excluded crash cases with multiple impacts or with vehicle body types differing from the sedan model used in the numerical database, such as pickup, utility, and van. Finally, the validation dataset contained **192 frontal collision cases** with occupant injury AIS levels for the head, neck, and chest ranging from 0 to 6. Both the numerical training database and the real-world validation dataset are available.


### 2.3 Methods
The RNN-based injury severity prediction model adopted a conventional encoder–decoder architecture with long short-term memory (LSTM) units, whereas the CNN-based model employed a temporal convolutional network (TCN) with causal and dilated convolutions to capture temporal dependencies using only past information. The models took vehicle crash pulses, occupant gender, and seatbelt and airbag use as inputs and predicted occupant kinematic and biomechanical responses, including head acceleration, chest displacement, neck force, and neck moment, which were subsequently converted into AIS injury levels. All input variables were embedded into high-dimensional representations through lookup tables before being fed into the hidden layers. Both models were trained using the adaptive moment estimation (Adam) optimizer with learning-rate decay. To reduce overfitting, we applied L2 regularization, dropout in the input and intermediate layers, and early stopping when the validation loss increased for five consecutive epochs. Hyperparameters for both models were selected using grid search.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Research_1/paper_wang_2021.png" title="Optimized architectures of RNN and CNN" class="img-fluid rounded z-depth-1" %} 
    </div>
</div>
<div class="caption">
    Optimized architectures of RNN and CNN
</div>

### 2.4 Results

On the numerical dataset, the final model predicted head injury severity with an accuracy of **85.4%**. To examine whether this performance transfers beyond simulation, the model was further evaluated on an independent dataset of **192 real-world collisions**. Considering the heterogeneity between the numerical database and real-world crash records, the model was retrained on this dataset and assessed using five-fold cross-validation, yielding an accuracy of **78.7%**, a precision of 0.636, a recall of 0.787, and an AUC of 0.698. The moderate decrease relative to the numerical dataset reflects the greater variability of real-world crash conditions, yet the model still recovers the dominant relationship between crash pulse, occupant characteristics, and injury outcome. Crucially, prediction requires only about **1.2 ± 0.4 ms** per case, which is several orders of magnitude faster than finite element simulation and therefore fast enough to inform occupant protection decisions within the crash itself.





## 3. [Safety performance evaluation and injury-aware decision making](https://doi.org/10.1016/j.aap.2025.108273)

### 3.1 Experimental framework
Existing real-world datasets for evaluating the safety protection performance of automated vehicles differ substantially from those of conventional vehicles in terms of accumulated mileage, crash types, and other characteristics, making direct comparisons across manufacturers and levels of driving automation difficult. To address this issue, we developed a unified driving-simulator-based framework for evaluating the safety protection performance of automated vehicles. The framework integrates automated vehicle models at different levels of automation, an accelerated generation algorithm for highway safety-critical scenarios, and a data-driven occupant injury quantification model, enabling comprehensive and fair comparisons under standardized crash conditions, consistent levels of scenario urgency, and unified evaluation metrics.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Research_1/paper_shen_2025_1.jpg" title="Highway simulated safety-critical scenarios and experimental procedure" class="img-fluid rounded z-depth-1" %} 
    </div>
</div>
<div class="caption">
    Highway simulated safety-critical scenarios and experimental procedure
</div>

The framework uses a high-fidelity driving simulator and parameterized hazard-triggering algorithms to generate three representative highway conflict types: Braking (a leading vehicle performs sudden emergency braking), Cut-in (a surrounding vehicle abruptly enters the ego vehicle's lane), and Merging (a surrounding vehicle competes for the ego vehicle's target lane during a lane change). The study finally collected 30 valid participants and 1,859 safety-critical vehicle interactions which contains 337 collisions. The unified experimental framework records the progression from normal driving through hazard triggering, decision making, driver intervention or disengagement, collision avoidance, and collision. 

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Research_1/paper_shen_2025_4.jpg" title="Distribution of safety performance evaluation datasets across different levels of driving automation" class="img-fluid rounded z-depth-1" %} 
    </div>
</div>
<div class="caption">
    Distribution of safety performance evaluation datasets across different levels of driving automation
</div>



### 3.2 Safety performance evaluation

Vehicle safety is often assessed primarily by crash rate. These indicators are necessary but incomplete: they do not describe the consequences for vehicle occupants when collision avoidance fails. We therefore developed a metric (**unified safety benefit**) that evaluates automated-driving safety through both **collision occurrence and occupant injury severity** under comparable safety-critical scenarios.


Vehicles representing **attentive manual driving (selective SAE L0), SAE L2, L3, and L4 automation** were evaluated under comparable road scenarios, similar urgency levels, and consistent metrics. Under the designed safety-critical scenarios, the study reported:

| Automation level | Collision rate | Probability of severe occupant injury | Unified safety benefit
| --- | ---: | ---: | ---: |
| selective L0 | 12.6% | 8.6% | 94.0% |
| L2 | 24.3% | 11.1% | 90.0% |
| L3 | 21.4% | 21.6% | 89.0% |
| L4 | 14.1% | 9.2% | 95.0% |

The results show that **a lower collision rate does not necessarily translate directly into a proportionally higher overall safety benefit**. In the experiment, Level 3 automation reduced collision occurrence relative to Level 2 but exhibited higher injury severity when collisions occurred, resulting in comparable unified safety benefits. Level 4 achieved a higher overall safety benefit primarily through a lower collision rate, while the reduction in residual-collision injury severity was more limited. Therefore, collision avoidance is necessary, but collision avoidance alone is not sufficient to characterize the safety performance of an automated vehicle.


### 3.3 Injury-aware decision making

The current published framework establishes the experimental and quantitative basis for injury-aware decision support. A fully closed-loop controller that directly optimizes automated-driving trajectories using predicted injury outcomes is an ongoing research direction.



