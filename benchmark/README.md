## Source Code

The upstream version of SU2 already contains the changes for hybrid parallel discrete adjoints. For the performance tests, we start in SU2's develop branch (tag `hpda_base`, commit 38cb78b7487a6fac42299c65ab76f82b84605343) and make a few changes.
- no special behaviour for the case of OpenMP parallelism with a single thread, for better comparability across different numbers of threads (tag `hpda_single_thread`, commit 04778d3ab7fc06472466ff63143d7402efcb7af0)
- a new build option to disable the preaccumulations that are also disabled in hybrid AD builds (tag `hpda_hybrid_preacc`, commit 4033dc96c5d44d45173a0e075eca0bce0d1aaf07)
- a Metis based ILU parallelization, an explorative change that modifies the distribution of nodes to OpenMP partitions (tag `hpda_metis_based_ilu`, commits 6a71815570897ca97d936ae6967daf68b883a1f5 and 6c203e220422cd004c7cff3d19257bdf85a9e876)
- smaller tolerance for the discrete adjoint GMRES solver, to have the same number of iterations across different parallel setups (tag `hpda_krylov_tol`, commit c7b488c329b767c1c536ce052e0cea2d2bdcbe02)

## SU2 Builds

If applicable, AVX512 needs to be enabled explicitly, for example as shown below.

```
export CFLAGS="$CFLAGS -march=skylake-avx512"
export CXXFLAGS="$CXXFLAGS -march=skylake-avx512"

```

The following basic build parameters are common to all builds. They also correspond to the classical MPI build with linear management.

```
--buildtype=release -Denable-autodiff=true -Denable-mixedprec=true
```

Adding `-Dcodi-tape=JacobianReuse` produces the MPI build with reuse management, and adding `-Donly-hybrid-ad-compatible-preaccs=true` on top results in the MPI build with reuse management and only hybrid-AD compatible preaccumulations.

The transition to a hybrid AD build is made by adding `-Dwith-omp=true` to the basic build parameters, and the shared reading optimization can be disabled with `-Dopdi-shared-read-opt=false`.

## Test Cases

The NACA 0012, Onera M6, and HL-CRM test cases can be found in the respective directories, with explanations on how to compute a primal solution and how to run discrete adjoint computations.

`SU2_CFD` and `SU2_CFD_AD` calls need to be complemented by suitable parallelism, by `mpirun -n ...` and/or `SU2_CFD* -t ...`, and in the case of multiple NUMA domains, binding via `hwloc` is advised.
