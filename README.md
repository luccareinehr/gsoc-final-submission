# Google Summer of Code 2022

Final submission for Google Summer of Code '22 in OpenFASoC at [@chipsalliance](github.com/chipsalliance/) 🤖

## Introduction
For this last Summer and the month of October, I've been working on an open silicon project in the Google Summer of Code program as part of CHIPS Alliance, an umbrella organization hosted by The Linux Foundation. The project is **OpenFASoC**, conducted by researchers at the University of Michigan. With this text, I would like to share what I've contributed to, and how the experience has been in this journey.

## Summary
OpenFASoC, an acronym for "Fully Open-Source Autonomous SoC Synthesis using Customizable Cell-Based Synthesizable Analog Circuits", is a somewhat recent piece of software that  aims to, given user specifications, automatically generate physical circuit blocks that can be later used in SoCs. The project scope is mainly focused on analog and mixed-signal designs. Currently, only six designs are at work, with more to come in the future.

I've worked mainly with one of the designs, a small subthreshold temperature sensor - or, in fact, the module that builds it: the temperature sensor generator (`temp-sense-gen`). My initial proposal was to create a Design Space Exploration (DSE) infrastructure to optimize this generator and automatically choose the best configuration for its internal generator parameters, such as the circuit's physical design generation variables, to keep them as default regardless of user specs. However, due to the complexity of the domain - I'm an EE student at CentraleSupélec, but still undergraduate - me and my mentor, prof. Mehdi Saligane, chose to change the goal to rather improve the current temperature sensor design generation, and at the end propose an opening for a future DSE project.

![image](https://user-images.githubusercontent.com/75084099/197396228-a0ff04d5-ecb2-44ef-9662-4b195abcb369.png)

_This is how the mixed-signal temperature sensor block looks like. It's cool, isn't it?_

## Working environment

The team working on OpenFASoC consists of researchers from UMich, professionals of the industry committed to open source software and a few students from different parts of the world, with my mentor being the lead. Communication was done by Slack and meetings were organized twice a week, with meeting hours being a minor inconvenience in such an international team. In fact, this characteristic of the project was one of the most enriching to me, as I could obtain an experience of working with people from different backgrounds, that communicate in different ways, and in a remote setting which is becoming more and more common.

This project was set as of long duration (~300 hours), with an agreed extended deadline of October 24th.

## Tools

Regarding development, in this project I mainly used:

- OpenROAD 
- OpenROAD Flow Scripts

To some extent, these tools were also used:

- KLayout
- Netgen
- Magic

In terms of programming languages, it mainly required:

- Python: for reading and writing scripts
- Tcl: for writing scripts to control the physical design generation flow

Other languages, used less frequently, were:

- C++: to read and debug OpenROAD code
- Verilog: to understand how the circuit description files are modified by the generator

There was also exposure to some types of files common in electronics physical design software, mainly:

- SPICE files: to debug simulation and LVS files
- DEF/LEF files: for describing and debugging the physical layout

## My contributions

We can devide the work done in three timeframes:

### First phase (June - July)

Being a fairly new repository (it was created in March 2021), OpenFASoC still doesn't have some of the features that mature projects do, which posed added difficulties during GSoC. Here are some of them:

- Lack of documentation
- Dependencies are very unstable and update often
- Project structure can change frequently
- The tool is not yet packaged
- There's no release schedule
- Installation is done all manually

These were major challenges at the start of the program, and while some have been addressed during GSoC (such as added installation options and documentation), they remain as difficulties to new contributors that will definitely get sorted as the project grows.

Because of this, the first weeks were dedicated to familiarizing with the tools that the project used and tackling small issues. Main points were:

- Ease the installation of dependencies by using Conda packages
- Experiments with the working generators
- Study of the integrated circuit design flow

### Second phase (August - Sept)

The month of August used to tackle a major dependency problem for the temperature sensor generator: the update of its flow scripts to the latest version of the OpenROAD tool. OpenROAD is an open source software that allows for the generation of physical circuit layouts from Verilog descriptions, so it is crucial to most generators in OpenFASoC. When updating it, I tried to change the OpenFASoC flow slightly to keep it as close to OpenROAD Flow Scripts as possible, to make it easy to update in the future since ours was a fork of it. This change would make it possible to later add punctual improvements to the generator in the third phase.

As a consequence, main points were:

- Study of the OpenROAD RTL-to-GDS tool
- Update the temperature sensor generator to use the latest version of OpenROAD ([pull request #82](https://github.com/idea-fasoc/OpenFASOC/pull/82))
- Organize generated artifacts
- Restructure docs and document the temperature sensor generator flow ([pull request #100](https://github.com/idea-fasoc/OpenFASOC/pull/100))

### Third phase (Sept - Oct)

During the last phase, I worked on improving the automatic generation of a placement and routing of cells that connect to multiple voltage sources.

Basically, OpenFASoC's temperature sensor design has two voltage domains: VDD/GND and VIN/GND, so most circuit components (which we call instances) has one or the other as a supply. Some components (the so-called header cells), however, are responsible for generating VIN from VDD, so they must connect to both voltage domains. The singularity of this case caused the generated route to be less than ideal - it was too thin and didn't have adequate connection points, which could lead to a voltage drop in the physical implementation that deteriorates the sensor output - or to not get generated at all!

Therefore, main points were:

- Study of DEF/LEF file structure
- Procedure for custom placement of header cells
- Update power delivery network layout ([pull request #111](https://github.com/idea-fasoc/OpenFASOC/pull/111))
- Change header cells pins to higher metal layers and add nondefault rule to their route to VIN
- Implement option for making multiple connections in the VIN route to the voltage domain ring ([pull request #113](https://github.com/idea-fasoc/OpenFASOC/pull/113))

### Bonus: opening for DSE

Since the proposal that got accepted to participate in GSoC was about Design Space Exploration, I figured it would be nice to create something for OpenFASoC that could futurely purpose as motivation for a DSE feature.

The original idea was to leverage the Google Cloud Platform (GCP) computing power to leave a DSE job running in the cloud, since physical circuit simulations can take hours. Also, during this GSoC, the concept of creating example Jupyter notebooks to illustrate open silicon flows rose in the community - in OpenFASoC, for example, a notebook showcasing the temperature sensor generator was created by one of the team members.

So, during the last week of the program, I chose to unite both concepts and write a Jupyter notebook to showcase DSE with the temperature sensor generator that could also be added as an example in the OpenFASoC documentation! The main point was, then:

- Experiments with Hyperparameter Tuning in GCP
- Write a Jupyter notebook, executable from Google Colab, running a DSE job in the cloud to optimize the temperature sensor generator power usage ([pull request #116](https://github.com/idea-fasoc/OpenFASOC/pull/116))

During all phases, I reached out for help from other members of the team (and of other open silicon projects), which helped a lot the development.

## State of the project and future work

For the temperature sensor generator, the idea is to further improve the design, for example by minimizing chip area. The proposal of a special router to the OpenROAD team to address situations like encountered with the VIN route was also discussed. For OpenFASoC, on the other hand, work remains to be done in other generators, or new circuit designs can be proposed.

Eventually, the tool could also be packaged and converted to a command line tool: instead of `cd`ing into the repo and running `make`, it could be made to run in the terminal just by calling `openfasoc generate`, for example.

## Challenges and learnings

### Challenges:
- Growing projects are hard to get into
- Dependency management is difficult
- There's a lot of very specific cases to learn in microelectronics
- Commercial physical design tools are expensive

### Learnings:
- Communication is key
- Write issues to not forget them and begin discussions
- Don't reach for help without doing your own research
- Reflect to be clear when explaining an issue, otherwise it will just confuse who you're talking with  
- Know how to adapt your communication style with people from different backgrounds than mine
- Improved organization and work in remote
- Tcl, Physical Design, some Docker and GCP, ... !

## Conclusion

Being a part of Google Summer of Code '22 was a major experience in my student life, and I can only thank everyone involved! It allowed me to get a bit more into a field I'm interested in, microelectronics, and to gain experience programming in a large project, but also to work with people from all over the world (the US, India, Morocco, Ethiopia, Rwanda, Japan, ...). In July, I also attended a conference on open silicon, the FSiC2022, which out of luck happened in Paris (the city I'm living in) and allowed me to meet my mentor in person!

To finish, here are some numbers:

- Pull requests merged: 7
- Issues opened: 12
- Issues closed: 9
- Commits: 27
- Lines of code added: +612655*
- Lines of code removed: -358035* 
- Optimized power consumption of the temperature sensor: 436 uW

_*This enormous number of lines of code is mainly due to the temperature sensor OpenROAD fork update! :)_


<!--
i reached out for help from other members of the team (and of other repos) and it helped a lot

conclusion:

allowed me to work with people from all over the world (the US, India, Morocco, Ethiopia, Rwanda, Japan

I feel I became more than a contributor, but <<a reference>> (mudar isso, botar algo tipo um apoio sla) to some of the other students in the team

FSiC2022 conference in Paris

some data (number of PRs, lines of code altered, issues opened, commits etc)
 -->
