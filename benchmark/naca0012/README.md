## NACA 0012 Test Case

Compute a primal solution with `SU2_CFD`.

```
SU2_CFD config_ad.cfg
```

Convert the produced solution output (`restart.dat`) to a solution input (`solution.dat`).

```
mv restart.dat solution.dat
```

Run discrete adjoint computations with `SU2_CFD_AD` (will load `solution.dat`).

```
SU2_CFD_AD config_ad.cfg
```
