# Heracles
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.13844857.svg)](https://doi.org/10.5281/zenodo.13844857)


Heracles is a physics-based compact model for HfO2-based ferroelectric capacitors. It includes thermal models, interface layers and accurately reproduces several device phenomena, such as transient polarization switching and capacitance hysteresis. Heracles consists of a stateful and computationally efficient non-equilibrium thermodynamics description of the device behaviour, allowing for efficient Monte Carlo simulations even in larger analog CMOS circuits. This makes Heracles suitable for Design-Technology Co-Optimization (DTCO) approaches to (analog) compute-in-memory, neuromorphic systems or sensory circuit design.

## Examples

A Verilog-A implementation that is tested in Cadence Spectre is available, including Monte-Carlo parameters. The parameters were extracted from measurement data by the Department of Electrical Engineering of IIT Bombay, as seen in the article cited below.

### Virtuoso
To use the model in Cadence Virtuoso, create a cell called *fecap* with a symbol view (type schematicSymbol) and rename this symbol view to *spectre*. Create a symbol with 2 inout pins named te and be. Then, in the Virtuoso main window, open *Tools/CDF/Edit* and load *cdf.il*. Now import the model card in your Maestro test bench via *Setup/Model Libraries*, for example *fehlings2024.scs* for the model card of the source citation. In Setup/environment (or in the hierachy editor when used) make sure that the *spectre* view is included in switch and stop lists.

Alternatively create a *veriloga* view and paste in the source code, then import the model card in Maestro. Keep in mind that in this way mismatch Monte Carlo simulations may not work, depending on your Virtuoso configuration and CMOS PDK.

### Ngspice
There is a compiled osdi binary shipped with every release, see the releases tab. You can find ngspice-based testbenches in the repository [heracles-testbenches](https://github.com/bics-rug/heracles-testbenches).

## Documentation
[Heracles Documentation](https://bics-rug.github.io/heracles/)

The documentation is still work in progress.

## Frequently Asked Questions (FAQ)
- How do I calibrate Heracles on my device data?
  - CV and PV hysteresis data has proven most useful for a base calibration. Initially, material parameters should be kept at their default values or set to physical characterization data, if possible. Then, the most critical parameters to calibrate are these related to the switching energy barrier `w_b` and `d_e` and these related to the capactive interaction between depletion, interface and ferroelectric layer: `n_depl`, `q_fix_depl`, `eps_depl`, `eps_fe`, `eps_int` and `eps_de`. Please refer to this preprint for a more detailed procedure: [Defect-Aware Physics-Based Compact Model for Ferroelectric nvCap: From TCAD Calibration to Circuit Co-Design](https://arxiv.org/abs/2511.21267)
- Can I use Heracles without calibrating it to device data first?
  - Yes, you can use one of the existing model cards calibrade on lab devices.
- Does Heracles work for devices other than FeCaps, such as FTJs or FeFETs?
  - Heracles can be extended to also cover these devices. Dedicated FeFET and FTJ extensions are in development right now.
- Which circuit simulators can I use with Heracles?
  - Heracles is currently only compatible with the proprietary Cadence Spectre simulator, but it has been used successfully with free simulators such as ngspice or VACASK with adjustments. Future release are planned to be explicitly compatible with these free simulators as well.
- Which simulation types does Heracles support?
  - DC, transient and AC. Please not that by the nature of how SPICE works, DC and AC will only show you operating point or steady state behaviour, and hence should can not be used to simulate CV or PV hystereses, which can only be simulated in transient mode.


## Contributing

## Acknowledgements
This work was supported by the European Research Council (ERC) through the European’s Union Horizon Europe Research and Innovation Programme under Grant Agreement No 101042585. Views and opinions expressed are however those of the authors only and do not necessarily reflect those of the European Union or the European Research Council. Neither the European Union nor the granting authority can be held responsible for them.

## Citation

If you find Heracles useful in your work, consider citing on of the following articles:

[Heracles: A HfO2 Ferroelectric Capacitor Compact Model for Efficient Circuit Simulations](https://arxiv.org/abs/2410.07791)
```
@article{fehlings2025heracles,
  author={Fehlings, Luca and Hanif Ali, Md and Gibertini, Paolo and Gallicchio, Egidio A. and Ganguly, Udayan and Deshpande, Veeresh and Covi, Erika},
  journal={IEEE Transactions on Electron Devices}, 
  title={Heracles: A HfO2 Ferroelectric Capacitor Compact Model for Efficient Circuit Simulations}, 
  year={2025},
  volume={72},
  number={11},
  pages={6009-6014},
  keywords={Integrated circuit modeling;Switches;Capacitance;Electrodes;Computational modeling;Semiconductor device modeling;Hafnium compounds;Charge carrier density;Load modeling;Leakage currents;Compact model;ferroelectric devices;HfO2;nonvolatile memory;semiconductor device modeling;SPICE},
  doi={10.1109/TED.2025.3615577}}
```

[Defect-Aware Physics-Based Compact Model for Ferroelectric nvCap: From TCAD Calibration to Circuit Co-Design](https://arxiv.org/abs/2511.21267)
```
@article{fehlings2025defect,
  title={Defect-Aware Physics-Based Compact Model for Ferroelectric nvCap: From TCAD Calibration to Circuit Co-Design},
  author={Fehlings, Luca and Raut, Nihal and Ali, Md Hanif and Puglisi, Francesco M and Padovani, Andrea and Deshpande, Veeresh and Covi, Erika},
  journal={arXiv preprint arXiv:2511.21267},
  year={2025}
}


```
