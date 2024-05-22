---
layout: page
title:  GPS and ML to detect Earthquakes in Real Time
description: Part II. Data Augmentation for Deep Learning
img: assets/img/project2/denoise_flow.drawio.png
importance: 2
category: research-to-operations
related_publications: true
---
> This is a two part post.  This second post addresses challenges we faced in [part I](https://timdittmann.github.io/projects/) due to operating in a data-limited regime.  

For this project:
1. [Motivation](#motivation)
2. [Augmentation](#data-augmentation-generating-psuedo-synthetic-timeseries)
3. [Deep-Learning](#more-data-for-deeper-learning)

----

## Motivation
Two facts that make data-driven GPS seismology challenging:
1. Large earthquakes are [relatively infrequent](https://www.iris.edu/hq/inclass/fact-sheet/how_often_do_earthquakes_occur) (fortunately!)
2. GPS, the oldest modern positioning satellite constellation, was only "fully" operational (24 satellites) [in 1993](https://www.nasa.gov/general/global-positioning-system-history/).

Given that archiving higher sample rates required to capture earthquake ground motions began ~decade later, and we have a relatively limited dataset of observed events with GNSS.

One strategy for addressing this limited data is [synthesizing waveforms](https://agupubs.onlinelibrary.wiley.com/doi/full/10.1002/2016JB013314).  We take inspiration from [Lin et al.](https://doi.org/10.1029/2021JB022703) using DL models to not only rapidly characterize seismic events but also shed insight into the evolution of large earthquakes.

Another strategy is to start with actual clean samples of occurrences and then augment them with the complexity of real-world noise and data variability.  For this we found particular inspiration from [Hoffman, et al. (2019)](https://www.science.org/doi/10.1126/sciadv.aau6792) strategy for crumpling of paper.  ([Crumpling](https://www.nytimes.com/2018/11/26/science/crumple-paper-math.html) is a fun topology deep-dive!)
>An alternative strategy is to consider a reference system free from data limitations alongside the target system, with the idea that similarities between the target and reference systems allow a machine learning model of one to inform that of the other.
 

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="https://www.science.org/cms/10.1126/sciadv.aau6792/asset/5be06f5f-d604-471c-8ae5-41c47a02f56c/assets/graphic/aau6792-f1.jpeg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    What does crumpled paper have to do with earthquakes?
</div>

----

## Data augmentation generating psuedo synthetic timeseries

We generated a pseudo synthetic training catalog, much larger than the existing observed catalog (see image below) as follows:
* **real signals:** we query a [database](https://peer.berkeley.edu/research/nga-west-2) of "noise-free" strong motion waveforms
* **synthetic noise:**  we characterize the frequency distribution of real noise, from which we can sample to generate realistic synthetic noise.

This database is not *free from data limitations* in the paper crumbling approach,  but does offer a much larger (~60x) set with clean labels from which to train (figure below).

---
> **real earthquake** signals + **synthetic GPS** noise = **psuedosynthetic GPS earthquake** timeseries 

---

* [Open-access manuscript. {% cite Dittmann_Morton_Crowell_Melgar_DeGrande_Mencin_2023%}](https://seismica.library.mcgill.ca/article/view/978)
* [Open Data Catalog.](https://zenodo.org/records/7909327)
* [Code repo.](https://github.com/timdittmann/psuedosynth_gnss_velocities)

<div class="row justify-content-sm-center">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/project2/synth_ngaw_gnss_distrib.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/project2/ambientnoise_H.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    (L) (a) is a histogram comparing the GNSS catalog with the strong motion database (“NGAW2”). The scatter plot in (b) are the individual event magnitudes as a function of time, and the secondary axis line plot is the cumulative station count over time observing the events. 
    (R) GNSS 5Hz velocity probabilistic power spectral densities for horizontal components of motion.  This characterization of stochastic noise in the frequency domain is used to generate representative synthetic noise.
</div>

----

## More data for deeper learning
Questions:
1. **Are these psuedosynthetics representative of real-world data?**.  
    * We trained one model only with these synthetics, and another with the [observed data](https://agupubs.onlinelibrary.wiley.com/doi/full/10.1029/2022JB024854).  On a held-aside unseen testing dataset, the synthetic-data-driven model outpeformed the real-data-driven case. {% cite Dittmann_Morton_Crowell_Melgar_DeGrande_Mencin_2023%}
2. **What questions can we ask given increased data volume and veracity?**
    * We experiment with deeper learning applications made possible by the volume and labels of this data catalog.

### Experiment: Denoising timeseries with deep learning
* **Objective:**
    In order to increase the signal to noise ratio (SNR) of relatively weak signals embedded in complex noise, we train a U-net convolutional network architecture used for image processing on **_denoising_** time windows of highrate GNSS velocity timeseries. Unlike traditional fixed bandwidth frequency filtering, the neural network learns a sparse representation mask of time-frequency domain features to separate complex, time-variant noise signatures from a range of signal inputs.
* **Data:**  We train the model using the previously mentioned catalog of synthetic 5Hz TDCP horizontal waveforms.  After splitting and windowing, the training set consists of ~75k samples.
* **Features:**
Following the strategy of [Zhu et al. (2019)](https://arxiv.org/abs/1811.02695) and [Thomas et al. (2023)](https://seismica.library.mcgill.ca/article/view/240), these incoming waveforms, Y (t) are represented as the superposition of signal S(t) and noise N(t), and their short-time fourier transforms (STFT), can be represented as:

\begin{equation} Y(t,f) = S(t,f) + N(t,f) \end{equation}

Our model input features are the STFT $$Y(t,f)$$ from 90 second, non-overlapping windows extracted from the horizontal components of motion.

The model target is a signal mask, $$M_s(t,f)$$:

\begin{equation} M_s(t,f)  = \frac{1}{1+\frac{\lvert N(t,f) \rvert }{\lvert S(t,f) \rvert}} \end{equation}

where $$N(t,f)$$ and $$S(t,f)$$ are the noise and signal STFT, respectively.

* **ML Architecture:**
The convolutional neural network (CNN) employed is a [U-Net](https://arxiv.org/abs/1505.04597) architecture, originally developed for biomedical image-segmentation.  We train the UNET using the Adam optimizer with a learning rate of 0.001, determined through hyperparamter optimization tuning. The model training is optimized using the mean square estimator (MSE) loss function. We train the model using the open source deep learning software [TensorFlow](https://github.com/tensorflow/tensorflow), with a batch size of 128 and 50 epochs, determined experimentally.

* **Manuscript:** {% cite Dittmann2024%}

* **Results:**

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/project2/denoise_flow.drawio.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Flow chart for denoising GNSS velocities, where a) is a ’raw’ single horizontal component 90 second window of 5HZ TDCP velocities. b) is the real and imaginary components of the Short Time Fourier Transform of this incoming (signal+noise) timeseries (eq. 1) that are the input to the UNET model, c) . This model outputs a frequency mask, d), (eq. 2) which is then applied to the original inputs, b) to generate a denoised STFT, e). Lastly, the inverse transform outputs a denoised time series, f).
</div>


<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/project2/denoise_ex4873.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/project2/snr_marg_peaksig.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
    (L) A denoised analysis of a horizontal strong motion waveform. The left panels (a) are the psuedosynthetic waveforms of stochastic GNSS noise and the true strong motion signal, shown in the middle panels (b). The right panels are the denoised panel (a). The top panels are time series, the bottom panels are the STFT for the window. 
    (R) Distribution of increase in SNR from denoising as a function of peak signal amplitude. The scatter is shaded by density of samples, and the marginal distributions for signal amplitude and SNR increase are visible on the top and right panels, respectively.
</div>

## What's next?

### Method
* evaluate denoising of existing [observed waveform catalog](https://zenodo.org/records/7909327).
* experiment with alternative feature/target and DL architecture strategies
* investigate integration with [classification](https://timdittmann.github.io/projects/1_project/#model-training-highlights)

### Product
* Integrate this DL into [real-time stream processing](https://timdittmann.github.io/projects/1_project/#real-time-inference-experiment) of TDCP velocity streams for boosting SNR of GNSS seismology.
