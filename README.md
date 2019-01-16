# SU(3) CG Code

This is a code written using Mathematica 11.3 to implement an algorithm given in :
 
"SU(3) Clebsch-Gordan coefficients and some of their symmetries" 
by Alex Clésio Nunes Martins, Mark W. Suffak and Hubert de Guise 

## Usage
This code is designed to calculate reduced SU(3) Clebsch-Gordan coefficients states in the irreducible representation $(p_2,q_2)$ that occurs in the tensor product  $(p_1,q_1)\otimes (\lambda,0)$.  This coupling is multiplicity-free, i.e. the irrep $(p_2,q_2)$ occurs at most once in the reduction of the decomposition.  The algorithm is limited to the couplings of the type above and will not work for general $(\lambda,\mu)$ irrep.

In addition to the dependence on $(p_1,q_1)$, $(\lambda,0)$ and $(p_2,q_2)$, the reduced CG  $\left\langle 
{(p_1,q_1) \atop {\nu_1; I_1}} \, 
{(\lambda,0)\atop {n_1; I_2} }\Big\Vert 
{(p_2,q_2)\atop N_1 ;I_3}\right\rangle$ 
depends on occupation numbers $\nu_1, n_1$ and $N_1$, and on $\mathfrak{su}(2)$ quantum numbers $I_1,I_2$ and $I_3$.  

The syntax is

    su3cg[{p1,q1},{nu1,I1},{lambda,0},{n1,I2},{p2,q2},{N1,I3}]

Defining 
$$
k=\frac{1}{3}(p_1+2q_1+\lambda -p_2-2q_2) \tag{1}
$$
the possible values of $N_1,I_3$ that will produce a non-zero reduced CG are

 1. $\vert I_2-I_1\vert \le I_3\le I_1+I_2$,
 2. $\nu_1+n_1=N_1+k$

The file contains three ancillary routines. 

The first is `SU32SU2[{p1,q1}]` that will list all possible values of the pairs $\nu_1 I_1$ in the irrep $(p_1,q_1)$.

The second is `decomposition[p1,q1,lambda]` that will list the sum of irreps $(p_2,q_2)$ in the decomposition $(p_1,q_1)\otimes(\lambda,0)$.

The last is `su3dim[lambda,mu]` that will return the dimension of irrep $(\lambda,\mu)$.


### Example

Consider the problem of obtaining some reduced CG for states in the decomposition of the product $(3,2)\otimes (3,0)$.  

    decomposition[3,2,3]={{6, 2}, {4, 3}, {2, 4}, {0, 5}, {5, 1}, {3, 2}, {1, 3}, {4, 0}, {2,1}} 
  is the list of all $(p_2,q_2)$ in this tensor product.

    SU32SU2[{3,2}]={{5, 1}, {4, 3/2}, {4, 1/2}, {3, 2}, {3, 1}, {3, 0}, {2, 5/2}, {2, 3/2}, {2, 1/2}, {1, 2}, {1, 1}, {0, 3/2}}
  is the list of all possible $\nu_1,I_1$ pairs in irrep $\{3,2\}$.

and finally 
`su3cg[{3,2},{2,3/2},{3,0},{2,1/2},{0,5},{4,2}]`
returns $\frac{\sqrt{3}}{5}$ as the reduced CG for $(p_2,q_2)=(0,5)$ and $(N_1,I_3)=(4,2)$.

### Performance
The code for `su3cg[{3,2},{2,3/2},{3,0},{2,1/2},{0,5},{4,2}]` should take a fraction of a second to run. 

We have tested the performance for larger irreps, greater $k$ values, and certain combination of quantum number $(\nu_1,I_1), (n_1,I_2),(N_1,I_3)$ by computing selected reduced CG in the tensor product $(75,60)\otimes (53,0)$, of dimensions $304695$ and $1485$ respectively.  There are $1485$ distinct reduced CG in the figure, and the total computation time was approximately $8$ hours on standard desktop, for an average of $20$ seconds per reduced Clebsch-Gordan coefficient.  Some individual coefficients took upwards of 90 seconds to evaluate and there doesn't seem to be an obvious recipe to reduce long calculation times.  

### References

 1. Alex Clésio Nunes Martins, Mark W. Suffak and Hubert de Guise, "SU(3) Clebsch-Gordan coefficients and some of their symmetries",
 2. For additional details on the notation see also: Rowe, D. J., B. C. Sanders, and H. de Guise. "Representations of the Weyl group and Wigner functions for SU(3)." Journal of Mathematical Physics 40.7 (1999): 3604-3615 (available [here](http://hdeguise.lakeheadu.ca/pdffiles/su3wignerfunction.pdf) or [here](https://www.researchgate.net/profile/Barry_Sanders/publication/2094364_Representations_of_the_Weyl_group_and_Wigner_functions_for_SU3/links/579402c308aeb0ffcce52f2c/Representations-of-the-Weyl-group-and-Wigner-functions-for-SU3.pdf)).
