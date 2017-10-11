---
permalink: /software/
title: "Software Projects"
author_profile: true
---
{% include base_path %}

This is a summary of my current academic and "for fun" open-source programming projects. Documentation for the projects can be found in the README file in the Github repository. The projects are posted on my [Github Profile](https://github.com/mdaddysman) or you can follow the links in the tables below.

### Academic

| Project (Github link) | Language  | Type |                                   |
| --------| --------- | ---- | ----------------------------------|
| [Single Particle Tracking](https://github.com/mdaddysman/Insulin-Tracking/tree/master/MSDPlotting) | R  | Interactive Web App | [View the App](https://mdaddysman.shinyapps.io/trajectory_analysis/) The GUI allows for the visualization of large data single particle tracking data sets. The data can be filtered on-the-fly and specific MSDs or parameters can be selected to visual the trajectory. |
| [Cell Counter](https://github.com/mdaddysman/Cell-Counter) |  Matlab | Script   | The script quantifies the fluorescence intensity in HeLa cells after immunocytochemistry staining of thymine dimers with Alexa 488. However, it can be modified for other counting tasks. Used in [this paper]({{ "/publication/2011-11-02-Biophys-J" | absolute_url }}). |
| [Point FRAP](https://github.com/mdaddysman/point-FRAP)  | Matlab | Script  | Matlab routines custom written to process and fit two-photon diffraction limited "point" FRAP data. Used in [this paper]({{ "/publication/2013-01-11-J-Phys-Chem-B" | absolute_url }}). |
| [Excelsior One GUI](https://github.com/mdaddysman/Excelsior-One-GUI) | Matlab | GUI | A GUI written in Matlab for controlling four Spectra-Physics Excelsior One OEM lasers for use on a spinning disk confocal microscope laser launch. The GUI controls on/off, power output, monitors temperature and options such as TTL pulsing. |
| [Thorlabs sCMOS USB Camera Driver Wrapper](https://github.com/mdaddysman/Thorlabs-CMOS-USB-cameras-in-Matlab) | C++, Matlab | SDK | This repository contains several wrapper functions written in Matlab compatible C++. When compiled in Matlab, the methods generated interface with the C++ API for Thorlabs CMOS USB cameras allowing their use in Matlab. |
| [Syringe Pump GUI](https://github.com/mdaddysman/Syringe-Pump-GUI) | Matlab | GUI | These GUIs control either one or two Harvard Apparatus pump 11 elite syringe pumps. The two pump control maintains [Laminar flow](https://en.wikipedia.org/wiki/Laminar_flow) in a properly configured microfluidic device to allow for rapid switching of buffer flow. |


### Personal "for fun" projects

| Project (Github link) | Language  | Type |                                   |
| --------| --------- | ---- | ----------------------------------|
| [Box Game or *Kästchenspiel*](https://github.com/mdaddysman/Box-Game) | C  | Simple Game | The game is programmed using C and [SDL 2.0](https://www.libsdl.org/) to learn SDL and platform independent graphical programming. The game features straightforward gameplay with a rapidly increasing challenge. |
