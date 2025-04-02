# HYPRE_ex3
Example 'ex3' from HYPRE ported to GPU clusters.

Files should be used as replacements in src/examples/ of the HYPRE library.

The idea is to make 'ex3.c' work on GPU clusters to have an example of the Struct interface combined with GPU-aware MPI (while *NOT* using unified memory).

Can be compiled as:  
make use_cuda=1 ex3

For HYPRE configured with  
./configure --with-cuda --with-gpu-arch=80  
the example runs fine when invoked as  
mpirun -np 1 ./ex3 -n 100 -solver 0 -v 1 1  
or
mpirun -np 4 ./ex3 -n 50 -solver 0 -v 1 1 

I obtain output:
b,b: 9.609803e-05
Iters       ||r||_2     conv.rate  ||r||_2/||b||_2
    1    2.499214e-03    0.254945    2.549448e-01
    2    1.016622e-04    0.040678    1.037056e-02
    3    1.619374e-06    0.015929    1.651924e-04
    4    2.948699e-08    0.018209    3.007968e-06
    5    6.974962e-10    0.023654    7.115158e-08
Iterations = 5
Final Relative Residual Norm = 7.11516e-08


For HYPRE configured with  
./configure --with-cuda --with-gpu-arch=80 --enable-gpu-aware-mpi  
the example runs fine when invoked as  
mpirun -np 1 ./ex3 -n 100 -solver 0 -v 1 1  
but fails for more GPUs with errors like: 

