---
title: 'Simulating Anomalous Diffusion using Matlab'
date: 2017-10-09
permalink: /posts/2017/10/anomalous-diffusion-simulation/
excerpt: 'Matlab provides a function for simulating anomalous diffusion from fractional Brownian motion theory. I show how I used this function to generate a simulated anomalous diffusion data set with random lengths, diffusion constants, and anomalous diffusion exponents.'
tags:
  - Matlab
  - Diffusion
  - Simulation
  - Data Visualization
---

### Introduction

See the full code [here](https://gist.github.com/mdaddysman/63c223c42eb73a086b0b0ef563bf7143)    

Welcome to my first blog post! I plan on using this blog to document issues encountered during the course of my research and code snippets used to solve them. In this post, I will describe code I used to create a data set that simulated anomalous diffusion using Matlab functions. The Matlab function is [*wfbm*](https://www.mathworks.com/help/wavelet/ref/wfbm.html) which uses Fractional Brownian Motion theory to simulate anomalous diffusion. However, I found that function had a few quirks that I had to work through to generate a data set.   

The goal of this code was to generate a large synthetic data set of diffusion. I'm currently working with large data sets made from tracking insulin granules inside $\beta$-cells. I've developed [an app](https://mdaddysman.shinyapps.io/trajectory_analysis/) to visualize these results. I plan on blogging how this app was developed later; however, to share this app online I needed a synthetic data set as the real data cannot be released right now. Hence, the code below.

### The Matlab *wfbm* function

Diffusion is modeled by the [mean squared displacement (MSD)](https://en.wikipedia.org/wiki/Mean_squared_displacement). For normal, or Brownian, diffusion the MSD increases linearly with lag time.

$$ MSD = 2dD\Delta $$

In this equation, *d* is the dimensionality of the system, *D* is the diffusion coefficient, and $\Delta$ is the lag time. For anomalous diffusion, the exponent of the lag time (typically represented by $\alpha$) is not equal to one. The generalized diffusion equation is used instead with $\alpha < 1$ referred to as subdiffusive and $\alpha > 1$ as superdiffusive. The transport coefficient, $\Gamma$, has replaced the diffusion coefficient to represent that the equivalence constant is now $\alpha$ dependent.

$$ MSD = 2d\Gamma\Delta^\alpha $$

In the Matlab documention the [*wfbm*](https://www.mathworks.com/help/wavelet/ref/wfbm.html) function is defined as:

```matlab
FBM = wfbm(H,L)
```

The two inputs define the nature of the diffusion, `H`, and the length of the trajectory to make `L`. The input `H` is the [*Hurst exponent*](https://en.wikipedia.org/wiki/Hurst_exponent) and is defined on the range of $0 < H < 1$ related to $\alpha$ by $\alpha = 2H$. However, there are some issues with using this function which are described in the next section.

### The code

I wanted this code to generate 1,000 trajectories that had random transport coefficients and diffusions exponents. The initial configuration sets the filename to save the output, the number of trajectories, and the maximum length of the trajectory.

```matlab
filename = 'Simulated1';
ntraj = 1000;
maxlength = 3000;
```
The next lines randomize the exponents, length of trajectory, and the transport coefficient. First the exponent which I choose to be uniformly distributed between $0.1 < H < 0.9$ ($0.2 < \alpha < 1.8$). However, *wfbm* had issues with long decimals, so I round to the thousands place.

```matlab
H = round( random('unif',0.1,0.9,[ntraj,1]) .* 1000 ) ./ 1000; %longer decimals appear to give problems
```
The lengths are integers distributed uniformly from 2 to the `maxlength`. The transport coefficients are distributed on a log-normal distribution which has the added benefit of preventing unphysical, negative transport coefficients.

```matlab
lengths = random('unid',maxlength-1,[ntraj,1]) + 1;
logD = random('norm',0,0.5,[ntraj,1]);
D = 2.^logD;
```
As a last setup step, I allocate the memory store the positions.  I also create a varible `s` that will store the current location in the large points matrix that the next set of trajectories should be stored.

```matlab
points = zeros(sum(lengths),4); %x,y,frame,traj number

s = 1;
```

The first command in the loop fixes a bug with the *wfmb* method. For some unknown reason values close to 0.5 *but not quite* 0.5 gave errors. In practice I found the cutoff to be about 0.025 away from 0.5. The following if statement changes these values to H = 0.5 to avoid the bug.

```matlab
if H(m) > 0.475 && H(m) < 0.525 %H near 0.5 but not quite give errors
    H(m) = 0.5;
end
```

The other issue was the trajectory length had to be longer than 100 steps. Although not the most efficient method, I got around this by simulating the maximum length trajectory every time for x and y and then truncating the sequence to the desired length.

```matlab
temp = D(m).*wfbm(H(m),maxlength); %points of less than 100 give an error. Just subsample.
points(s:s+lengths(m)-1,1) = temp(1:lengths(m));
temp = D(m).*wfbm(H(m),maxlength);
points(s:s+lengths(m)-1,2) = temp(1:lengths(m));
```
Finally, the frame number and and trajectory number are loaded into the matrix and the data is saved. The entire script is enclosed in `tic`-`toc` commands to time the script. It takes around 20-30 seconds to generate a data set on my 2014 iMac. The extra sampling is not an issue.

The code on [Github Gist](https://gist.github.com/mdaddysman/63c223c42eb73a086b0b0ef563bf7143)
