This is a work-in-progress [website](https://fanwangecon.github.io/CodeDynaAsset/) of collections of code for solving several infinite horizon exogenous incomplete dynamic asset models, produced by [Fan Wang](https://fanwangecon.github.io/). Materials gathered from various [papers](https://fanwangecon.github.io/research) on financial access with fixed costs, discrete and continuous asset choice grids and other features.

Generally, looped, vectorized, and optimized-vectorized implementations of the same solution algorithm with tabular, graphical and profiling results are shown. Looped codes are shown for clarity, vectorized codes are shown for speed. Codes are designed to not require special hardware or explicit parallelization. Codes tested on Windows 10 with [Matlab 2019a](https://www.mathworks.com/company/newsroom/mathworks-announces-release-2019a-of-matlab-and-simulink.html) for replicability. Please contact [FanWangEcon](https://fanwangecon.github.io/) for problems.

All functions are written with default parameters and are directly callable: 1, clone the project, see [here](docs/gitsetup.md); 2, add to path; 3, click run. There are three types of files:

1. **m**: matlab m file
2. **publish html**: html files generated by matlab publish, includes code mark-ups and tabular and graphical outputs from benchmark simulation
3. **profile**: html files generated by profiling the m file with timing results


# Introduction

Several models related to several [papers](https://fanwangecon.github.io/research) are implemented here. For each economic model, there are often a number of computational algorithms. Algorithms compete on their ability to best approximate the underlying economic model's solutions. For each computational algorithm, there are generally also different implementations that generate identical solutions but compete on speed.

The first main model below is the one dynamic asset and shocks model. The second main model is the two dynamic asset and shocks model. For each model, several algorithms are provided that offer different qualities of approximations. For each algorithm, various implementations are also shown with varying speed but identical solution approximations.

Dynamic asset models have three broad components: 1, solution; 2, distribution; 3, equilibrium. The sections below try to cover different algorithms and implementations of algorithms to solve for optimal conditional choices, joint asset distributions, and equilibrium prices. These algorithms here are developed in the process of trying to solve and estimate models in these [papers](https://fanwangecon.github.io/research), and might not be appropriate for alternative models.

# 1. The One Asset One Shock Problem (BZ)
<!-- https://fanwangecon.github.io/CodeDynaAsset/m_az -->

The *bz* problem: standard model with an asset and one shock, exogenous incomplete borrowing and savings, wage shocks follow AR1.

## 1.1 Main Optimization Solution Files (BZ)

Parameters can be adjusted [here](https://fanwangecon.github.io/CodeDynaAsset/m_az/paramfunc/html/ffs_az_set_default_param.html), for the benchmark simulation:

- savings only
- **750** grid points for asset states/choices
- **15** grid points for the AR1 shock

Using three algorithm that provide identical solutions:

1. *bz* model [looped solution](https://fanwangecon.github.io/CodeDynaAsset/m_az/solve/html/ff_az_vf.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_az/solve/ff_az_vf.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_az/solve/html/ff_az_vf.html) \| [**profile**](https://fanwangecon.github.io/CodeDynaAsset/m_az/solve/profile/ff_az_vf_default_p3/file0.html)
    * speed: **9765.7** seconds
    * loops: 1 for VFI, 1 for shocks, 1 for asset state, 1 for asset choice, 1 for future shocks
2. *bz* model [vectorized solution](https://fanwangecon.github.io/CodeDynaAsset/m_az/solve/html/ff_az_vf_vec.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_az/solve/ff_az_vf_vec.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_az/solve/html/ff_az_vf_vec.html) \| [**profile**](https://fanwangecon.github.io/CodeDynaAsset/m_az/solve/profile/ff_az_vf_vec_default_p3/file0.html)    
    * speed: **34.3** seconds
    * loops: 1 for VFI, 1 for shocks, vectorize remaining 3 loops
3. *bz* model [optimized vectorized solution](https://fanwangecon.github.io/CodeDynaAsset/m_az/solve/html/ff_az_vf_vecsv.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_az/solve/ff_az_vf_vecsv.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_az/solve/html/ff_az_vf_vecsv.html) \| [**profile**](https://fanwangecon.github.io/CodeDynaAsset/m_az/solve/profile/ff_az_vf_vecsv_default_p3/file0.html)    
    * speed: **1.2** seconds
    * loops: 1 for VFI, 1 for shocks, vectorize remaining 3 loops, reuse u(c)
    * reuse u(c) in cells, speed improvements described [here](https://fanwangecon.github.io/M4Econ/)

## 1.2 Asset Distributions (BZ)

Solving for the asset distribution.

## 1.3 Solution Support Files (BZ)

**Parameters and Function Definitions**:
1. *bz* model [set default parameters](https://fanwangecon.github.io/CodeDynaAsset/m_az/paramfunc/html/ffs_az_set_default_param.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_az/paramfunc/ffs_az_set_default_param.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_az/paramfunc/html/ffs_az_set_default_param.html)
    * param_map: container map for carrying parameters across functions
    * support_map: container map for carrying programming instructions etc across functions
2. *bz* model [set functions](https://fanwangecon.github.io/CodeDynaAsset/m_az/paramfunc/html/ffs_az_set_functions.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_az/paramfunc/ffs_az_set_functions.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_az/paramfunc/html/ffs_az_set_functions.html)
    * functions: centrally define functions as function handles
3. *bz* model [generate states, choices, and shocks grids](https://fanwangecon.github.io/CodeDynaAsset/m_az/paramfunc/html/ffs_az_get_funcgrid.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_az/paramfunc/ffs_az_get_funcgrid.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_az/paramfunc/html/ffs_az_get_funcgrid.html)
    * func_map: container map containing all function handles
    * armt_map: container map containing matrixes for states and choices.

**Output Analysis**:
1.  *bz* model [solution results processing](https://fanwangecon.github.io/CodeDynaAsset/m_az/solvepost/html/ff_az_vf_post.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_az/solvepost/ff_az_vf_post.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_az/solvepost/html/ff_az_vf_post.html)
    * table: value and policy function by states and shocks
    * table: iteration convergence and percentage policy function change by shock
    * graph: value + policy functions with levels, logged levels and percentages
    * mat: store all workspace matrixes, arrays, scalar values to matrixes
2. *bz* model [solution results graphing](https://fanwangecon.github.io/CodeDynaAsset/m_az/solvepost/html/ff_az_vf_post_graph.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_az/solvepost/ff_az_vf_post_graph.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_az/solvepost/html/ff_az_vf_post_graph.html)
    * graph: value function by asset and shock
    * graph: consumption and asset choice levels
    * graph: consumption and asset logged levels
    * graph: consumption and asset as percentages of coh and assets


## 1.4 Cash-on-hand and One Shock (COH-Z)

The problem could be posed slightly differently with the asset state variable as cash-on-hand. The codes are basically identical, and are shown here in this [folder](https://fanwangecon.github.io/CodeDynaAsset/m_oz). Speeds and outcomes are the same. This is useful for thinking about finding asset distributions. Additionally, part 2 model state space is all in terms of cash-on-hand.

**Main Optimization Solution Files**:
- *coh-z* model [looped solution](https://fanwangecon.github.io/CodeDynaAsset/m_oz/solve/html/ff_oz_vf.html), [vectorized solution](https://fanwangecon.github.io/CodeDynaAsset/m_oz/solve/html/ff_oz_vf_vec.html), [optimized vectorized solution](https://fanwangecon.github.io/CodeDynaAsset/m_oz/solve/html/ff_oz_vf_vecsv.html)

**Parameters and Function Definitions**:
- *coh-z* model [set default parameters](https://fanwangecon.github.io/CodeDynaAsset/m_oz/paramfunc/html/ffs_oz_set_default_param.html), [set functions](https://fanwangecon.github.io/CodeDynaAsset/m_oz/paramfunc/html/ffs_oz_set_functions.html), [generate states, choices, and shocks grids](https://fanwangecon.github.io/CodeDynaAsset/m_oz/paramfunc/html/ffs_oz_get_funcgrid.html)

**Output Analysis**:
- *coh-z* model [solution results processing](https://fanwangecon.github.io/CodeDynaAsset/m_oz/solvepost/html/ff_oz_vf_post.html), [solution results graphing](https://fanwangecon.github.io/CodeDynaAsset/m_oz/solvepost/html/ff_oz_vf_post_graph.html)


# 2. The Risky + Safe Asset Problem (Part 1)

Two endogenous assets, one safe one risky. Risky asset could be stocks with constant return to scale, or physical capital investment with depreciation and decreasing return to scale. Note that the utility function is CRRA, however, households do not have constant share of risky investment for any wealth (cash-on-hand) levels when risky asset has decreasing return to scale and when shock is persistent.

There are more analytical ways of solving the basic version of this problem. Here we stick to using this grid based solution algorithm which allows for flexibly solving non-differentiable and non-continuous problems. The grid based solution algorithm now with 2 endogenous choices and states requires exponentially more computation time than the *bz* model. Here I provide three sets of solution algorithms at increasing speeds:

- In **2.1**, solve the problem with the two asset choice concurrently
- In **2.2**, solve the problem in two stages
- In **2.3**, two stage solution with interpolation

## 2.1 Concurrent Solution (BKZ)

The *bkz* problem. Parameters can be adjusted [here](https://fanwangecon.github.io/CodeDynaAsset/m_akz/paramfunc/html/ffs_akz_set_default_param.html), for the benchmark simulation:

- savings problem with alternative safe and risky assets
- **50** aggregate savings grid points, **1274** combinations of safe and risky asset choices. Choice grids shown [here](https://fanwangecon.github.io/CodeDynaAsset/m_akz/paramfunc/html/ffs_akz_get_funcgrid.html) at the end of the file. Note that the benchmark parameters for the *bz* model has *750* grid points of choices/states, the benchmark problem here is larger and hence takes more time.
- **15** grid points for the AR1 shock

1. *bkz* model [looped solution](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/html/ff_akz_vf.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_akz/solve/ff_akz_vf.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/html/ff_akz_vf.html) \| [**profile**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/profile/ff_akz_vf_default_p3/file0.html)    
    * speed: **32764.7** seconds
    * loops: 1 for VFI, 1 for shocks, 1 for coh(b,k,z), 1 for (b',k') choices, 1 for future shocks
2. *bkz* model [vectorized solution](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/html/ff_akz_vf_vec.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_akz/solve/ff_akz_vf_vec.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/html/ff_akz_vf_vec.html) \| [**profile**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/profile/ff_akz_vf_vec_default_p3/file0.html)    
    * speed: **92.6** seconds
    * loops: 1 for VFI, 1 for shocks, vectorize remaining
3. *bkz* model [optimized vectorized solution](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/html/ff_akz_vf_vecsv.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_akz/solve/ff_akz_vf_vecsv.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/html/ff_akz_vf_vecsv.html) \| [**profile**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/profile/ff_akz_vf_vecsv_default_p3/file0.html)    
    * speed: **2.5** seconds
    * loops: 1 for VFI, 1 for shocks, vectorize remaining
    * reuse u(c) in cells, speed improvements described [here](https://fanwangecon.github.io/M4Econ/)

## 2.2 Two Stage Solution (WKZ)

The *wkz* problem, w=k'+b'. Takes significantly less time than *2.1*, produces identical results. Parameters can be adjusted [here](https://fanwangecon.github.io/CodeDynaAsset/m_akz/paramfunc/html/ffs_akz_set_default_param.html), for the benchmark simulation, same as *2.1*:

- savings problem with alternative safe and risky assets
- **45** aggregate savings grid points, **1035** combinations of safe and risky asset choices.
- **15** grid points for the AR1 shock

1. *wkz* model [2nd stage solution](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/html/ff_wkz_evf.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_akz/solve/ff_wkz_evf.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/html/ff_wkz_evf.html)
    * solving for k(w,z) = argmax_{k'}(E(V(coh(k',b'=w-k'),z')) given z and w.
2. *wkz* model [looped solution](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/html/ff_wkz_vf.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_akz/solve/ff_wkz_vf.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/html/ff_wkz_vf.html) \| [**profile**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/profile/ff_wkz_vf_default_p3/file0.html)    
    * speed: **606.3** seconds (*54* times faster than *2.1*)
    * Step One solve k(w,z); Step Two solve w(z,coh(b,k,z)) given k(w,z)
    * loops: 1 for VFI, 1 for shocks, 1 for coh(b,k,z), 1 for w(z)=k'+b'
3. *wkz* model [vectorized solution](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/html/ff_wkz_vf_vec.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_akz/solve/ff_wkz_vf_vec.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/html/ff_wkz_vf_vec.html) \| [**profile**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/profile/ff_wkz_vf_vec_default_p3/file0.html)    
    * speed: **3.1** seconds (*30* times faster than *2.1*)
    * Step One solve k(w,z); Step Two solve w(z,coh(b,k,z)) given k(w,z)
    * loops: 1 for VFI, 1 for shocks, vectorize remaining
4. *wkz* model [optimized vectorized solution](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/html/ff_wkz_vf_vecsv.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_akz/solve/ff_wkz_vf_vecsv.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/html/ff_wkz_vf_vecsv.html) \| [**profile**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/profile/ff_wkz_vf_vecsv_default_p3/file0.html)    
    * speed: **0.7** seconds (*4* times faster than *2.1*)
    * Step One solve k(w,z); Step Two solve w(z,coh(b,k,z)) given k(w,z)
    * loops: 1 for VFI, 1 for shocks, vectorize remaining
    * store u(c) in cells, update when k*(w,z) changes, speed improvements described [here](https://fanwangecon.github.io/M4Econ/)

## 2.3 Two Stage with Interpolation (iWKZ)

The *iwkz* problem, interpolated version of 2.2. Takes significantly less time than *2.2* at larger choice grids, produces approximately identical results as *2.2*. Simulations below are at the same grid points as *2.2* and *2.1* for comparison, but at these low accuracy grid points, *iwkz* is not necessarily faster than *2.2*. Parameters can be adjusted [here](https://fanwangecon.github.io/CodeDynaAsset/m_akz/paramfunc/html/ffs_akz_set_default_param.html), for the benchmark simulation, same as *2.1* nad *2.2*, but we introduce two additional measures of precision:

- savings problem with alternative safe and risky assets
- **50** aggregate savings grid points, **1274** combinations of safe and risky asset choices.
- **15** grid points for the AR1 shock
- **0.0001** consumption interpolation grid gap
- **0.1** value function cash-on-hand interpolation grid gap

*Note*: the reason that *iKWZ* is faster then *2.2* is that when we increase aggregate savings grid points, we do not need to reduce the two interpolation grid gaps. COH is the endogenous state, and its size is determined by the *cash-on-hand interpolation grid gap*. The choice grid increases in size as we increase aggregate savings grid points, but since we are solving in two stages, the dimensionality of the problem increases proportionally (but not exponentially). The total number of safe and risky asset choice is determined by assuming equi-distance grid gap for aggregate savings as well as both risky and safe investments. Speed up achieved via interpolation as described [here](https://fanwangecon.github.io/M4Econ/). In section 2.6 below, I show how solution graphs change as we increase aggregate savings grid points, shock grid points, and reduce the interpolation gaps.

1. Use again *wkz* [2nd stage solution](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/html/ff_wkz_evf.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_akz/solve/ff_wkz_evf.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/html/ff_wkz_evf.html)
    * solving for k(w,z) = argmax_{k'}(E(V(coh(k',b'=w-k'),z')) given z and w.
2. *iwkz* model [looped solution](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/html/ff_iwkz_vf.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_akz/solve/ff_iwkz_vf.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/html/ff_iwkz_vf.html) \| [**profile**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/profile/ff_iwkz_vf_default_p3/file0.html)    
    * speed: **2683.0** seconds (*4.4* times slower than *2.2*)
    * Step One solve k(w,z); Step Two solve w(z,coh(b,k,z)) given k(w,z)
    * loops: 1 for VFI, 1 for shocks, 1 for coh(b,k,z), 1 for w(z)=k'+b'    
    * interpolate u(c), interpolate v(coh,z)
3. *iwkz* model [vectorized solution](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/html/ff_iwkz_vf_vec.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_akz/solve/ff_iwkz_vf_vec.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/html/ff_iwkz_vf_vec.html) \| [**profile**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/profile/ff_iwkz_vf_vec_default_p3/file0.html)    
    * speed: **2.2** seconds (*1.4* times faster than *2.2*)
    * Step One solve k(w,z); Step Two solve w(z,coh(b,k,z)) given k(w,z)
    * loops: 1 for VFI, 1 for shocks, vectorize remaining
    * interpolate u(c), interpolate v(coh,z)
4. *iwkz* model [optimized vectorized solution](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/html/ff_iwkz_vf_vecsv.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_akz/solve/ff_iwkz_vf_vecsv.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/html/ff_iwkz_vf_vecsv.html) \| [**profile**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solve/profile/ff_iwkz_vf_vecsv_default_p3/file0.html)    
    * speed: **0.6** seconds (much faster at denser grid)
    * Step One solve k(w,z); Step Two solve w(z,coh(b,k,z)) given k(w,z)
    * loops: 1 for VFI, 1 for shocks, vectorize remaining
    * interpolate u(c), interpolate v(coh,z)
    * store u(c) in cells, update when k*(w,z) changes

## 2.4 Asset Distributions (BKZ + WKZ + iWKZ)

Solving for the asset distribution.

## 2.5 Solution Support Files (Shared)

All solution algorithms share the same support files.

**Parameters and Function Definitions**:
1. *bkz+wkz+iwkz* model [set default parameters](https://fanwangecon.github.io/CodeDynaAsset/m_akz/paramfunc/html/ffs_akz_set_default_param.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_akz/paramfunc/ffs_akz_set_default_param.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/paramfunc/html/ffs_akz_set_default_param.html)
    * param_map: container map for carrying parameters across functions
    * support_map: container map for carrying programming instructions etc across functions
2. *bkz+wkz+iwkz* model [set functions](https://fanwangecon.github.io/CodeDynaAsset/m_akz/paramfunc/html/ffs_akz_set_functions.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_akz/paramfunc/ffs_akz_set_functions.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/paramfunc/html/ffs_akz_set_functions.html)
    * functions: centrally define functions as function handles
3. *bkz+wkz+iwkz* model [generate states, choices, and shocks grids](https://fanwangecon.github.io/CodeDynaAsset/m_akz/paramfunc/html/ffs_akz_get_funcgrid.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_akz/paramfunc/ffs_akz_get_funcgrid.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/paramfunc/html/ffs_akz_get_funcgrid.html)
    * func_map: container map containing function handles
    * armt_map: container map containing matrixes for states and choices.

**Output Analysis**:
1. *bkz+wkz+iwkz* model [solution results processing](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solvepost/html/ff_akz_vf_post.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_akz/solvepost/ff_akz_vf_post.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solvepost/html/ff_akz_vf_post.html)
    * table: value and policy function by states and shocks
    * table: convergence and percentage policy function change by shock
    * graph: value + policy functions with levels, logged levels and percentages
    * mat: store all workspace matrixes, arrays, scalar values to matrixes
2. *bkz+wkz+iwkz* model [solution results graphing](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solvepost/html/ff_akz_vf_post_graph.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_akz/solvepost/ff_akz_vf_post_graph.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/solvepost/html/ff_akz_vf_post_graph.html)
    * graph: value function by asset and shock
    * graph: consumption and asset choice levels
    * graph: consumption and asset logged levels
    * graph: consumption and asset as percentages of coh and assets


## 2.6 The Risky + Safe Asset Problem Testing (iWKZ)

We simulate the joint asset choice problem using the *optimized-vectorized* method for *iwkz* algorithm from *2.3*. Now we analyze model features by adjusting parameters.

1. *iwkz* model solution precision
    * [adjust choice grid points](https://fanwangecon.github.io/CodeDynaAsset/m_akz/test/ff_iwkz_vf_vecsv/test_precision/html/fsi_ikwz_vf_vecsv_w_n.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_akz/test/ff_iwkz_vf_vecsv/test_precision/fsi_ikwz_vf_vecsv_w_n.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/test/ff_iwkz_vf_vecsv/test_precision/html/fsi_ikwz_vf_vecsv_w_n.html)
    * [adjust shock grid points](https://fanwangecon.github.io/CodeDynaAsset/m_akz/test/ff_iwkz_vf_vecsv/test_precision/html/fsi_ikwz_vf_vecsv_z_n.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_akz/test/ff_iwkz_vf_vecsv/test_precision/fsi_ikwz_vf_vecsv_z_n.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/test/ff_iwkz_vf_vecsv/test_precision/html/fsi_ikwz_vf_vecsv_z_n.html)
    * [adjust cash-on-hand interpolation grid gap](https://fanwangecon.github.io/CodeDynaAsset/m_akz/test/ff_iwkz_vf_vecsv/test_precision/html/fsi_ikwz_vf_vecsv_coh_interp_grid_gap.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_akz/test/ff_iwkz_vf_vecsv/test_precision/fsi_ikwz_vf_vecsv_coh_interp_grid_gap.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/test/ff_iwkz_vf_vecsv/test_precision/html/fsi_ikwz_vf_vecsv_coh_interp_grid_gap.html)
    * [benchmark vs high-precision](https://fanwangecon.github.io/CodeDynaAsset/m_akz/test/ff_iwkz_vf_vecsv/test_precision/html/fsi_ikwz_vf_vecsv_main.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_akz/test/ff_iwkz_vf_vecsv/test_precision/fsi_ikwz_vf_vecsv_main.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/test/ff_iwkz_vf_vecsv/test_precision/html/fsi_ikwz_vf_vecsv_main.html)
2. *iwkz* model risky investment return
    * [stock vs bond](https://fanwangecon.github.io/CodeDynaAsset/m_akz/test/ff_iwkz_vf_vecsv/test_prod/html/fsi_stock.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_akz/test/ff_iwkz_vf_vecsv/test_prod/fsi_stock.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_akz/test/ff_iwkz_vf_vecsv/test_prod/html/fsi_stock.html)


# 3. The Risky + Safe Asset Problem (Part 2)

In Section 2, we solved the two asset problem in levels. Here, the same model is solved, but using an alternative algorithm. Now the model is solved in percentages. Households choose the percentage of cash-on-hand to allocate to aggregate savings, and then given that the percentage of total savings to allocate to safe vs risky asset.

In the levels solution above, households at different state space points face the same choice set, but almost half of the choice points are not invalid because they would lead to negative consumption today. Solving the problem in percentages allow all households to have the same choice grid in terms of percentage levels. This potentially offers much more precise solutions under some model parameters combinations. For example, for the constant-return-to-scale risky investment, we know analytically optimal choices should be percentages based.

Additionally, the levels based grid solution might lead to degenerate steady state asset distributions. This is because at low levels of cash-on-hand, if the risky asset's lowest level of choice is invalid, then an individual is stuck in that state. A subset of state space points become absorbing states. For households starting at higher levels of cash-on-hand, there is non-zero probability that they move towards these absorbing states due to bad shocks. With enough iteration, the distribution becomes potentially degenerate. The percentage grid problem does not have this problem, at all levels of cash-on-hand there is non-zero risky asset investment available. This means that households that reach lower levels of cash-on-hand can rise back to higher wealth levels through investments.


## 3.1 Two Stage Percentage Interpolation (ipWKZ)

This is the *ipWKZ* problem: *i* stands for interpolation, *p* stands for percentage, *wk* stand for two stage w=k'+b' first then k' given w. Parameters can be adjusted [here](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/paramfunc/html/ffs_ipwkz_set_default_param.html), for the benchmark simulation, same as *2.3*, but we introduce several additional measures of precision:

- savings problem with alternative safe and risky assets
- **50** aggregate savings percentage points, **50** risky asset percentage points
- **15** grid points for the AR1 shock
- interpolate 1: **0.0001** consumption interpolation grid gap
- interpolate 2: **0.1** value function cash-on-hand interpolation grid gap
- interpolate 3: **0.1** expected value function aggregate savings interpolation grid gap

1. Use again *ipwkz* [2nd stage solution](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/solve/html/ff_ipwkz_evf.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_ipwkz/solve/ff_ipwkz_evf.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/solve/html/ff_ipwkz_evf.html)
    * solving for kperc(w,z) = argmax_{kperc'}(E(V(coh(kperc',b'=w-w*kperc'),z')) given z and w.
2. *ipwkz* model [looped solution](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/solve/html/ff_ipwkz_vf.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_ipwkz/solve/ff_ipwkz_vf.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/solve/html/ff_ipwkz_vf.html) \| [**profile**](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/solve/profile/ff_ipwkz_vf_default_p3/file0.html)    
    * speed: **2683.0** seconds (*4.4* times slower than *2.2*)
    * Step One solve kperc(w,z); Step Two solve wperc(z,coh(b,k,z)) given kperc(w,z)
    * loops: 1 for VFI, 1 for shocks, 1 for coh(b,k,z), 1 for wperc(z)=kperc'+b'    
    * interpolate u(c), interpolate v(coh,z), interpolate EV(w,z)
3. *ipwkz* model [vectorized solution](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/solve/html/ff_ipwkz_vf_vec.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_ipwkz/solve/ff_ipwkz_vf_vec.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/solve/html/ff_ipwkz_vf_vec.html) \| [**profile**](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/solve/profile/ff_ipwkz_vf_vec_default_p3/file0.html)    
    * speed: **2.2** seconds (*1.4* times faster than *2.2*)
    * Step One solve kperc(w,z); Step Two solve wperc(z,coh(b,k,z)) given kperc(w,z)
    * loops: 1 for VFI, 1 for shocks, vectorize remaining
    * interpolate u(c), interpolate v(coh,z), interpolate EV(w,z)
4. *ipwkz* model [optimized vectorized solution](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/solve/html/ff_ipwkz_vf_vecsv.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_ipwkz/solve/ff_ipwkz_vf_vecsv.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/solve/html/ff_ipwkz_vf_vecsv.html) \| [**profile**](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/solve/profile/ff_ipwkz_vf_vecsv_default_p3/file0.html)    
    * speed: **1.0** seconds (much faster at denser grid)
    * Step One solve kperc(w,z); Step Two solve wperc(z,coh(b,k,z)) given kperc(w,z)
    * loops: 1 for VFI, 1 for shocks, vectorize remaining
    * interpolate u(c), interpolate v(coh,z), interpolate EV(w,z)
    * store u(c) in cells, update when k*(w,z) changes

## 3.2 Asset Distributions (ipWKZ)

Solving for the asset distribution.

## 3.3 Solution Support Files (ipWKZ)

All solution algorithms share the same support files.

**Parameters and Function Definitions**:
1. *ipwkz* model [set default parameters](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/paramfunc/html/ffs_ipwkz_set_default_param.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_ipwkz/paramfunc/ffs_ipwkz_set_default_param.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/paramfunc/html/ffs_ipwkz_set_default_param.html)
    * param_map: container map for carrying parameters across functions
    * support_map: container map for carrying programming instructions etc across functions
2. *ipwkz* model [set functions](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/paramfunc/html/ffs_ipwkz_set_functions.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_ipwkz/paramfunc/ffs_ipwkz_set_functions.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/paramfunc/html/ffs_ipwkz_set_functions.html)
    * functions: centrally define functions as function handles
3. *ipwkz* model [generate states, choices, and shocks grids](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/paramfunc/html/ffs_ipwkz_get_funcgrid.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_ipwkz/paramfunc/ffs_ipwkz_get_funcgrid.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/paramfunc/html/ffs_ipwkz_get_funcgrid.html)
    * func_map: container map containing function handles
    * armt_map: container map containing matrixes for states and choices.

**Output Analysis**:
1. *ipwkz* model [solution results processing](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/solvepost/html/ff_ipwkz_vf_post.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_ipwkz/solvepost/ff_ipwkz_vf_post.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/solvepost/html/ff_ipwkz_vf_post.html)
    * table: value and policy function by states and shocks
    * table: convergence and percentage policy function change by shock
    * graph: value + policy functions with levels, logged levels and percentages
    * mat: store all workspace matrixes, arrays, scalar values to matrixes
2. *ipwkz* model [solution results graphing](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/solvepost/html/ff_ipwkz_vf_post_graph.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_ipwkz/solvepost/ff_ipwkz_vf_post_graph.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/solvepost/html/ff_ipwkz_vf_post_graph.html)
    * graph: value function by asset and shock
    * graph: consumption and asset choice levels
    * graph: consumption and asset logged levels
    * graph: consumption and asset as percentages of coh and assets


## 3.4 The Risky + Safe Asset Problem Testing (ipWKZ)

We simulate the joint asset choice problem using the *optimized-vectorized* method for *ipwkz* algorithm from *2.3*. Now we analyze model features by adjusting parameters.

1. *ipwkz* 2nd stage optimal k given w percentage vs level
    * [risky physical capital (drs) investments](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/test/ff_ipwkz_evf/test_prod/html/fsi_ipwkz_evf_physicalk.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_ipwkz/test/ff_ipwkz_evf/test_prod/html/fsi_ipwkz_evf_physicalk.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/test/ff_ipwkz_evf/test_prod/html/fsi_ipwkz_evf_physicalk.html)
    * [risky financial investment (crs)](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/test/ff_ipwkz_evf/test_prod/html/fsi_ipwkz_evf_stock.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_ipwkz/test/ff_ipwkz_evf/test_prod/html/fsi_ipwkz_evf_stock.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/test/ff_ipwkz_evf/test_prod/html/fsi_ipwkz_evf_stock.html)
2. *ipwkz* model solution precision
    * [adjust choice grid points](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/test/ff_ipwkz_vf_vecsv/test_precision/html/fsi_ipwkz_vf_vecsv_w_n.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_ipwkz/test/ff_ipwkz_vf_vecsv/test_precision/fsi_ipwkz_vf_vecsv_w_n.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/test/ff_ipwkz_vf_vecsv/test_precision/html/fsi_ipwkz_vf_vecsv_w_n.html)
    * [adjust shock grid points](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/test/ff_ipwkz_vf_vecsv/test_precision/html/fsi_ipwkz_vf_vecsv_z_n.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_ipwkz/test/ff_ipwkz_vf_vecsv/test_precision/fsi_ipwkz_vf_vecsv_z_n.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/test/ff_ipwkz_vf_vecsv/test_precision/html/fsi_ipwkz_vf_vecsv_z_n.html)
    * [adjust cash-on-hand interpolation grid gap](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/test/ff_ipwkz_vf_vecsv/test_precision/html/fsi_ipwkz_vf_vecsv_coh_interp_grid_gap.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_ipwkz/test/ff_ipwkz_vf_vecsv/test_precision/fsi_ipwkz_vf_vecsv_coh_interp_grid_gap.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/test/ff_ipwkz_vf_vecsv/test_precision/html/fsi_ipwkz_vf_vecsv_coh_interp_grid_gap.html)
    * [benchmark vs high-precision](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/test/ff_ipwkz_vf_vecsv/test_precision/html/fsi_ipwkz_vf_vecsv_main.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_ipwkz/test/ff_ipwkz_vf_vecsv/test_precision/fsi_ipwkz_vf_vecsv_main.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/test/ff_ipwkz_vf_vecsv/test_precision/html/fsi_ipwkz_vf_vecsv_main.html)
3. *ipwkz* model risky investment return
    * [stock vs bond](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/test/ff_ipwkz_vf_vecsv/test_prod/html/fsi_ipwkz_vf_vecsv_stock.html): [**m**](https://github.com/FanWangEcon/CodeDynaAsset/blob/master/m_ipwkz/test/ff_ipwkz_vf_vecsv/test_prod/fsi_ipwkz_vf_vecsv_stock.m) \| [**publish html**](https://fanwangecon.github.io/CodeDynaAsset/m_ipwkz/test/ff_ipwkz_vf_vecsv/test_prod/html/fsi_ipwkz_vf_vecsv_stock.html)
