# TSMP-PDAF Environment variables #

A number of environment variables are set during the build process of
TSMP-PDAF in order to make the code behave in specific ways.

TSMP-PDAF environemnt variables are set in the machine-specific
TSMP-PDAF build-scripts of the form:

```
bldsva/intf_DA/pdaf/arch/<machine>/build_interface_pdaf_<machine>.ksh
```

where `<machine>` could be f.e. `JURECA` or `JUWELS`.

## CLMSA ##

Environment variable `CLMSA` is true if CLM-standalone is used in
TSMP-PDAF, i.e. no coupling to other component models (ParFlow,
atmospheric model).

`CLMSA` is used in many places in TSMP-PDAF, where CLM-standalone
specific code is introduced. This includes
- observation reading
- setting observation vector
- setting state vector
- communicator handling
- localized filters
- TSMP-PDAF-wrapper routines

## PARFLOW_STAND_ALONE ##

Environment variable `PARFLOW_STAND_ALONE` is true if
ParFlow-standalone is used in TSMP-PDAF, i.e. no coupling to other
component models (CLM, atmospheric model).

It is used less frequently than [CLMSA](#clmsa), only at code places
where the behavior of ParFlow-CLM-PDAF and ParFlow-PDAF should differ.

## OBS_ONLY_PARFLOW ##

Environment variable `OBS_ONLY_PARFLOW` is true if observations in
TSMP-PDAF are of ParFlow-type.

This will remove unnecessary code during observation reading, when
ParFlow-CLM-PDAF is built, but no CLM-type observations are included.

## OBS_ONLY_CLM ##

Environment variable `OBS_ONLY_CLM` is true if observations in
TSMP-PDAF are of CLM-type.

This will remove unnecessary code during observation reading, when
ParFlow-CLM-PDAF is built, but no ParFlow-type observations are
included.

## FOR2131 (PDAF / ParFlow build, experimental) ##

The environment variable `FOR2131` is set

1. In the PDAF-build script
   `bldsva/intf_DA/pdaf/arch/<machine>/build_interface_pdaf_<machine>.ksh`
2. In `bldsva/intf_DA/pdaf/tsmp/parflow/solver_richards.c`. Right
   before the line `#ifdef FOR2131`, one should add a line reading
   `#define FOR2131`.

The environment variable `FOR2131` affects

1. TSMP-PDAF in `enkf_parflow.c`, there are two main effects:
   - behavior of `PF:updateflag == 2` is changed to include pressure in
     the state vector
   - a check for saturations greater than 1 is included

2. `ParFlow`: There is an additional saturation update in
   `solver_richards.c`.

## WATSAT3D ##

Environment variable `WATSAT3D` is set in `common_build_interface.ksh`
in function `c_configure_clm`.

If it is turned on, the possibility of a read-in porosity is
implemented in CLM's `iniTimeConst.F90`.
