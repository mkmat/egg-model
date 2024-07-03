# egg-model
Supplementary Information for [DOI: 10.1142/S0218202522500300](https::doi.org/10.1142/S0218202522500300)


M. Kröger, P. Ilg<br>
Combined dynamics of magnetization and particle rotation of a suspended superparamagnetic particle in the presence of an orienting field: Semi-analytical and numerical solution<br>
Math. Mod. Meth. Appl. Sci. 32 (2022) 1349-1383<br>

    @article{egg-model,
       author = {M. Kr\"oger and P. Ilg},
       title = {Combined dynamics of magnetization and particle rotation of a suspended superparamagnetic particle in the presence of an orienting field: Semi-analytical and numerical solution},
       journal = {Math. Mod. Meth. Appl. Sci.},
       volume = {32},
       pages = {1349-1383},
       year = {2022},
       doi = {10.1142/S0218202522500300}
      }

## Abstract

The magnetization dynamics of suspended superparamagnetic particles is governed byinternal N .eel relaxation as well as Brownian diffusion of the whole particle. We herepresent semi-analytical and numerical solutions of the kinetic equation, describing thecombined rotation of particle orientation and magnetization. The solutions are based onan expansion of the joint probability density into a complete set of bipolar harmonics,leading to a coupled set of ordinary differential equations for the expansion coefficients.Extending previous works, we discuss the spectrum of relaxation times as well as con-vergence and limits of applicability of the method. Furthermore, we also provide thenumerical scheme in electronic form, so that readers can readily implement and use themodel.

## Files contained in this repository     

Each folder contains sufficient information to immediately construct matrix ${\bf A}$ and vector ${\bf d}$,
that enter the equation of change for the vector of moments ${\bf b}$

$$\frac{db}{dt} = A\cdot b - d$$

for any choice of parameters $\tau_B$, $\tau_D$, $h$, and $\kappa$. The user can choose
between different orders, files within ORDER-*n* result in a $[I(n)-1]\times [I(n)-1]$ matrix $A$. 

-----------------------------
Non-matlab users: 
-----------------------------

Files: 

1) b-components.txt contains the multi-indices $\{l_1,l_2,L,M\}$ of the $I(n)-1$ moments that form vector ${\bf b}$.

2a) C-B-components.txt  contains all nonvanishing components of matrix CB in the form $[i,j,C_{ij}]$

2b) C-D-components.txt  contains all nonvanishing components of matrix CD

2c) C-Bh-components.txt contains all nonvanishing components of matrix CBh

2d) C-Dh-components.txt contains all nonvanishing components of matrix CDh

2e) C-Dk-components.txt contains all nonvanishing components of matrix CDk

The matrix ${\bf A}$ is then computed, with values for $\tau_B$, $\tau_D$, $h$, and $\kappa$ at hand, as follows: 

$${\bf A} = \frac{u_h}{\tau_B} {\sf CBh} + \frac{u_h}{\tau_D} {\sf CDh} + \frac{u_\kappa}{\tau_D} {\sf CDk} + \frac{1}{\tau_B}{\sf CB} + \frac{1}{\tau_D}{\sf CD}$$ 

where the following abbreviations were used: $u_h=-4\pi h/\sqrt{3}$ and $u_\kappa=-8\pi\kappa/\sqrt{45}$.

3a) d-Bh-components.txt contains all nonvanishing components of vector dBh in the form $[i,d_{i}]$

3b) d-Dh-components.txt contains all nonvanishing components of vector dDh

3c) d-Dk-components.txt contains all nonvanishing components of vector dDk

The vector ${\bf d}$ is computed as follows:

$${\bf d} = \frac{u_h}{\tau_B} {\sf dBh} + \frac{u_h}{\tau_D}{\sf dDh} + \frac{u_\kappa}{\tau_B}{\sf dBh}$$

-----------------------------
matlab users: 
-----------------------------

All the above sparse matrices CB, CD, .., dDk can be loaded with a single command and the matrix A 
and vector d are then computed, with values for tauB ($\tau_B$), tauD ($\tau_D$), h ($h$), and kappa ($\kappa$) at hand, as follows: 

      load('load-order-8.mat')

      uh=-4*pi*h/sqrt(3); 
      ukappa=-8*pi*kappa/sqrt(45); 
      tau_inverse = CB/tauB + CD/tauD; 
      A = (uh/tauB)*CBh + (uh/tauD)*CDh + (ukappa/tauD)*CDk + tau_inverse;
      d = (uh/tauB)*dBh + (uh/tauD)*dDh + (ukappa/tauB)*dBh; 

The multi-indices of the components of **b** are provided in b-components.txt.

Matrix **A** and vector **d** can be converted to full matrices via full(A) and full(d). 
The stationary **b** is for example calculated via 

      b_stat = A\d; 

Full example: plot $b_{10}^{10}$ versus $h$ at $\kappa=0$ and compare with the exact result.  

      kappa=0; tauB=1; tauD=1; load(['load-order-8.mat']);
      for h=0:0.1:10; 
         uh=-4*pi*h/sqrt(3); 
         ukappa=-8*pi*kappa/sqrt(45); 
         tau_inverse = CB/tauB + CD/tauD; 
         A = (uh/tauB)*CBh + (uh/tauD)*CDh + (ukappa/tauD)*CDk + tau_inverse;
         d = (uh/tauB)*dBh + (uh/tauD)*dDh + (ukappa/tauB)*dBh; 
         bstat = A\d; 
         figure(1); plot(h,bstat(82),'ks'); hold on; plot(h,sqrt(3)*(coth(h)-1/h),'gx'); 
      end


-----------------------------
Additional hints:
-----------------------------
According to b-components.txt, for the case of *n=8*, the $b_{10}^{10}$-component of ${\bf b}$ is row 82,
and the $b_{22}^{00}$-component of ${\bf b}$ is row 325 in the vector or moments.



