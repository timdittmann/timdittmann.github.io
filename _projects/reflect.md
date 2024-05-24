---
layout: page
title:  Reflected space signals for surface hydrology
description: Remote sensing signals of opportunity
img: assets/jupyter/images/cccp_invsnr.png
importance: 3
category: research-to-operations
related_publications: false
---

[`gnssrefl` is an "an open source software package for GNSS interferometric reflectometry (GNSS-IR)."](https://github.com/kristinemlarson/gnssrefl)

This open source project, lead by Dr. Kristine Larson (who is the core developer, chief scientist and primary advocate), is an example of:

* **bridging the gap from science to operations:**  [Open-source software development](https://github.com/kristinemlarson/gnssrefl/issues), [packaging the research software](https://pypi.org/project/gnssrefl/), [developing notebooks](https://github.com/kristinemlarson/gnssrefl/tree/master/notebooks), [web api's](https://gnss-reflections.org/rzones) and [containerizing the research application](https://github.com/kristinemlarson/gnssrefl/pkgs/container/gnssrefl) were core activities of this project that support [**open science**](https://www.whitehouse.gov/ostp/news-updates/2024/01/31/fact-sheet-biden-harris-administration-marks-the-anniversary-of-ostps-year-of-open-science/) at scale for new discoveries.

* **opportunistic science:**   Reflected energy, aka multipath, is a nuisance for users interested in the direct GPS signal, which is basically every user.  However, the interference patterns in the received signal power can tell you about the hydrologic parameters around the ground station. This includes [snow depth](https://tc.copernicus.org/articles/14/1985/2020/), [soil moisture](https://ieeexplore.ieee.org/document/6479284), and [sea level](https://ihr.iho.int/articles/water-level-measurements-using-reflected-gnss-signals/). *One person's trash is another person's treasure*.  The treasure, in this case, are invaluable measurements of [essential climate variables](https://gcos.wmo.int/en/essential-climate-variables/).

> From a **remote sensing** perspective, this source of these critical environmental paramters not only complements existing, dedicated instruments and satellite missions, but offers some advantages:
>> **soil moisture** and **snow depth** are sourced either from single instruments biased by hyperlocalized effects or costly satellite missions that typically have lower temporal resolution and depending on the instrument, coarser spatial resolution.

>> In the case of **sea level**, an observation from an instrument that is not submerged in corrosive water AND simultaneously measures its location in a global reference frame will last longer and provide an absolute reference for relative measurements.



<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="https://earthscope.piwigo.com/uploads/l/x/m/lxmzkzl6k6//2022/10/26/20221026003403-92c06ad7.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="https://gnss-reflections.org/static/images/overview.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="https://earthscope.piwigo.com/uploads/l/x/m/lxmzkzl6k6//2022/11/07/20221107193927-b19a0dba.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    (L) Example of a station capable of measuring snowdepth (credit: EarthScope). (Center) Illustration from gnss-reflections.org (credit: K. Larson). (R) Example of a station capable of measuring sea-level (Credit: EarthScope).
</div>

## [Check out the **project page** to learn ALOT more.](https://gnssrefl.readthedocs.io/en/latest/)


----

### Example Notebook
Here is a notebook for a scenario in which a user provides a GNSS observation file (RINEX) for a ground station that they think *might* be a candidate for measuring sea level with reflected signals. This is forked from Kelly Enloe's [project notebooks](https://github.com/kristinemlarson/gnssrefl/tree/master/notebooks).


{::nomarkdown}
{% assign jupyter_path = "assets/jupyter/unknown_rinex_cccp.ipynb" | relative_url %}
{% capture notebook_exists %}{% file_exists assets/jupyter/unknown_rinex_cccp.ipynb %}{% endcapture %}
{% if notebook_exists == "true" %}
{% jupyter_notebook jupyter_path %}
{% else %}

<p>Sorry, the notebook you are looking for does not exist.</p>
{% endif %}
{:/nomarkdown}