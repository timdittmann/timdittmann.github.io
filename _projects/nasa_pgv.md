---
layout: page
title: GNSS Velocity Data Center 
description: 
img: assets/img/seismica_crowell_italy.png
#redirect: https://unsplash.com
importance: 5
category: scientific-cloud-computing
---
# Prototype Operational Applications to Seismic and Space Weather Networks 

We were recently funded by the [NASA ROSES](https://science.nasa.gov/researchers/sara/grant-solicitations/) program to develop a prototype of a GNSS Velocity Data Center.
This work is in collaboration with the University of Washington (PI Brendan Crowell, researchers Jensen DeGrande and Jessica Ghent).

This page is a work in progress, I am (personally) really motivated by the arc of  research-to-operations in this highimpact scientific and cyberinfrastructure collaboration.
The project depends on collaborating expertise across geodesy, seismology, space weather, cloud infrastructure, event streaming, scientific data archiving and ML integration.  

[Here's a presentation on youtube](https://youtu.be/X7Ulzctn77Y?si=nTXYK5KxL6c857om&t=2970) in which Brendan describes the project.



## Code repos:
* [SNIVEL Python Repo](https://github.com/crowellbw/SNIVEL)
* EarthScope [golang real-time/batch TDCP processing](https://gitlab.com/earthscope/gnsstools)

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/nasa_pgv/crowell_24.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    ShakeMaps for the Mw 6.6 Norcia earthquake on December 30, 2016. (a) A ShakeMap run with the default parame- ters and using only the GNSS velocities for instrumental data. The squares show the locations of the GNSS stations, colored by their Modified Mercalli Intensity (MMI). The circles are the Did You Feel It? (DYFI) reports. (b) The published USGS ShakeMap which uses only seismic data for instrumental data. The triangles show the locations of the seismic stations, colored by their MMI. (c) A ShakeMap run combining the GNSS and seismic instrumental data. Squares and triangles indicate the locations of GNSS and seismic stations respectively. (d) The MMI residual between panels (c) and (b). All models use the same DYFI, earthquake source, and GMM information. (Figure and caption from Crowell, B. et al. 2023)
</div>

## Some background references on TDCP/Variometric processing:
_(not meant to be comprehensive! let me know what i should add)_
1. TDCP Methodology and Analysis:
    * van Graas, F. and Soloviev, A. Precise Velocity Estimation Using a Stand-Alone GPS Receiver. In Proceedings of the ION NTM 2003, page 352–360. Institute of Navigation, 2003.
    * Colosimo, G., M. C. and Mazzoni, A. Real-time GPS seismology with a stand-alone receiver: A preliminary feasibility demonstration. Journal of Geophysical Research, 116(B11302), 2011.
    * Grapenthin, R., West, M., Tape, C., Gardiner, M., , and Freymueller, J. Single-Frequency Instantaneous GNSS Velocities Resolve Dynamic Ground Motion of the 2016 Mw 7.1 Iniskin, Alaska, Earthquake. Seismol. Res. Lett, 89(1040-1048), 2018.
    * Shu, Y., Fang, R., Li, M., Shi, C., and nan Liu, J. Very high-rate GPS for measuring dynamic seismic displacements without aliasing: perfor- mance evaluation of the variometric approach. GPS Solutions, 22:1–13, 2018.
    * Dittmann, T., Hodgkinson, K., Morton, J., Mencin, D., and Mattioli, G. S. Comparing Sensitivities of Geodetic Processing Methods for Rapid Earthquake Magnitude Estimation. Seismological Research Letters, 93(3):1497–1509, 2022a.
    * Crowell, B., DeGrande, J., Dittmann, T., and Ghent, J. Validation of Peak Ground Velocities Recorded on Very-high rate GNSS Against NGA- West2 Ground Motion Models. Seismica, 2(1), Mar. 2023.

2. TDCP Applications:
    * Benedetti, E., Branzanti, M., Biagi, L., Colosimo, G., Mazzoni, A., and Crespi, M. Global Navigation Satellite Systems Seismology for the 2012 Mw 6.1 Emilia Earthquake: Exploiting the VADASE Algorithm. Seismological Research Letters, 85(3):649–656, 2014.
    * Savastano, G., Komjathy, A., Verkhoglyadova, O. et al. Real-Time Detection of Tsunami Ionospheric Disturbances with a Stand-Alone GNSS Receiver: A Preliminary Feasibility Demonstration. Sci Rep 7, 46607 (2017).
    * Fratarcangeli, F., Ravanelli, M., Mazzoni, A., Colosimo, G., Benedetti, E., Branzanti, M., Savastano, G., Verkhoglyadova, O., Komjathy, A., and Crespi, M. The variometric approach to real-time high-frequency geodesy. Rendiconti Lincei. Scienze Fisiche e Naturali, 29, 2018.
    * Hohensinn, R. and Geiger, A. Stand-alone GNSS sensors as velocity seismometers: Real-time monitoring and earthquake detection. Sensors, 18(11)(3712), 2018.
    * Fang, R., Zheng, J., Geng, J., Shu, Y., Shi, C., and Liu, J. Earthquake Magnitude Scaling Using Peak Ground Velocity Derived from High-Rate GNSS Observations. Seismol. Res. Lett, XX(1-11), 2020.
    * Crowell, B. W. Near-Field Strong Ground Motions from GPS-Derived Velocities for 2020 Intermountain Western United States Earthquakes. Seismol. Res. Lett, XX(1-9), 2021.
    * Ravanelli, M., Occhipinti, G., Savastano, G. et al. GNSS total variometric approach: first demonstration of a tool for real-time tsunami genesis estimation. Sci Rep 11, 3114 (2021).
    * Dittmann, T., Liu, Y., Morton, Y., and Mencin, D. Supervised Machine Learning of High Rate GNSS Velocities for Earthquake Strong Motion Signals. Journal of Geophysical Research: Solid Earth, 127(11), 2022b. 
    * Parameswaran, R. M., Grapenthin, R., West, M. E., and Fozkos, A. Interchangeable Use of GNSS and Seismic Data for Rapid Earthquake Characterization: 2021 Chignik, Alaska, Earthquake. Seismological Research Letters, 03 2023.
