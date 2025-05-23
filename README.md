<h1>README file for HysolSim: A Digital Twin (Simulator) for Hybrid Solar Thermal Power Plant</h1>

To begin working with the code, extract the contents of the **`HysolSim_Code.zip`** file. The main file to run the simulator is **`All_HSTPP_Simulator.m`** (inside HysolSim_Code folder).

##
**HysolSIm**: **`HysolSim`** is a **`MATLAB`**-based codebase that serves as a digital twin (simulator) for a Hybrid Solar Thermal Power Plant where both Parabolic Trough Collectors (PTC) and Linear Fresnel Reflectors (LFR) work as the concentrating mirrors. Note that the simulator can also be run in PTC-only mode, without the LFR. Further details are provided in the following section. An article using the simulator, authored by Baidya et al. (2025), has been submitted to SoftwareX.

Details on how to test the simulator are provided in Section 3 of this README file.  

# 1 Initialisation

Before starting the Hybrid solar thermal power plant (HSTPP) simulator, ensure various parameters values and initial conditions for the plant are initialized as desired. All the initial conditions and parameter values are specified in the file `‘HSTPP_Parameters.m’`. Values can be changed in this file by the user[^1]. After specifying values in the `‘HSTPP_Parameters.m’` file, run (execute) the file. It will then ask the user to choose Solar radiation `Type`  `Type0`/`Type1`/`Type2`/`Type3`). The details about the various `Types` of solar radiation will be discussed later after this section. After choosing the `Type`, a message asking the user to wait for some time will be displayed. During this time the value of the newly changed parameters will overwrite the earlier ones in the respective `‘.mat’` file. All the workspace variables are automatically saved in the `‘.mat’` file which will be loaded in the script `‘Hybrid.m/Oil_only.m’` file for running the simulator.

**Note:** Users can readily utilize the provided '.mat' files within the
simulator without the necessity to access the `‘HSTPP_Parameters.m’`
file. This convenience allows users to seamlessly engage with the
simulator, bypassing the need to adjust parameter values or initial
conditions unless desired. The `‘HSTPP_Parameters.m’` file becomes
relevant only when users intend to modify such parameters or initial
conditions to tailor the simulation to their specific requirements.

# 2 Simulator structure

The simulator has several pre-configured options. These are mentioned
below:

## 2.1 Different **`Types`** for the simulator: (Depends on the solar radiation profile)

The Simulator comes with various `Types` depending upon the solar
radiation profile. `Type0` for constant solar radiation, `Type1` is for
constant with step change solar radiation, `Type2` is for Real-time
(data extracted from plant) solar radiation, and `Type3` is for
Quadratic with cloud cover disturbance solar radiation. The proposed
simulator comes with 4 pre-generated `‘.mat’` files corresponding to
four different types of solar insolation profiles as follows,

-   `Type 0`: This is for the constant solar radiation (For ease of
    understanding the dynamic behavior of the plant) which loads from
    the `‘lfr_I_sec_700_all_Lgp.mat’` file. The purpose of running
    simulator with this profile is to enable the user to check if the
    code is working or not as there is no variation in the solar
    radiation.

-   `Type 1`: This `Type` is for running the simulator with constant but
    step change type solar radiation from $700\ \mathrm{W/m^2}$ to $400\ \mathrm{W/m^2}$,
    which loads from the `‘lfr_I_sec_step_all_Lgp.mat’` file.

    This change happens in one second. This profile is also for
    simulator testing purposes. Note that the user should not give the
    solar radiation below $400\ \mathrm{W/m^2}$ as it may lead to some error. The
    error may be in terms of complex numbers, or some variable may
    appear as not a number (NaN). Such errors can usually be identified 
    and corrected through careful debugging.

-   `Type 2`: This is for the Real data of solar radiation (Run the
    simulator with this type to get the idea of how solar plants behave
    in real aspects) which loads from `‘lfr_I_sec_real_all_Lgp.mat’`
    file. This real-time data has been taken from the Gurgaon HSTPP
    plant for a summer day in 2014 [\[1\]](#ref1), [\[2\]](#ref2). This
    solar insolation data is saved as `‘maymonth.xlxs’` file.\
    **Note:** For Ubuntu, '.xlxs' will work, but for Windows OS,
    `‘maymonth.xlxs’` should be changed to `‘maymonth.csv’` format.

-   `Type 3`: This is for the Quadratic type solar radiation with cloud
    cover present for some time, which loads from
    `‘lfr_I_sec_quad_all_Lgp.mat’` file. Cloud cover acts as a
    disturbance.

## 2.2 Different **`Modes`** of the Simulator:

-   Mode 1: In this mode, only the Parabolic Trough Collector (PTC)
    field is operational i.e. steam is produced only from the Steam
    generator (SG). The Linear Fresnel Reflector (LFR) field is
    non-operational.

-   Mode 2: Here both PTC as well as LFR fields are operational. Thus,
    steam will be produced from the Steam generator (SG) as well as the
    LFR.

## 2.3 Different **`Startup types`** of the simulator:

The simulator can replicate a two-day simulation with identical daily
solar radiation profiles. The first day represents a Cold startup,
starting with all initial conditions set to normal atmospheric
conditions. Conversely, the second day corresponds to a Hot startup,
where all startup conditions are derived from dynamic nighttime cooling
data.

-   Only Cold startup: In this scenario, the plant will commence
    operations from its initial state, which is presumed to be under
    nominal atmospheric conditions. It is important to note that the
    plant has been inactive for several days prior to this point.

-   Only Hot startup: For this case, the initial condition of the plant
    depends on the data obtained from the cooling process during the
    nighttime of the last active day of operation. This application will only function
    after a Cold startup has been executed at least once. During the Cold startup process,
    a `.mat` file is generated, which is essential for subsequent Hot startup operations.
    Users must run the Cold startup initially to ensure the required data file is available for Hot startup to utilize.

-   Cold+Hot startup: In this scenario, the plant will undergo both cold
    startup and hot startup scenarios, necessitating a comprehensive
    two-day simulation inclusive of night-time cooling.

The user can choose any combination depending on the Mode, Type &
Startup type for the simulator to run. To start the simulator, open
`‘All_HSTPP_Simulator.m’` in MATLAB. A snapshot of this file is shown
below in [Figure 1](#fig1).

<p align="center">
  <img src="Readme_images/Front.png" alt="Main script of the Simulator" width="70%">
</p>

<p id="fig1" align="center"><b>Figure 1:</b> Main script of the Simulator</p>


# 3 Simulation Procedure (To test/run the simulator)

All the `‘.m’` files, `‘.mat’` files, the `‘.xlsx’` file, and the `‘iMAGE’` folder should be placed inside a single main
directory. Within the iMAGE folder, there must be four subfolders named `‘Type0’`, `‘Type1’`, `‘Type2’`, and
`‘Type3’`. Each of these Type folders should, in turn, contain four additional subfolders: `‘Day1_hybrid’`,
`‘Day1_oilonly’`, `‘Day2_hybrid’`, and `‘Day2_oilonly’`, which are designated for saving the generated images. If any of these folders are missing, the user should create them before proceeding with code testing. The folder structure is illustrated in the figure below in [Figure 2](#fig2)

   <p align="center">
      <img src="Readme_images/Folder_Structure.jpg" alt="Folder Structure" width="50%">
   </p>

   <p id="fig2" align="center"><b>Figure 2:</b> Selecting Types</p>

The steps involved in running the simulator are briefly discussed below:

-   Execute the file `‘All_HSTPP_Simulator.m’` in MATLAB. A message will
    be displayed as shown in [Figure 3](#fig3) to select the Type of simulation.

    <p align="center">
      <img src="Readme_images/Type.png" alt="Selecting Types" width="70%">
   </p>

   <p id="fig3" align="center"><b>Figure 3:</b> Selecting Types</p>

-   User can type any number from `0` to `3` and press enter. Another
    message will be displayed on the screen asking the user to select
    the `Mode` of operation as shown in [Figure 4](#fig4).

    <p align="center">
     <img src="Readme_images/Mode.png" alt="Selecting Mode" width="70%">
    </p>

    <p id="fig4" align="center"><b>Figure 4:</b> Selecting Mode</p>

-   User can choose either 1 or 2 and press enter. Another message will
    come on the screen to select the `Startup` condition as shown in
    [Figure 5](#fig5).

    <p align="center">
     <img src="Readme_images/Startup1.png" alt="Selecting Startup condition" width="70%">
    </p>

    <p id="fig5" align="center"><b>Figure 5:</b> Selecting Startup condition</p>


-   User can choose either `1`, `2` or `3` and press enter.

-   Now the simulation will start. Wait for the simulation to end. As an
    example, if Hybrid plant (Mode 2) is simulated with startup option
    3, then a total `26` different figures for Day-1 (Cold startup
    with Figure number `1` to `26`) and another `26` figures for Day-2
    (Hot startup with Figure number `31` to `56`) will get generated.
    Thus, the user should wait till the last Figure is displayed to know
    that the simulation is complete. For Oil-only mode (Mode 1), Figures
    1-11 will be generated for cold startup, while Figures `31` to
    Figure `41` will be generated for warm startup.

-   If the performance of the plant needs to be evaluated, then the user
    should execute the file `‘Performance_calculation.m’`.

**Note:** The simulator has been successfully tested in Ubuntu 22.04.3
LTS, and Windows 11/10 operating systems. MATLAB version R2023a has been
used for simulation.

# 4 Simulation Time

In this section, we present some illustrative numbers to give an idea of
the time taken for simulation. We consider two day simulation (startup
option 3) for various solar insolation profiles and both modes (PTC
alone, hybrid) of simulation. All the simulations have been performed on
a PC with the following specifications:

-   Intel Code i9 13900K processor

-   Asus Strix Z790 F

-   64 GB DDR5 RAM, 5200MHz

-   Samsung 980 M.2 SSD

-   Ubuntu 22.04.3 LTS Operating System

Table [1](#tab1) lists the time needed (in seconds) for simulation for various cases. This
simulation time is recorded in variable 'Simu_time' in Seconds.

<p id="tab1"><b>Table 1: Simulation time</b></p>

| Solar Radiation Type    | Day   | Oil Only Loop | Hybrid Loop |
|------------------------|-------|---------------|-------------|
| Type-0 (Constant)       | Day-1 | 7166 sec          | 6847    sec    |
|                        | Day-2 | 7307   sec        | 7292    sec    |
| Type-1 (Step change)    | Day-1 | 7236   sec       | 7280   sec     |
|                        | Day-2 | 7348   sec       | 7384    sec   |
| Type-2 (Real-time)      | Day-1 | 7220  sec        | 7268    sec    |
|                        | Day-2 | 7334    sec      | 7371    sec    |
| Type-3 (Quadratic)      | Day-1 | 7203    sec      | 7252   sec     |
|                        | Day-2 | 7322     sec     | 7357    sec    |




# 5 Results

All the simulation plots are automatically saved into `‘iMAGE’` folder
present in the Simulator. Within the `‘iMAGE’` folder, there are four
other folders corresponding to four Types of Solar radiation profiles
(Constant-Type0, Step change-Type1, Realtime-Type2, and Quadratic-Type3). With each such
sub-folder, the cold startup (Day-1) and warm startup (Day-2) results
for both the oil-only loop and hybrid loop folders are given. Inside the respective folders, the user can find all the generated images.

# 6 Abbreviation

-   PTC: Parabolic trough collector.

-   HT: High-temperature tank.

-   LT: Low-temperature tank.

-   SH: Super heater.

-   SG: Steam generator.

-   PH: Pre heater.

-   LFR: Linear fresnel reflector.

-   SD: Steam Drum.

# 7 Description about different function files in the Simulator

The simulator consists of several `‘.m’` files which are briefly
described below:

1.  **solarfield**: Contains dynamic model of the Parabolic trough
    collector (PTC). Here, `3` PDEs of PTC converted to `45` ODEs.

2.  **density_oil**: Computes density of oil (used in PTC field) given
    its temperature.

3.  **Cp_sf/Cp_oil**: Computes specific heat capacity (Cp) of oil given
    its temperature.

4.  **Kin_vis**: Computes Kinematic viscosity of oil given its
    temperature.

5.  **Ther_con**: Computes Thermal Conductivity of oil given its
    temperature.

6.  **h_oil**: Function file for calculating Enthalpy of oil given its
    temperature.

7.  **dbyden_sf**: Computes derivative of the density of oil with
    respect to oil temperature.

8.  **dbyCp_sf**: Computes derivative of Specific heat capacity (Cp) oil
    with respect to oil temperature..

9.  **PTC_HT_LT_connection**: Connection file for PTC-HT-LT.

10. **PTC_LT_connection**: Connection file for PTC-LT.

11. **HTtank**: Contains dynamical model of HT tank.

12. **LTtank**: Contains dynamical model of LT tank.

13. **Energy_Compute_PTC**: Computes the energy produced by PTC.

14. **Energy_Compute_LFR**: Computes the energy produced by LFR.

15. **LFR_sub_fun1**: Contains some initialization parameters for the
    LFR field.

16. **LFRsimulation**: Contains dynamic model of LFR.

17. **Xsteam**: Computes steam and water properties.

18. **ffrough**: Computes surface friction factor for the LFR pipe
    produced from water/steam.

19. **Ffcw**: Computes the friction factor used in ffrough.

20. **h2o_mug**: Computes the dynamic viscosity of saturated steam at a
    given pressure.

21. **h2o_muf**: Computes the dynamic viscosity of saturated water at a
    given pressure.

22. **compute_hp**: Computes Heat Transfer Coefficient (HTC) for single
    and two-phase flow in LFR.

23. **absorb_exp/absorb**: Function for Energy balance for LFR absorber
    pipe and LFR glass envelope.

24. **SD_dynamics_updated_ode**: Contains dynamic model of the Steam
    drum.

25. **HXSG_subcool**: Function file for the steam generator in subcooled
    condition (Dynamic model).

26. **HXSG_sat**: Function file for steam generator in saturation
    condition (Dynamic model).

27. **HXSH_bef_PTC**: Superheater function file when only LFR is
    generating steam (Dynamic model).

28. **PTC_HT_SH_SG_PH_LT_connection**: Connection file for PTC, HT, SH,
    SG, PH and LT.

29. **mixer_SH**: Function files the mixing of steam produced from LFR
    and SG (Static model).

30. **HXSH**: Contains dynamic model of the superheater.

31. **HXPH**: Contains dynamic model of the preheater.

32. **All_HSTPP_Simulator**: Main file to run the simulator.

33. **Hybrid**: Main file for the whole hybrid (PTC+LFR) plant for day-1
    (Cold start-up).

34. **Oil_only**: Main file for PTC only loop for day1 (Cold start up).

35. **day2**: Main file for whole hybrid (PTC+LFR) plant for day2 (Hot
    start up).

36. **day2_oil_only**: Main file for PTC only loop for day2 (Hot start
    up).

37. **HSTPP_Parameters**: File assigning values to various parameters
    and initialization variables.

38. **h2o_mu**: It gives Dynamic viscosity at a given temperature and
    density of water.

39. **h20_muf**: It gives Dynamic viscosity of the saturated water at a
    given pressure.

40. **H2o_mug**: It gives Dynamic viscosity of saturated steam at a
    given pressure.

41. **h2o_rhof**: It gives the Saturated density of water at a given
    pressure.

42. **h2o_rhog**: It gives Saturated steam density at a given pressure.

43. **h2o_rhos**: It gives the Superheated density of steam at a given
    temperature and pressure .

44. **h2o_rhol**: It gives Subcooled density of water at a given
    temperature and pressure.

45. **h2o_tsat**: It gives Saturation temperature of water at a given
    pressure in °C .

46. **HXSG_night1**: It contains the dynamic model for SG nighttime
    cooling.

47. **Nighttime_cooling**: It contains the dynamic model of nighttime
    cooling for hybrid plants (PTC+LFR).

48. **Nighttime_cooling_Oil_only**: It contains dynamic model of
    nighttime cooling for the PTC plant.

49. **Plot_coldSU**: It will give all the plots for the cold startup of
    HSTPP.

50. **Plot_hotSU**: It will give all the plots for the hot startup of
    HSTPP.

51. **Plot_oil_only**: It will give all the plots for the cold startup
    of the Oil loop (PTC-only plant).

52. **Plot_hotSU_oil_only**: It will give all the plots for the hot
    startup of the Oil loop (PTC only plant).

53. **SD_night1**: It contains a dynamic model for SD nighttime cooling.

54. **SetGraphics**: For the purpose of the graphic.

55. **UAF_PH_fun**: It is used to calculate the heat transfer
    coefficient of PH while the PH mass flow rate of water is greater
    than 0.01 kg/sec.

56. **UAF_PH_mw0**: It is used to calculate heat transfer coefficient of
    PH while PH mass flow rate of water is less than 0.01 kg/sec.

57. **UAF_SG_fun**: It is used to calculate the heat transfer
    coefficient of SG in a saturated condition.

58. **UAF_SG_sub_fun**: It is used to calculate the heat transfer
    coefficient of SG in subcooled conditions.

59. **UAF_SH_fun**: It is used to calculate the heat transfer
    coefficient of SH.

60. **Performance_calculation**: With the help of this file, the user
    can evaluate different performance criteria of the plant.

61. **enthal_temp_HT/enthal_temp_LT**: Function file for calculating
    Enthalpy of HT/LT oil.

62. **event_fun_SD**: Provides 'options' for Steam drum dynamics.

63. **Extra_Enthalpy_Temp_Convert**: To find the Temperature of oil from
    oil Enthalpy. It is an extra file for checking purposes.

64. **h2o_hf**: To find Saturated enthalpy of water at a given pressure.

65. **h2o_hfg**: To find Latent Heat of Evaporation of vapour at a given
    pressure.

66. **h2o_hg**: To find Saturated enthalpy of vapour at a given
    pressure.

67. **h2o_hl**: To find Subcooled enthalpy of water at a given
    temperature and pressure.

68. **h2o_hs**: To find Superheated enthalpy of steam at a given
    temperature and pressure.

69. **h2o_mul**: To find Dynamic viscosity of the subcooled water.

70. **h2o_psat**: To find Saturation pressure of water at a given
    temperature in kPa.

71. **h2o_rhotp**: To find Two-phase density of water in kg/m^3.

72. **h2o_sigma**: To find the surface tension of water at a given
    temperature.

73. **h2o_sl**: To find the subcooled entropy of water at given
    temperature.

74. **h2o_ss**: To find the Superheated of steam entropy at a given
    temperature and pressure.

75. **HXSG_height**: To find the Steam generator height of water.

# 8 Case Study

To illustrate how the simulator can be used to investigate different
designs, we perform a case study (Case Study-2 in the paper 'A Simulator
for Hybrid Solar Thermal Power Plant' by Baidya et al.
[\[3\]](#ref3) for two scenarios: (i) Scenario-1: the HT and LT tanks
contain 5000 kg of oil each, and (ii) Scenario-2: the HT and LT tank
contain 15000 kg, and 5000 kg of oil, respectively, i.e. more oil is
available in Scenario-2. For Scenario-1, load
`‘lfr_I_sec_HX_Case_study_Low.mat’` and for Scenario-2, load
`‘lfr_I_sec_HX_Case_study_high_Low.mat’` file. The user should ensure
that while considering Scenario-1, the line
`‘load lfr_I_sec_HX_Case_study_high_Low.mat %Scenario-2`' should be
commented. Similarly, while considering Scenario-2, the line
`‘load lfr_I_sec_HX_Case_study_Low.mat %Scenario-1’` should be
commented. In [Figure 6](#fig6), line
no 22 is for Scenario-1, and line no 23 is for Scenario-2.

This specific Case study simulation will be performed with the options:
`Type`=2, `Mode`=2, and `Startup`=1. The purpose of this case study,
along with all the simulation results and discussions, are well
described in the paper published by Baidya et al. [\[3\]](#ref3).

<p align="center">
  <img src="Readme_images/SC.png" alt="Case Study" width="100%">
</p>

<p id="fig6" align="center"><b>Figure 6:</b> Case Study</p>

[^1]: The simulator has not been tested extensively for various initial
    conditions or parameter values. Thus, provide reasonable values for
    these quantities



## References

<p id="ref1">[1]. I. B. Project, National solar thermal power plant, https://www.ese.iitb.ac.in/~NSTPP/.</p>
<p id="ref2">[2]. S. Kannaiyan, S. Bhartiya, M. Bhushan, Dynamic modeling and simulation of a hybrid solar thermal power plant, Industrial & Engineering Chemistry Research 58 (2019) 7531–7550. doi: 10.1021/acs.iecr.8b04730.</p>
<p id="ref3">[3]. D. Baidya, S. Kannaiyan, M. Bhushan, S. Bhartiya, Simulator for hybrid solar thermal power plant, SoftwareX (2025).</p>
