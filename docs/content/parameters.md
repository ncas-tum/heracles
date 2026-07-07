# Parameters

## 8.1 Geometry / phase composition
| Parameter | Description | Unit | Default | Range |
|---|---|---|---|---|
| `area` | Device area (instance) | m² | 1e-12 | [1e-18, ∞) |
| `p_ferro` | Ferroelectric (orthorhombic) area fraction | 1 | 0.6 | [0, 1] |

## 8.2 Material stack
| Parameter | Description | Unit | Default | Range |
|---|---|---|---|---|
| `t_fe` | Ferroelectric film thickness | m | 9.8e-9 | [1e-9, 5e-8] |
| `t_int` | Interface (dead) layer thickness | m | 2e-9 | [1e-10, 2e-8] |
| `eps_fe` | Rel. permittivity, orthorhombic HZO | 1 | 70 | [1, 100] |
| `eps_de` | Rel. permittivity, monoclinic HZO | 1 | 20 | [1, 100] |
| `eps_int` | Rel. permittivity, interface layer | 1 | 70 | [1, 500] |
| `eps_depl` | Rel. permittivity, electrode-side depletion region | 1 | 2.2 | (0, 100] |

## 8.3 Polarization dynamics
| Parameter | Description | Unit | Default | Range |
|---|---|---|---|---|
| `w_b` | Switching energy barrier height | eV | 1.05 | (0, ∞) |
| `d_e` | Ion/jump distance (see §3 unit note) | m (declared m²) | 7.5e-9 | (0, ∞) |
| `e_off` | Offset (built-in) electric field | V/m | 0 | [-1e9, 1e9] |
| `p_s` | Saturation polarization density | C/m² | 0.27 | [1e-3, 1] |

## 8.4 Depletion / screening
| Parameter | Description | Unit | Default | Range |
|---|---|---|---|---|
| `n_depl` | Carrier density, interface depletion region | m⁻³ | 1.4e28 | [1e25, 1e30] |
| `q_fix_depl_u` | Fixed interface charge, "up" state | (norm.) | -0.08 | [-1, 1] |
| `q_fix_depl_d` | Fixed interface charge, "down" state | (norm.) | 0.20 | [-1, 1] |

## 8.5 TAT — ferroelectric layer
| Parameter | Description | Unit | Default | Range |
|---|---|---|---|---|
| `m_eff_fe` | Electron effective mass (×m₀) | 1 | 0.1 | [0.01, ∞) |
| `phi_b_fe` | Electrode/HfO₂ barrier height | eV | 2.0 | (0, 5] |
| `n_tr_fe` | Trap density | m⁻³ | 1e25 | [1e18, 1e28] |
| `w_tr_t_fe` | Trap depth | eV | 2.1 | [0, 5] |
| `w_tr_rel_fe` | Trap relaxation energy | eV | 1.19 | [0, 5] |
| `dos_trap_fe` | Trap DOS | m | 1e-8 | [1e-10, 1e40] |
| `x_c_fe` | TAT characteristic hop length | m | 5e-9 | [1e-10, 1e-7] |

## 8.6 TAT / FN — interface layer
| Parameter | Description | Unit | Default | Range |
|---|---|---|---|---|
| `m_eff_int` | Electron effective mass (×m₀) | 1 | 0.4 | [0.01, ∞) |
| `phi_b_int` | Electrode/interface barrier height | eV | 3.1 | (0, 5] |
| `n_tr_int` | Trap density | m⁻³ | 1e23 | [1e18, 1e28] |
| `w_tr_t_int` | Trap depth | eV | 3.0 | [0, 5] |
| `w_tr_rel_int` | Trap relaxation energy | eV | 0.4 | [0, 5] |
| `dos_trap_int` | Trap DOS | m | 1e-8 | [1e-10, 1e40] |
| `x_c_int` | TAT characteristic hop length | m | 1e-9 | [1e-10, 1e-7] |

## 8.7 Monte-Carlo (inherited)
| Parameter | Perturbs | Default |
|---|---|---|
| `area_mc` | `area` | 0 |
| `t_fe_mc` | `t_fe` | 0 |
| `t_int_mc` | `t_int` | 0 |
| `p_s_mc` | `p_s` (also used deterministically, see §4) | 0 |
| `n_depl_mc` | `n_depl` | 0 |
