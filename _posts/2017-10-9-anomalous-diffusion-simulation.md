---
title: 'Simulating Anomalous Diffusion'
date: 2017-10-09
permalink: /posts/2017/10/anomalous-diffusion-simulation/
tags:
  - Matlab
  - Diffusion
  - Simulation
  - Data Visualization 
---

## Simulating Anomalous Diffusion using Matlab

See the full code [here](https://gist.github.com/mdaddysman/63c223c42eb73a086b0b0ef563bf7143)   
   
### Introduction

Welcome to my first blog post! I plan on using this blog to document issues encountered during the course of my research and code snippets used to solve them. In this post, I will describe code I used to create a data set that simulated anomalous diffusion using Matlab functions. The Matlab function is [*wfbm*](https://www.mathworks.com/help/wavelet/ref/wfbm.html) which uses Fractional Brownian Motion theory to simulate anomlous diffusion. However, I found that function had a few querks that I had to work through to generate a data set.   

The goal of this code was to generate a large syntheic data set of diffusion. I'm currently working with large data sets made from tracking insulin granules inside $\beta$-cells. I've developed [an app](https://mdaddysman.shinyapps.io/trajectory_analysis/) to visulize these results. I plan on bloging how this app was developed later; however, to share this app online I needed a sythenic data set as the real data cannot be released right now. Hince, the code below. 

### The Matlab *wfbm* function

Diffusion is modeled by the [mean squared displacement (MSD)](https://en.wikipedia.org/wiki/Mean_squared_displacement). For normal, or Brownian, diffusion the MSD increases linearly with lag time. 

$$ MSD = 2dD\Delta $$

In this equation, *d* is the dimensionality of the system, *D* is the diffusion coefficient, and $\Delta$ is the lag time. For anomalous diffusion, the exponent of the lag time (typically represented by $\alpha$) is not equal to one. The generalized diffusion equation is used instead with $\alpha < 1$ referred to as subdiffusive and $\alpha > 1$ as superdiffusive. The transport coefficient, $\Gamma$, has replaced the diffusion coefficient to represent that the equlivence constant is now $\alpha$ dependant.

$$ MSD = 2d\Gamma\Delta^\alpha $$

In the Matlab documention the [*wfbm*](https://www.mathworks.com/help/wavelet/ref/wfbm.html) function is defined as: 

```matlab
FBM = wfbm(H,L)
```

The two inputs define the nature of the diffusion, `H`, and the length of the trajectory to make `L`. The input `H` is the [*Hurst exponent*](https://en.wikipedia.org/wiki/Hurst_exponent) and is defined on the range of $0 < H < 1$ related to $\alpha$ by $\alpha = 2H$. However, there are some issues with using this function which are described in the next section. 

### The code

I wanted this code to generate 1,000 trajectories that had random transport coefficients and diffusions exponents. 

```matlab
filename = 'Simulated1'; 
ntraj = 1000;
maxlength = 3000;
```
  