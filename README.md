# HYPRE_ex3
Example 'ex3' from HYPRE ported to GPU clusters.

Files should be used as replacements in src/examples/ of the HYPRE library.

The idea is to make 'ex3.c' work on GPU clusters to have an example of the Struct interface combined with GPU-aware MPI (while *NOT* using unified memory).

Can be compiled as:
make use_cuda=1 ex3

For HYPRE configured with
./configure --with-cuda --with-gpu-arch=80
the example runs fine when invoked as
mpirun -np 1 ./ex3 -n 33 -solver 0 -v 1 1
mpirun -np 4 ./ex3 -n 33 -solver 0 -v 1 1

For HYPRE configured with
./configure --with-cuda --with-gpu-arch=80 --enable-gpu-aware-mpi
the example runs fine when invoked as
mpirun -np 1 ./ex3 -n 33 -solver 0 -v 1 1
but fails for more GPUs with errors like:
[acn35:1588861:0:1588861] Caught signal 11 (Segmentation fault: invalid permissions for mapped object at address 0x153b88e00004)
==== backtrace (tid:1588861) ====
0 0x0000000000012d10 __funlockfile() :0
1 0x00000000009a6851 hypre_FinalizeCommunication() /scratch/project/open-29-3/hypre-master_paragpu5/src/struct_mv/struct_communication.c:1216
2 0x00000000009b379e hypre_StructMatrixAssemble() /scratch/project/open-29-3/hypre-master_paragpu5/src/struct_mv/struct_matrix.c:1436
3 0x0000000000996886 HYPRE_StructMatrixAssemble() /scratch/project/open-29-3/hypre-master_paragpu5/src/struct_mv/HYPRE_struct_matrix.c:323
