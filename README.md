# Google Summer of Code '22
Final submission for Google Summer of Code '22 at [@chipsalliance](github.com/chipsalliance/) 🤖

## Introduction
For this last Summer and the month of October, I've been working on an open silicon project in the Google Summer of Code program as part of CHIPS Alliance, an umbrella organization hosted by The Linux Foundation. The project is OpenFASoC, conducted by researchers at the University of Michigan. With this text, I would like to share what I've contributed to, and how the experience has been in this journey.

## Summary
OpenFASoC, an acronym for "Fully Open-Source Autonomous SoC Synthesis using Customizable Cell-Based Synthesizable Analog Circuits", is a somewhat recent piece of software that  aims to, given user specifications, automatically generate physical circuit blocks that can be later used in integrated circuits. The project scope is mainly focused on analog and mixed-signal designs. Currently, only six designs are at work, with more to come in the future.

I've worked mainly with one of the designs, a small subthreshold temperature sensor - or, in fact, the module that builds it: the temperature sensor generator (`temp-sense-gen`). My initial proposal was to create a Design Space Exploration (DSE) infrastructure to optimize this generator and automatically choose the best configuration for its internal generator parameters, such as the circuit's physical design generation variables, to keep them as default regardless of user specs. However, due to the complexity of the domain - I'm an EE student at CentraleSupélec, but still undergraduate - me and my mentor, prof. Mehdi Saligane, chose to change the goal to rather improve the current temperature sensor design generation, and at the end propose an opening for a future DSE project.

<!-- how the team is structured

Being a new repo,
lack of documentation
structure that can change frequently
dependencies are very unstable
the tool is not yet packaged
there's no release schedule
installation is all manual


conclusion:

allowed me to work with people from all over the world (the US, India, Morocco, Ethiopia, Rwanda, Japan
 -->
