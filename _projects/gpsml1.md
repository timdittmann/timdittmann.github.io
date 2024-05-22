---
layout: page
title: GPS and ML to detect Earthquakes in Real Time
description: Part I. Real-time classification
img: assets/img/project1/figure_3.png
importance: 1
category: research-to-operations
related_publications: true
---
> This is a two part post.  This first post presents some scientific motivation, our ML classification strategy, and implementation into operations.  Then check out [part II](https://timdittmann.github.io/projects/) for data augmentation of this domain for deep learning.

In this project:
1. [Motivation](#end-to-end-ml)
2. [ML Strategy](#model-training-highlights)
3. [Real-time Inference Implementation](#real-time-inference-experiment)

# End-to-end ML.  

Continously operating remote GPS (or more broadly [GNSS](https://www.earthscope.org/what-is/gps/gnss-vs-gps/)) ground stations exist globably, built and operated to support positioning, navigation and timing services such as surveying, reference frames and geophysics.  In geophysics, historically these were built for measuring plate motion (...think large slow processes...).  However, if sampled and estimated at higher frequencies, these GPS will track the ground motion of an earthquake, valuable for analysis of earthquake mechanics.  If this data is streamed in real-time, this is valuable information for hazard monitoring and early warning, especially when existing networks are [sparse](http://www.grapenthin.org/download/Grapenthin_etal_2017_iniskin_eew.pdf) or earthquakes are [really large](https://youtu.be/tLjGJFWLSvY?si=82EO4YemWWwEjgQH&t=385) (and consequently generally the most destructive).

However, using GPS for these signals has challenges: these can be relatively weak signals amidst complex noise.  Existing strategies require other instruments to resolve false alerts.  Instead we wish to reduce external dependencies and increased decision confidence at the individual node level to propogate to more rapid event-awareness.

We used machine learning (:evergreen_tree:`random forest`:evergreen_tree:) to classify seismic events in GNSS data that outperform the current methods.
We then share our ongoing effort taking this offline research into realtime inference for low latency detection.




<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/project1/figure_3.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

---
> **_Wait!_** Everything these days is End-to-end: _What end? To what (to which?) end?_

---


1. **One End:** Raw GPS measurements from fixed ground stations.
2. **The other end:** Real time inference for actionable earthquake insight.


# Model training highlights: 
covered in {% cite Dittmann2022b %}:
* :artificial_satellite: **Geodesy:** 5Hz GNSS [TDCP processing](https://github.com/crowellbw/SNIVEL) using broadcast ephemeris and narrow lane.
* :chart_with_upwards_trend: **Seismology:** ~2k waveforms from ~80 earthquakes >M4.9 labeled by visual inspection.
* :computer: **ML:** Random forest classifier with 5 fold nested cross validation optimized for F1 score.
* :memo: **Open Science:** [open **manuscript**.](https://doi.org/10.1029/2022JB024854) + [**code** repo.](https://github.com/timdittmann/gnss_vel_classifier) + [open **datasets**.](https://zenodo.org/records/7909327)




<div class="row justify-content-sm-center">
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/project1/figure_2.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/project1/figure_7.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    (L) Map of seismic focal mechanisms used in this work and distribution of 77 event magnitudes. (R) Performance of Random forest model developed in the work here across the entire event catalog.
</div>


<img src="/assets/img/project1/tsRF.gif" alt="drawing" width="75%"/>
<div class="caption">
    Velocity and detection time series from a station experiencing a weaker event and adjacent "noise" event. Middle panels b/c show alerts using the current detection methods. Panel d is an <b>example of the RF classifier of this work. </b>
</div>


# Real time inference experiment 
## Components:
* **stream processing platform:** Apache Kafka in AWS MSK.
    * this incredibly powerful pub/sub platform had a steep learning curve for me, with a lot left to learn.  I appreciate [Redpanda+docker compose](https://docs.redpanda.com/current/get-started/quick-start/) for deploying local clusters for development. 
* **autoscaling, low latency model endpoint:** AWS Sagemaker (Python SDK)
    * Like much of AWS, there are many steps along the way w.r.t. how commited you are to their services versus open source or external services.  I found the sagemaker SDK to be currently a balance for fast iteration of deploying training jobs and model endpoints of familiar open source ML libraries (scikitlearn + tensorflow)
* **stateful stream processing (windowing + invoke endpoint):** [Bytewax](https://bytewax.io/)
    * I experimented with Faust and Bytewax for pythonic Kafka stream processing, and found Bytewax to be the most intuitive for windowing feature extraction and inference.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="https://upload.wikimedia.org/wikipedia/commons/thumb/5/53/Apache_kafka_wordtype.svg/2560px-Apache_kafka_wordtype.svg.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="https://images.ctfassets.net/paqvtpyf8rwu/htf2EtvdmoTDv2knt2VYq/1077d9a11cf7a07fdf8cfd7abe0b1764/migrating-kafka-to-redpanda-events.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="https://d1.awsstatic.com/product-marketing/IronMan/AWS-service-icon_sagemaker.5ccec16f16a04ed56cb1d7f02dcdada8de261923.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="https://rtp.vc/wp-content/uploads/2023/11/logo_white-background-transparent.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<img src="https://github.com/bytewax/bytewax/assets/156834296/4e314f17-38ab-4e72-9268-a48ddee7a201" alt="bytewax model"  width="75%" />

*Image credit: Bytewax*

I like this visualization of stream processing.  Our stack :books: is 

> [Kafka consumer] :arrow_right::arrow_right: [Bytewax agg/enrichment in AWS ECS] :arrow_right::arrow_right: [Kafka producer] 

## Operational Performance:
[This is under development!]

