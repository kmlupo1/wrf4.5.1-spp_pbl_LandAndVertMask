# wrf4.5.1-spp_pbl_LandAndVertMask
WRFv4.5.1 with mods to enable land/sea masking and vertical tapering of spp-pbl 

NEW: registry.weightmask → registry.stochmask
Declare new namelist and output variables
MOD: registry.em_shared_collection
Add line to include new registry file in compile
MOD: module_initialize_real.F
Add code to compute the distance to the nearest land/sea grid points & produce a tapered mask field based on the users’ specifications in the namelist
Currently requires running real.exe as one process
MOD: module_first_rk_step_part2.F
After each call to rand_pert_update, if the user requests in the namelist - multiply the perturbation field by the weighted mask at all vertical levels. This should be the field passed to the parameterization schemes at the next timestep.
Since the rand_pert_update routine uses spectral coefficients to update the field, there should be no impact on the evolution of the perturbations by modifying the pert field here.
After the horizontal weighting step, the vertical grid is searched for levels between two specified levels (i.e., a weighting window), a mask is applied such that perturbations within the window are tapered from a maximum (or minimum) value below the window to a minimum (or maximum) value above the window
