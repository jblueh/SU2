## HL-CRM Test Case

This test case is an adapted version of [this one](https://github.com/pcarruscag/SU2/tree/engineering_note_on_adjoints/Paper_Data/HLCRM).

The mesh needs to be obtained separately, it is available [here](https://hiliftpw-ftp.larc.nasa.gov/HiLiftPW3/HL-CRM_Grids/Committee_Grids/B3-HLCRM_UnstrHexPrismPyrTet_PW/FullGap/CGNS/), we use the coarse version.

Start computing a primal solution with `SU2_CFD`, with settings to ramp CFL.

```
SU2_CFD config_primal_1.cfg
```

Convert the intermediate solution output (`restart.dat`) to a solution input (`solution.dat`).
Continue converging the primal solution, with fixed CFL (will load `solution.dat`).
If needed, repeat this until convergence (no substantial changes in `rms[Rho]` any more).

```
mv restart.dat solution.dat
SU2_CFD config_primal_2.cfg
```

Convert the final solution output (`restart.dat`) to a solution input (`solution_0.dat`).
Run discrete adjoint computations with `SU2_CFD_AD` (will load `solution_0.dat`).

```
mv restart.dat solution_0.dat
SU2_CFD_AD config_adjoint.cfg
```
