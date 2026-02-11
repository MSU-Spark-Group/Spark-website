---
title: Team
nav:
  order: 2
  tooltip: About our team
---

# {% include icon.html icon="fa-solid fa-users" %}Team


{% include section.html %}

{% include list.html data="members" component="portrait" filter="role == 'principal-investigator'" %}
{% include list.html data="members" component="portrait" filter="role == 'postdoc'" %}
{% include list.html data="members" component="portrait" filter="role == 'phd'" %}
{% include list.html data="members" component="portrait" filter="role == 'grad'" %}

{% include section.html background="images/DSC_0314.jpg" dark=true %}

# Collaborator

{% include section.html %}

{% capture content %}

{% include figure.html
   image="images/members/Artboard 1.png"
   caption="Princeton University Combustion and Low Carbon Energy Conversion Lab"
   link="https://mae.princeton.edu/people/yiguang-ju" %}


{% include figure.html
   image="images/members/P2.svg"
   caption="Princeton Collaborative Low Temperature Plasma Research Facility"
   link="https://pcrf.princeton.edu/" %}

{% include figure.html
   image="images/members/MSUlogo.png"
   caption="Michigan State University Irrigation Lab "
   link="https://www.msuirrigationlab.com/home" %}

{% endcapture %}

{% include grid.html style="square" content=content %}
