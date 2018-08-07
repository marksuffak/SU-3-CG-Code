# SU(3) CG Code

This code is designed to calculate reduced SU(3) Clebsch-Gordan coefficients of the tensor product (p_1,q_1)x(lambda,0)->(p_2,q_2). 

The code defines a function named "su3cg". The input into the function is in the following style: {p_1,q_1},{v_1,I_1},{lambda,0},{n_1,I_2},{p_2,q_2},{N_1,I_3}. Where {v_1,I_1},{n_1,I_2},{N_1,I_3} are the first quantum number and angular momentum corresponding to their preceding irrep, {p_1,q_1},{lambda,0},{p_2,q_2} respectively. 

Please note that the variable "mu" specified in the function within the irrep (lambda,mu) must be set to zero when running the function after it is initially defined; (lambda,0) is the only form of that irrep which this code is designed to calculate.

The choice of quantum numbers and angular momenta to go along with the irreps is not random. The following rules must be satisfied:

1. |I_2-I_1|<= I_3 <= I_1+I_2
2. v_1+n_1 = N_1+k where k = (1/3)(p_1+2q_1+lambda-p_2-2q_2)

To assist the user with choosing these numbers, we've included the "decomposition" and "SU32SU2" functions.

The "decomposition" function takes the input: p1,q1,lambda. The output is a list of (p2,q2) irreps which are possible from the coupling of (p1,q1)x(lambda,0). These irreps are for all k values, so if you're looking to have a specific k value, it's best to check which (p2,q2) irreps suit your needs.

The "SU32SU2" function takes an irrep ({p1,q1} for instance), and outputs all physically possible values of (v_1,I_1). The user can use this function for all irreps if they would like, and pick the sets of quantum numbers and angular momenta which suit the rules above, while ensuring they are physically possible within the specific irrep.

If the input (p2,q2) irrep is not in the decomposition of (p1,q1)x(lambda,0), or if the input sets of quantum numbers and angular momenta do not exist for the specific irreps, then the user will obtain a zero from the output of the main function. (Of course, it is important to remember that it is possible to have all numbers satisfying the appropriate conditions and still have the Clebsch-Gordan coefficient come out as zero.)

Example

Say you wanted to couple (3,2)x(3,0) and wanted k=0. Then you would type the function "decomposition[3,2,3]" and find that (0,5) is one of the (p_2,q_2) options for which k=0. You can then use "SU32SU2[{3,2}]", 
"SU32SU2[{3,0}]" and "SU32SU2[{0,5}]" to obtain lists of the possible quantum number and angular momentum sets for each irrep. After making your choice for each irrep, you can use the "su3cg" function as follows:

su3cg[{3,2},{2,3/2},{3,0},{2,1/2},{0,5},{4,2}]

and would obtain an answer of Sqrt[3]/5.

The code for this example should take a fraction of a second to run, however, for larger irrep numbers, greater k values, and certain quantum number and angular momentum sets, the run time could be upwards of 90 seconds as we have found in some cases, though there doesn't seem to be an obvious recipe for long calculation times.
