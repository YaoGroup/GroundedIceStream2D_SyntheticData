# GroundedIceStream2D_SyntheticData

## Overview
The data in this repository was generated with the finite-element package Ice-Sheet and Sea-Level System Model (ISSM) in Matlab. The model equations were the shelf-stream (for grounded ice) and shallow-shelf (for floating ice) approximations (SSA).

There are many ways in which basal friction is parametrized. The approach used by ISSM, as used by this dataset, can be written simply as follows:

$$ \tau_{bx} = Cu $$

$$ \tau_{by} = Cv $$ 

where $C$ (also referred as $\alpha^2$ in literature to emphasize that it is a positive quantity) is a possibly complicated function of ice flow dynamics. The only parametrization used in this dataset is the "simple" Weertman parametrization:

$$C\equiv\alpha^2 = C_w (u^2 + v^2)^{\frac{1 - m}{2m}}$$

Let the deviatoric stress tensor be $\tau_{ij}$, the thickness be $h$, the surface elevation be $s$, the density of ice be $\rho$, then the shelfy-stream approximation equations are given by 

$$ \partial_x[h(2\tau_{xx}+\tau_{yy})] + \partial_y(h\tau_{xy}) -\rho g h \partial_x s - \tau_{bx} = 0$$

$$ \partial_x(h\tau_{xy}) + \partial_y[h(\tau_{xx}+2\tau_{yy})] -\rho g h \partial_y s - \tau_{by} = 0$$

where $\tau_{ij}$ is related to strain rate by Glen's Law in ISSM:

$$\tau_{ij} = 2\mu\dot\epsilon_{ij} = B\dot\epsilon_{eff}^{\frac{1}{n}-1} \dot\epsilon_{ij} $$

$$\dot\epsilon_{eff}^2 = \dot\epsilon_{xx}^2 + \dot\epsilon_{yy}^2 + +\dot\epsilon_{xy}^2 + \dot\epsilon_{xx}\dot\epsilon_{yy} $$

In each of the `data-#` folders:
- `md.mat` is the ISSM model file that contains all information regarding the setup, in particular bed and channel geometries, rheology and basal friction.
- `syndata_full.mat` is a proceessed version of the `md.mat` file that extracts all variables that one could in principle have access to when doing inversion using real data.
- `syndata_grounded.mat` is the same as `syndata_full.mat` except any floating ice is omitted. If you are interested in working on basal inversion, this is the dataset you will find most helpful.

Then, in either `syndata_full.mat` or `syndata_grounded.mat`, the following variables are stored:
- `xd` and `yd`: The x- and y- coordinates of FEM vertices at which velocities are calculated
- `ud` and `vd`: The x- and y- components (a.k.a. $u$ and $v$) of velocity corresponding to the entries in `xd` and `yd`
- `xct` and `yct`: The x- and y- coordinates of FEM vertices on the boundary of the domain (the grounded dataset stores these values evaluated at the boundary of the grounded region, and not the entire model domain).
- `bd_ud`, `bd_vd`, and `bd_mu`: The values of $u$, $v$, and $\mu$ at all FEM vertices on the boundary of the domain corresponding to entires in `xct` and `yct`. 
- `basal_c` and `mu`: The values of $C$ (a.k.a. $\alpha^2$) and $\mu$ corresponding to entries in `xd` and `yd`
- `xd_h` and `yd_h`: The x- and y- coordinates of FEM vertices at which thickness are calculated. These are the same as `xd` and `yd`, but this distinction is used in expectation of real data having different points where thickness data is available vs. those where velocity data is available.
- `hd` and `sd`: The thickness and surface elevation at FEM vertices corresponding to entries in `xd_h` and `yd_h`.
The nomenclature here mostly follows the DIFFICE_jax code developed by our group.

## Data Specification (SI units)
- `data-1`: $n=3$, $B=10^8$, $C_w=6.6247\times 10^4$, ice geometry is not in mass balance equilibrium (that is $\partial_t h \neq 0$)
