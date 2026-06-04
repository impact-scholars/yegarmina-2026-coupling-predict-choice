---
title: Bilateral hemispheric coupling is widespread but dissociated from choice encoding across the mouse brain
abstract: |
    Hemispheric interactions during cognition reflect the division and integration of neural resources required to transform perception into action. Using the IBL Brain-wide Map, comprising large-scale Neuropixels recordings from mice performing a visual decision-making task, we characterize interhemispheric coordination across 39 brain regions during choice behavior. Canonical Correlation Analysis (CCA) applied to bilateral recording sessions reveals that interhemispheric coupling is widespread (r = 0.25–0.96) but functionally dissociated from choice encoding: regions with the strongest bilateral synchrony do not necessarily represent the animal's decision most strongly. Zero-lag dominance across regions supports simultaneous common-input drive over sequential hemispheric relay. Our findings reveal dissociated organizational principles governing interhemispheric coordination and decision-specific lateralization, and provide a reusable pipeline for characterizing bilateral neural dynamics across large-scale recording datasets. 
acknowledgments: |
    We extend our sincere thanks to the Neuromatch Academy and the Impact Scholars program for the opportunity, resources and funding to develop this work. We will fondly remember our mentor, Nader Nikbakht, for his unwavering guidance, constructive feedback, great humor and fascinating insights into the world of systems neuroscience. Thank you for sharing your passion and allowing ours to grow. We promise to cherish and carry forward the mentorship you offered as we grow with this field.
---

# 1. Introduction

Rodent decision-making research has identified region-specific neural contributions: anterior cingulate cortex encodes cost-benefit analysis (Hillman & Bilkey, 2010), ventral tegmental dopaminergic neurons signal reward magnitude and delay (Roesch et al., 2007), and orbitofrontal cortex prepares expected outcome values for learning (Miller et al., 2022) — a role subsequently confirmed in humans and macaques (Noonan et al., 2017). Yet most rodent neuroscience observes the brain unilaterally, overlooking behaviorally relevant hemispheric asymmetries including limb preferences (Manns et al., 2021) and anatomical volume asymmetries analogous to human cerebral petalia (Silberfeld et al., 2025). Disregarding hemispheric interactions risks missing crucial asymmetries in neural processing that contribute to cognitive function and commissural organization (Mundorf & Ocklenburg, 2025). Bilateral research is therefore essential: functional lateralization is not a fixed property but arises from learning-dependent synaptic plasticity producing asymmetric cross-hemispheric communication, as demonstrated by unilateral manipulation of the anterior lateral motor cortex during tactile decision-making in mice (Ocklenburg & Guo, 2024; Concha et al., 2012). The IBL Brain-wide Map — 699 Neuropixels insertions across 281 regions, of which 50 insertions across 39 regions were recorded homotopically — provides a uniquely suitable dataset for examining cross-hemispheric communication at scale during a visual decision-making task (IBL, 2022; Meshulam et al., 2025).

# 2. Methodology

## 2.1 Data and Preprocessing
Electrophysiological recordings from N=152 bilateral region-pairs across 39 brain regions in mice performing a visual discrimination task were obtained from the International Brain Laboratory dataset (Meshulam et al., 2025). Regions corresponding to white matter tracts, ventricles, and mapping artifacts (void, root, fiber tracts) were excluded from analysis, yielding 39 brain regions. Spike times were binned at 25 ms resolution and smoothed with a Gaussian kernel (σ ≈ 37 ms, effective temporal resolution ~90 ms). Temporal analyses including cross-correlograms and peak latency estimates should be interpreted with this resolution constraint in mind. All spike-sorted units were included without additional quality filtering beyond the IBL Brainwidemap session-level quality control. While this may include multi-unit or unstable clusters, PCA preferentially captures coordinated population activity, mitigating the impact of individual noisy units on the leading components. Principal Component Analysis reduced population activity to K=3 components per hemisphere, capturing a median of 25.9% of total variance (IQR: 19.6–34.0%) across 614 region-hemisphere entries (median 42 neurons per entry, range 8–561). While this represents a fraction of total neural variance, the leading components capture the dominant shared population dynamics most relevant to bilateral coupling analysis, consistent with low-rank communication subspace approaches (Semedo et al., 2019) (Figure 0 - Total Variance explained).
 <p align="center">
  <img src="figures/Figure_0.png" width="800"/>
</p>. 

## 2.2 Temporal Canonical Correlation Analysis (CCA) with Cross-Validation
PCA scores were re-binned into 80 time intervals (25 ms resolution). Within-hemisphere decision encoding strength was quantified as the Fréchet distance between choice-averaged PCA trajectories (left-choice vs right-choice trials), providing a measure of trajectory separation in the low-dimensional neural manifold independent of bilateral coupling. For each time bin, we then computed canonical variates U (left) and V (right) maximizing Pearson correlation r via 3-fold cross-validation on trials. Peak bilateral coupling (r_peak) was identified as max(r_time) across all time bins. Reported r-values reflect test-fold estimates to guard against overfitting.

## 2.3 Significance Testing
For each region, 500 permutations were generated by shuffling right-hemisphere trial order and recomputing CCA. The 95th percentile of permuted r_peak values served as the significance threshold. Cross-correlograms between U_time and V_time (lags ±500 ms) were assessed against null lag distributions (200 permutations).

## 2.4 Temporal Dynamics and Hemispheric Asymmetry
Bilateral coupling evolution was characterized via coupling_range = max(r_time) − min(r_time), classifying regions as stable (range < 0.10) or dynamic (range ≥ 0.10). Coupling strength was analyzed across three trial phases: pre-stimulus (−1000 to −500 ms), movement (−200 to +200 ms), and post-movement (+500 to +1000 ms). Hemispheric asymmetry was quantified via shape_similarity, temporal_offset, and amplitude_asymmetry. Asynchrony distance and trajectory length in canonical variate space were computed per phase.

## 2.5 Choice Encoding Analysis
Choice was normalized to ±1 and correlated with canonical variates at each time bin: 
bilateral choice encoding = (|r_choice_U(t)| + |r_choice_V(t)|) / 2.

We extracted r_choice_peak, r_choice_mean, and peak_latency per region. Phase-specific choice encoding was computed for each trial phase. Spearman correlation tested whether bilateral coupling independently predicts choice encoding.

## 2.6 Choice Decoding Analysis
To verify that the symmetric bilateral encoding observed in the CCA analysis was not an artifact of CCA's inherent symmetry constraint, an independent population-level decoder (logistic regression with 5-fold cross-validation on the same PCA-reduced subspace) was trained per hemisphere across 614 (region, hemisphere, session) entries.

## 2.7 Statistical Reporting
Region-level summaries were aggregated across sessions using Fisher z-transformation of r values. Confidence intervals were computed as tanh(z_mean ± 1.96×SE_z).

## 2.8 Software
Python 3.10; scikit-learn, numpy, scipy.stats, pandas. All code and reproducible outputs available at [https://github.com/maleeshakarawita/Neuronova].

# 3. Results

## 3.1 Bilateral coupling is brain-wide but spatially heterogeneous. 

Temporal CCA, applied independently at each time bin with cross-validation, is robust to temporal misalignments from spike sorting and circuit-specific timescales, ensuring regional differences in coupling reflect genuine interhemispheric coordination rather than biophysical confounds. All 39 regions showed coupling above permutation null, ranging from r = 0.25 (VISpm2/3) to r = 0.96 (SUB)(Figure 1) <p align="center">
  <img src="figures/Figure_1. Top 15 brain regions ranked by bilateral coupling strength.png" width="900"/>
</p>

Hippocampal formation (SUB: 0.96; CA1: 0.91; DG: 0.85–0.88) and midbrain nuclei (APN: 0.89; MRN: 0.87) showed the strongest coupling; higher visual areas the weakest (Supplementary Figure 1). This heterogeneity argues against a single global synchronization mechanism and suggests an extension of the distributed coding framework (Steinmetz et al., 2019) to the interhemispheric domain.

## 3.2 Coupling strength and choice encoding are dissociated. 

Peak coupling and peak choice encoding |r_choice| were uncorrelated across regions (Spearman ρ = −0.21, p = 0.17, Figure 2).<p align="center">
  <img src="figures/Figure_2.Choice encoding strength.png" width="800"/>
</p>

<p align="center">. 

Hippocampal regions had the highest coupling but the lowest decision fraction — the proportion of bilateral coupling devoted to choice encoding, defined as |r_choice| / r_peak — (CA1: 0.04; SUB: 0.06), meaning less than 6% of their bilateral synchrony related to the animal's upcoming choice. In contrast, prelimbic and infralimbic cortex had moderate coupling with the highest decision fraction (PL6a: 0.22; ILA6a: 0.20), indicating that over a fifth of their interhemispheric coordination specifically carried decision information.
The Swanson flatmap visualizes this dissociation (Figure 3): coupling is dominated by hippocampal and midbrain structures, while choice encoding concentrates in prefrontal and posterior thalamic regions, paralleling the selective communication subspace of Semedo et al. (2019, Neuron) extended to the bilateral context.
<p align="center">
  <img src="figures/Figure 3.Three-panel Swanson flatmap .png" width="1000"/>
</p>. 


## 3.3 Choice encoding strength varied widely across regions. 

The strongest encoders were SCdg (r_choice = 0.34, +75 ms), PL6a (0.26, −75 ms), and PO (0.26, +475 ms) (Figure 4). PL6a's pre-movement timing supports motor planning; PO and ILA6a peaked post-movement. 
<p align="center">
  <img src="figures/Figure_4. Choice encoding strength.png" width="800"/>
</p>

<p align="center">. 

Bilateral coupling was sustained across the trial in motor, hippocampal, and midbrain regions, with phase-dependent modulation in visual and thalamic regions (Figure 5).
<p align="center">
  <img src="figures/Figure_5. Bilateral coupling strength r(t) over time.png" width="800"/>
</p>

<p align="center">. 

## 3.4 Three temporal profiles identified in bilateral coupling cross-correlograms. 

Cross-correlograms (±500ms, 25ms resolution) revealed zero-lag dominance across the majority of regions, inconsistent with sequential hemispheric relay and consistent with common-input drive (Figure 6). Three exploratory profiles emerged: oscillatory zero-lag coupling with theta-period flanking in hippocampal and midbrain regions; broad sustained zero-lag coupling in prefrontal and collicular regions; and asymmetric positive-lag profiles in retrosplenial and posterior thalamic regions. Though not individually significant after permutation correction, their anatomical specificity points to mechanistically distinct bilateral coordination strategies across the brain's functional hierarchy.
<p align="center">
  <img src="figures/Figure_6. Cross-correlogram heatmap.png" width="800"/>
</p>

<p align="center">.

# 4. Discussions

The central finding of this study is not that bilateral coupling fails to predict choice encoding, but that the two properties are organized by distinct anatomical principles. Hippocampal regions present a particularly informative case: despite showing the strongest bilateral coupling (SUB: 0.96; CA1: 0.91), their decision fraction was the lowest observed (CA1: 0.04; SUB: 0.06), yet the independent decoder confirmed that each hemisphere individually encodes choice at above-chance levels (CA1: 0.65; SUB: 0.66), revealing that choice information exists within each hemisphere but is not routed through the bilateral channel. The  temporal profiles identified through bilateral coupling cross-correlograms must be treated as exploratory observations that can inform about distinct bilateral coordination strategies across the brain's functional hierarchy.
The organizational principle identified here, integration for motor output and segregation for sensory analysis, parallels hemispheric specialization observed across vertebrates and may reflect a conserved feature of bilateral brain organization. Clinically, the regional specificity of bilateral coupling offers a framework for predicting consequences of interhemispheric disconnection, with motor and hippocampal regions expected to be most vulnerable to callosal disruption. The CCA pipeline developed here provides a reusable tool for characterizing bilateral neural dynamics across large-scale datasets, with natural extension to machine-learning inference of contralateral dynamics from unilateral recordings. 

# 5. Limitations

Our study has several limitations. The IBL Brainwidemap dataset is dominated by unilateral recordings, which restricted the sample size of bilateral pairs. Cell-type information was unavailable, preventing us from distinguishing excitatory, inhibitory, and interneuronal contributions to bilateral coupling and inflating within-region signal variability. CCA itself imposes constraints: by extracting only the maximally correlated bilateral subspace, it captures a small fraction of total population activity, and additional decision-related communication may exist in lower-ranked canonical dimensions not analysed here. All results are correlational and cannot establish causality.

# 6. Future Directions

Given the wealth of unilateral data and the restrictions in obtaining bilateral recordings, machine-learning frameworks are being developed to infer contralateral dynamics from recorded hemispheres. Applied to IBL's large-scale left-hemisphere data, such models could probe bilateral dynamics at greater scale. Optogenetic unilateral silencing of top choice-encoding regions would test whether bilateral coupling is causally necessary for coordinated choice. Linking coupling strength to behavioral outcomes (accuracy, stimulus contrast) would clarify its functional relevance. Decoding-based extensions, replacing Pearson correlation with classifiers trained jointly on canonical variates, would more precisely resolve when decision information emerges in the bilateral subspace. Together, these directions would move the field from characterizing interhemispheric coordination to testing its computational necessity.

## Links
Github Page with Code and Notebooks [github: https://github.com/maleeshakarawita/Neuronova]
