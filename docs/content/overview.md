# Overview

## 1. General

Heracles is a two-terminal (`te`, `be`) compact model for a metal/HfO₂-ZrO₂(HZO)/metal ferroelectric capacitor (FeCap). It combines:

- Mixed-phase HZO: a fraction `p_ferro` of the film area is orthorhombic (ferroelectric, switches), the remainder `1-p_ferro` is monoclinic (linear dielectric, non-switching).
- A field- and phase-dependent electrode depletion/screening layer (imperfect metal screening → internal depolarization field, source of imprint/wake-up asymmetry).
- A physical interfacial low-κ "dead layer" (`t_int`, `eps_int`) in series.
- Polarization switching kinetics via a two-state Eyring/Arrhenius (transition-state) rate model — not Landau-Khalatnikov.
- Two independent leakage/conduction paths: trap-assisted tunneling (TAT) through the FE layer and through the interface layer, plus a direct Fowler-Nordheim (FN) channel through the interface layer.

## 2. Equivalent Circuit / Node Topology

Three series stages between `te` and `be`:

1. **Depletion/screening layer** (`te`→`n_depletion`): voltage-defined branch enforcing charge balance between bound polarization charge and free screening charge.

2. **Ferroelectric core** (`n_depletion`→`n_interface`): switching capacitance + polarization current, in parallel (at the `te`–`n_interface` level) with the non-switching monoclinic dielectric and the FE-layer TAT leakage.

3. **Interface dead layer** (`n_interface`→`be`): linear capacitance + TAT + FN leakage.

## 3. State Variable and Switching Kinetics

`p ≡ V(n1) ∈ [0,1]` is the occupation probability of the "down" polarization state (bistable two-well model, **not** Landau-Khalatnikov/Preisach).

**Driving force / barrier shift** (from `E_fe = V(fecap)/t_fe`):

$$w_e = (E_{fe} - E_{off})\, d_e$$

**Transition rates** (Eyring/transition-state theory, attempt frequency `kT/h`):

$$k_+ = \frac{k_BT}{h}\exp\!\left(-\frac{(w_b - w_e)q}{k_BT}\right), \qquad
k_- = \frac{k_BT}{h}\exp\!\left(-\frac{(w_b + w_e)q}{k_BT}\right)$$

**Relaxation ODE** (imposed via `I(polarization) <+ ddt(p) + k_-\,p - k_+\,(1-p) = 0`):

$$\dot p = k_+ (1-p) - k_-\, p, \qquad \tau = \frac{1}{k_++k_-},\quad p_{eq}=\frac{k_+}{k_++k_-}$$

This is a **single Debye-type relaxation**, not a distribution of nucleation sites (unlike NLS/Preisach-type HZO models, e.g. Fengler-type switching-time-distribution approaches). Device-to-device/domain-to-domain dispersion must be introduced externally (e.g. via the Monte-Carlo parameters or paralleled sub-capacitor instances), not captured intrinsically.

**Polarization charge:**

$$P = 2(p_s+p_{s,mc})\,p \quad [\mathrm{C/m^2}],\ \ P\in[0,\,2p_s]$$

$$i_p = \frac{d}{dt}\big[2(p_s+p_{s,mc})\,p\big]$$

## 4. Depletion / Internal Screening Layer

Effective screening (depletion) width, separately for the two polarization states (asymmetric fixed interface charge → imprint):

$$t_{depl,u} = \max\!\left(10^{-12},\ \left|\frac{\varepsilon_0\varepsilon_{fe}E_{fe} + Q_{fix,u}}{q\,(n_{depl}+n_{depl,mc})}\right|\right)$$

$$t_{depl,d} = \max\!\left(10^{-12},\ \frac{\varepsilon_0\varepsilon_{fe}E_{fe} + Q_{fix,d}}{q\,(n_{depl}+n_{depl,mc})}\right)$$

$$C_{depl,u}=\frac{\varepsilon_0\varepsilon_{depl}}{t_{depl,u}},\quad C_{depl,d}=\frac{\varepsilon_0\varepsilon_{depl}}{t_{depl,d}},\quad
C_{depl,tot} = p\,C_{depl,d} + (1-p)\,C_{depl,u}$$

**Charge-balance (voltage-defined) branch**, sets the depletion-node voltage directly from polarization and FE-cap charge:

$$V_{depletion} = \frac{2(p_s+p_{s,mc})\,p - (p_s - p_{s,mc}) + C_{fe}\,V_{fecap}}{C_{depl,tot}}$$


## 5. Capacitances (linear parts)

$$C_{fe} = \frac{\varepsilon_0\varepsilon_{fe}}{t_{fe}+t_{fe,mc}}, \qquad
C_{de} = \frac{\varepsilon_0\varepsilon_{de}}{t_{fe}+t_{fe,mc}}, \qquad
C_{int} = \frac{\varepsilon_0\varepsilon_{int}}{t_{int}+t_{int,mc}}$$

`C_fe` and `C_de` share the same physical thickness `t_fe` (mixed-phase film of one thickness, split electrically by area fraction `p_ferro`/`1-p_ferro`), so there is no independent monoclinic-phase thickness parameter.

## 6. Leakage / Conduction Currents

### 6.1 Trap-Assisted Tunneling (`itat` macro, applied separately to FE and interface layers)

Multi-phonon/WKB-type trap conduction model: attenuation length from WKB tunneling through a triangular/parabolic barrier,

$$\lambda_{xtm} = \left[2\sqrt{\phi_b\cdot 2 m_{eff}m_0 q/\hbar^2}\cdot 4\pi^2\,\right]^{-1}$$

followed by a field- and temperature-dependent trap-centroid position `x_tm` (closed-form solution of a quadratic in bias `v`), an occupation/barrier-lowering term `w_tr_c`, and a capture time constant

$$\tau_c = \mathrm{clamp}\!\left(10^{-15},\,10^{6},\ \frac{1}{D_{trap}\,n_{tr}\,\exp\!\left(-x_{tm}/\lambda_{xtm} - w_{tr,c}\,k_BT/q\right)}\right)$$

$$i_{tat} = \tanh_{lim}(v,0.1)\cdot \frac{q\,n_{tr}\,x_c}{2\,\tau_c}$$

### 6.2 Fowler-Nordheim tunneling (interface layer only)

Standard FN equation (Sze & Ng):

$$A_{FN} = \frac{q^2}{8\pi\hbar\,\phi_{b,int}}, \qquad
B_{FN} = \frac{8\pi}{3}\frac{\sqrt{2\,m_{eff,int}m_0\,(q\,\phi_{b,int})^3}}{\hbar^2 q^2}$$

$$i_{leak,int} = \tanh_{lim}(v_{int},0.1)\cdot A_{FN}\,E_{int}^2\,\exp\!\left(-\frac{B_{FN}}{|E_{int}|}\right)$$


## 7. Branch Current Assembly (all scaled by instance area)

$$I_{interface} = \big[C_{int}\dot V_{int} + i_{tat,int} + i_{leak,int}\big]\cdot(\mathrm{area}+\mathrm{area}_{mc})$$

$$I_{fecap} = \big[C_{fe}\,p_{ferro}\,\dot V_{fecap} + \dot P\big]\cdot(\mathrm{area}+\mathrm{area}_{mc})$$

$$I_{dielectric} = C_{de}(1-p_{ferro})\,\dot V_{dielectric}\cdot(\mathrm{area}+\mathrm{area}_{mc})$$

$$I_{leakage} = i_{tat,fe}\cdot(\mathrm{area}+\mathrm{area}_{mc})$$

$$V_{depletion} = \text{(charge-balance expression, §4)}$$
