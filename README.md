# LactaSense: membrane model, full derivation

## 1. What we model

LactaSense builds on the microneedle lactate electrode of Bollella et al. [1]: lactate oxidase (LOx) and a poly(methylene blue) mediator on gold–carbon-nanotube microneedles. On its own, this electrode saturates at the enzyme's Michaelis constant ($K_\mathrm{M} = 0.64$ mM), far below the lactate levels seen in septic shock (up to ~20 mM).

Our modification is a thin **polyurethane membrane** over the enzyme that limits diffusion on purpose. The question is how thick and how permeable it must be for the signal to stay linear up to 20 mM, and what that costs in current and response time.

**Assumptions**

- Steady state and one-dimensional transport through the membrane (the membrane, 10 µm thick, is much thinner than the needle).
- Lactate is the only limiting species. The mediator carries the electrons, so oxygen does not limit the reaction.
- Every lactate that reaches the enzyme is converted and gives $n = 2$ electrons to the electrode.
- The concentration in the interstitial fluid (ISF), $C_\mathrm{ISF}$, is fixed: the sensor consumes very little lactate compared with what the tissue holds.
- Partition coefficient $K = 1$, so the permeability equals the diffusion coefficient in the membrane: $P = K D_m = D_m$.

**Parameters**

| Symbol | Meaning | Value | Source |
|---|---|---|---|
| $K_\mathrm{M}$ | apparent Michaelis constant of LOx | 0.64 mM | [1] |
| $j_\mathrm{max}$ | maximum current density | 733 µA $\mathrm{cm^{-2}}$ | [1] |
| $V_\mathrm{max} = j_\mathrm{max}/(nF)$ | maximum conversion rate per area | 3.8$\times 10^{-5}\mathrm{molm^{-2}s^{-1}}$| derived |
| $n$ | electrons per lactate | 2 | |
| $F$ | Faraday constant | 96 485 C \mathrm{mol^{-1}} | |
| $A$ | geometric electrode area (one array) | 0.2 cm$^2$ | [1] |
| $P$ | membrane permeability | $1\times10^{-11}$ m$^2$ s$^{-1}$ | assumed |
| $L$ | membrane thickness | 10 µm | design |

$P$ is about 65 times lower than the diffusion coefficient of lactate in water ($6.5\times 10^{-10}$ m$^2$ s$^{-1}$ [2]). Polyurethane membranes reach values in this range by tuning their hydrophilic content, but the real value must be measured.

Concentrations are in mM, which equals mol m$^{-3}$, so the units below work out directly.

---

## 2. The two processes

**Membrane: Fick's first law.** With a linear concentration profile across a membrane of thickness $L$,

$$J = \frac{P}{L}\left(C_\mathrm{ISF} - C_\mathrm{e}\right)$$

where $J$ is the lactate flux (mol m$^{-2}$ s$^{-1}$) and $C_\mathrm{e}$ is the lactate concentration at the enzyme, on the inner side of the membrane.

**Enzyme: Michaelis–Menten kinetics** (from $\mathrm{E} + \mathrm{S} \rightleftharpoons \mathrm{ES} \rightarrow \mathrm{E} + \mathrm{P}$), written per area:

$$v = \frac{V_\mathrm{max}\,C_\mathrm{e}}{K_\mathrm{M} + C_\mathrm{e}}$$

**Steady state.** Lactate cannot pile up between the membrane and the enzyme, so everything that crosses the membrane is converted:

$$J = v$$

**Faraday's law** turns the converted lactate into the measured current:

$$I = nFA\,J$$

---

## 3. The circuit analogy

![Circuit model](figures/circuit_model.png)

Fick's law has the same form as Ohm's law, $I = (V_1 - V_2)/R$. So lactate transport maps onto a simple circuit:

| Circuit | Sensor |
|---|---|
| voltage | lactate concentration |
| current | lactate flux (mol m⁻² s⁻¹) |
| resistance | transport resistance (s m⁻¹) |
| ideal voltage source | the ISF, which holds $C_\mathrm{ISF}$ fixed |
| ground (0 V) | zero lactate: the enzyme converts all lactate that reaches it |
| node | the enzyme surface, at $C_\mathrm{e}$ |

- **The membrane is a resistor**, $R_\mathrm{m} = L/P$: Fick's law becomes $J = (C_\mathrm{ISF} - C_\mathrm{e})/R_\mathrm{m}$.
- **The enzyme is a resistor to ground.** At low concentration ($C_\mathrm{e} \ll K_\mathrm{M}$), Michaelis–Menten is linear, $v \approx (V_\mathrm{max}/K_\mathrm{M})\,C_\mathrm{e} = C_\mathrm{e}/R_\mathrm{e}$, with $R_\mathrm{e} = K_\mathrm{M}/V_\mathrm{max}$. At higher concentration the enzyme is a *nonlinear* resistor, $R_\mathrm{e}(C_\mathrm{e}) = (K_\mathrm{M} + C_\mathrm{e})/V_\mathrm{max}$, which grows as the enzyme saturates (the arrow in the symbol).
- **Kirchhoff's current law** at the node is the steady-state condition $J = v$.
- **Series resistors** give

$$J = \frac{C_\mathrm{ISF}}{R_\mathrm{m} + R_\mathrm{e}} \qquad\Rightarrow\qquad I = nFA\,J \approx \frac{nFA\,C_\mathrm{ISF}}{L/P + K_\mathrm{M}/V_\mathrm{max}}$$

- **The voltage divider** gives the concentration at the enzyme: $C_\mathrm{e} = C_\mathrm{ISF}\,R_\mathrm{e}/(R_\mathrm{m} + R_\mathrm{e})$.

With our parameters:

$$R_\mathrm{m} = \frac{L}{P} = 1.0 \times 10^{6}\ \mathrm{s\,m^{-1}}, \qquad R_\mathrm{e} = \frac{K_\mathrm{M}}{V_\mathrm{max}} = 1.7 \times 10^{4}\ \mathrm{s\,m^{-1}}, \qquad \frac{R_\mathrm{m}}{R_\mathrm{e}} \approx 59$$

So almost the whole concentration drop happens across the membrane: $C_\mathrm{e} \approx C_\mathrm{ISF}/60$. The membrane, not the enzyme, sets the current. This is the core of the design: a large linear resistor in series with a small nonlinear one gives a nearly linear total.

---

## 4. Sensitivity $S_0$

The **sensitivity** is the slope of the calibration curve at low concentration, in µA per mM:

$$S_0 = \lim_{C_\mathrm{ISF} \to 0} \frac{I}{C_\mathrm{ISF}} = \frac{nFA}{R_\mathrm{m} + R_\mathrm{e}} = \frac{nFA}{L/P + K_\mathrm{M}/V_\mathrm{max}}$$

This is exact, not an approximation: as $C_\mathrm{ISF} \to 0$, also $C_\mathrm{e} \ll K_\mathrm{M}$, so the linearised Michaelis–Menten rate holds exactly. The two cases are limits of the same formula:

| Case | Dominant resistance | $S_0$ | Value |
|---|---|---|---|
| no membrane ($R_\mathrm{m} = 0$) | enzyme | $nFA\,V_\mathrm{max}/K_\mathrm{M}$ | 229 µA/mM |
| 10 µm membrane | membrane | $\approx nFA\,P/L$ | 3.8 µA/mM |

The membrane makes the sensor about 60 times less sensitive. That is the price of linearity, and 3.8 µA per mM is still easy for a potentiostat to measure.

**Units check:** $\dfrac{1 \cdot \mathrm{C\,mol^{-1}} \cdot \mathrm{m^2}}{\mathrm{s\,m^{-1}}} = \mathrm{C\,s^{-1}\,m^3\,mol^{-1}} = \mathrm{A/mM}$.

---

## 5. Exact solution

The circuit result above uses the linearised enzyme. Fig. 3 on the poster uses the exact steady state. Setting $J = v$ with the full Michaelis–Menten rate, and writing $k_\mathrm{m} = P/L$:

$$k_\mathrm{m}\left(C_\mathrm{ISF} - C_\mathrm{e}\right) = \frac{V_\mathrm{max}\,C_\mathrm{e}}{K_\mathrm{M} + C_\mathrm{e}}$$

Multiplying out gives a quadratic in $C_\mathrm{e}$:

$$k_\mathrm{m}\,C_\mathrm{e}^2 + \left(V_\mathrm{max} + k_\mathrm{m}K_\mathrm{M} - k_\mathrm{m}C_\mathrm{ISF}\right)C_\mathrm{e} - k_\mathrm{m}K_\mathrm{M}C_\mathrm{ISF} = 0$$

Only the positive root is physical:

$$C_\mathrm{e} = \frac{-b + \sqrt{b^2 + 4k_\mathrm{m}^2 K_\mathrm{M} C_\mathrm{ISF}}}{2k_\mathrm{m}}, \qquad b = V_\mathrm{max} + k_\mathrm{m}\left(K_\mathrm{M} - C_\mathrm{ISF}\right)$$

and the current is $I = nFA\,k_\mathrm{m}\left(C_\mathrm{ISF} - C_\mathrm{e}\right)$.

---

## 6. Linearity: the "reported lactate" in Fig. 3

A real sensor is calibrated at a low, known concentration and then converts every current into a concentration with that factor:

$$C_\mathrm{reported} = \frac{I}{S_0}$$

If the response is linear, $C_\mathrm{reported} = C_\mathrm{ISF}$. Fig. 3 plots $C_\mathrm{reported}$ against $C_\mathrm{ISF}$.

**Without a membrane**, $C_\mathrm{e} = C_\mathrm{ISF}$ and the current is pure Michaelis–Menten:

$$I = \frac{nFA\,V_\mathrm{max}\,C_\mathrm{ISF}}{K_\mathrm{M} + C_\mathrm{ISF}} \qquad\Rightarrow\qquad C_\mathrm{reported} = \frac{K_\mathrm{M}\,C_\mathrm{ISF}}{K_\mathrm{M} + C_\mathrm{ISF}}$$

This levels off at $K_\mathrm{M} = 0.64$ mM. At 20 mM the reported value is $K_\mathrm{M}/(K_\mathrm{M} + C) = 3$ % of the true one, i.e. **97 % too low**.

**With the 10 µm membrane** (exact solution):

| $C_\mathrm{ISF}$ (mM) | $C_\mathrm{e}$ (mM) | $C_\mathrm{e}/C_\mathrm{ISF}$ | $I$ (µA) | deviation from linear |
|---|---|---|---|---|
| 1 | 0.017 | 1.7 % | 3.8 | 0.04 % |
| 2 | 0.035 | 1.7 % | 7.6 | 0.09 % |
| 4 | 0.074 | 1.8 % | 15.2 | 0.19 % |
| 10 | 0.22 | 2.2 % | 37.7 | 0.6 % |
| 20 | 0.66 | 3.3 % | 74.6 | **1.7 %** |

At 20 mM the enzyme sees $C_\mathrm{e} \approx K_\mathrm{M}$, so it is half saturated. But because $R_\mathrm{e}$ is only a small part of the total resistance, the total current is still within 2 % of linear.

---

## 7. Robustness to enzyme loss

Losing 30 % of the enzyme (e.g. scraped off on insertion) lowers $V_\mathrm{max}$ to 70 %, so $R_\mathrm{e}$ grows by a factor $1/0.7 \approx 1.43$. In units of $R_\mathrm{m}$, the total resistance goes from $1 + 1/59 = 1.017$ to $1 + 1.43/59 = 1.024$. The current falls by **0.7 %** (0.8 % in the exact solution at 2 mM).

Without a membrane, the same loss would cut the signal by 30 %, and the sensor would need recalibrating.

---

## 8. Response time $t_{90}$

$t_{90}$ is the time for the current to complete 90 % of its change after a step in $C_\mathrm{ISF}$. The time scale is set by diffusion across the membrane (Fick's second law, $\partial C/\partial t = D_m\,\partial^2 C/\partial x^2$).

If the enzyme were a perfect sink ($C_\mathrm{e} = 0$), the flux out of a membrane after a step at its outer side is the classic slab solution (Crank):

$$\frac{J(t)}{J_\infty} = 1 + 2\sum_{n=1}^{\infty}(-1)^n \exp\!\left(-\frac{n^2\pi^2 D_m t}{L^2}\right)$$

At 90 % only the $n = 1$ term matters, $2e^{-\pi^2 D_m t/L^2} = 0.1$, so

$$t_{90} \approx \frac{\ln 20}{\pi^2}\,\frac{L^2}{D_m} = 0.30\,\frac{L^2}{D_m}$$

Our finite-difference simulation, with the real enzyme boundary (step from 1 to 5 mM), gives $0.32\,L^2/D_m$, slightly longer because the enzyme is not a perfect sink. For $L = 10$ µm and $D_m = 10^{-11}$ m² s⁻¹, $L^2/D_m = 10$ s, so

$$t_{90} \approx 3\ \mathrm{s}$$

That is negligible compared with the ~5 min lag between lactate in ISF and in blood [3].

---

## 9. Choosing the membrane thickness

For a given material ($P$ fixed):

- **Linearity** needs $R_\mathrm{m} = L/P \gg R_\mathrm{e}$, i.e. a membrane that is thick enough or permeable little enough.
- **Current** scales as $S_0 \approx nFA\,P/L \propto 1/L$.
- **Response time** scales as $t_{90} \propto L^2/D_m$.

So the best membrane is the *thinnest* one that is still linear. Required linearity fixes the ratio $L/P$. Within that, a less permeable material allows a thinner membrane, which is faster. That is why we chose a low-permeability polyurethane rather than a hydrogel: a hydrogel lets lactate through almost as fast as water, so to be linear it would have to be about 230 µm thick (for < 5 % deviation at 20 mM), with a response time of almost a minute instead of a few seconds.

---

## 10. Link to the Cottrell equation (Module 2)

Without a membrane, a step in electrode potential gives the Cottrell current

$$i(t) = nFAC\sqrt{\frac{D}{\pi t}}$$

which decays because the depleted layer next to the electrode grows as $\delta = \sqrt{\pi D t}$. The membrane fixes the diffusion distance at $L$ instead of letting it grow. The current then reaches a steady value, $i_\mathrm{ss} = nFA\,(P/L)\,C$, which is the same form as the steady-state current $i_\mathrm{ss} = nFA\,k_\mathrm{eff}\,C$ from Module 2, with $k_\mathrm{eff} = P/L$.

---

## 11. Limitations

- $P$ is assumed, not measured. The design target is the ratio $L/P$; the real membrane must be characterised.
- Steady-state, one-dimensional model: the pyramid needle geometry, biofouling and drift are not included.
- Partition coefficient $K = 1$ is assumed.
- Temperature dependence of $V_\mathrm{max}$ and $P$ is not modelled (the patch measures skin temperature to correct for it).

---

## References

1. P. Bollella, S. Sharma, A. E. G. Cass, R. Antiochia, "Microneedle-based biosensor for minimally-invasive lactate detection", *Biosens. Bioelectron.* **123** (2019) 152–159.
2. K. A. Saleh et al., "A multiphysics engineering model of pH and oxygen transport limitations in textile-based wearable lactate biosensors", *Eng. Rep.* **8** (2026) e70874. doi:10.1002/eng2.70874
3. [TODO: Ming et al. 2022, *BMJ Innovations* 8(2) 87–94, doi:10.1136/bmjinnov-2021-000864. Check the ISF–blood lag value before publishing.]
4. J. Crank, *The Mathematics of Diffusion*, 2nd ed., Oxford University Press (1975).

*Prepared with help from Claude (Anthropic); the authors reviewed the content and take responsibility for it.*
