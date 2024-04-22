## Onera M6 Test Case

The mesh needs to be obtained spearately, it is available [here](https://github.com/su2code/TestCases/blob/master/rans/oneram6/mesh_ONERAM6_turb_hybrid_258969.su2).

The provided configuration files are based on [this one](https://github.com/su2code/SU2/blob/master/TestCases/rans/oneram6/turb_ONERAM6_nk.cfg).

Obtain a primal solution with `SU2_CFD`.

```
SU2_CFD turb_ONERAM6_nk.cfg
```

Convert the produced solution output (`restart.dat`) to a solution input (`solution_0.dat`).

```
mv restart.dat solution_0.dat
```

Run discrete adjoint computations with `SU2_CFD_AD` (will load `solution_0.dat`).

```
SU2_CFD_AD turb_ONERAM6_ad.cfg
```
