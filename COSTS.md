# LactaSense: parts and prices

Supplementary material for the cost-per-patient box on the poster:

$$\text{Cost per patient} = 2\,\left(C_\mathrm{sensor} + C_\mathrm{electronics}\right)$$

Each patch is single use and is replaced every 24-48 h, so a 3-day stay uses two. $C_\mathrm{sensor}$ covers the microneedle chip, sensing layers, membrane, thermistor and piezoelectric film; $C_\mathrm{electronics}$ covers the potentiostat, Bluetooth chip and battery. Reusing the electronics would lower the cost but has not been worked out yet.

Prices are for a batch of 10 units, excluding VAT and shipping, checked 25 Nov 2026. Dollar prices are converted at 1 € = 1.095 $.

## Sensor part, $C_\mathrm{sensor}$

| Part | Product | Price per unit | Batch of 10 | Source |
|---|---|---|---|---|
| Thermistor | Wavelength Electronics TCS650, 50 kΩ | $9.90 ≈ €9.04 | $99.00 ≈ €90.41 (one 10-pack) | [1] |
| Piezoelectric film | TE Connectivity LDT0-028K (proxy, see note) | €4.13 | €41.30 | [2] |
| Microneedle chip, sensing layers, membrane | own fabrication (estimate, see note) | ~€1 | ~€10 | [6] |
| **Total** | | **≈ €14.17** | **≈ €141.71** | |

**Note on the piezoelectric film:** the AlN film we propose [3] is made in a research cleanroom and has no catalogue price. We use a commercial PVDF piezo film sensor as a proxy. The AlN film is made by standard microfabrication and is expected to cost less at volume.

**Note on the microneedle chip:** the chip is our own design, so there is no catalogue price. As an order-of-magnitude anchor, PATH estimates the manufacturing cost of a measles–rubella vaccine microneedle patch at about $0.70–1.11 per patch at 20 million patches a year [6]. Our chip also needs gold, carbon nanotubes, the mediator, lactate oxidase and the polyurethane membrane, and would be made in much smaller numbers at first, so ~€1 is a lower bound. The real cost must be estimated once the fabrication process is fixed.

## Electronics, $C_\mathrm{electronics}$

| Part | Product | Price per unit | Batch of 10 | Source |
|---|---|---|---|---|
| Potentiostat | Texas Instruments LMP91000SDX/NOPB | €3.58 | €35.80 | [4] |
| Battery | CR1025 3 V coin cell (Jauch) | €0.79 | €7.86 | [5] |
| Bluetooth chip | Nordic Semiconductor nRF52810 (Bluetooth Low Energy) | €2.04 | €20.40 | [7] |
| **Total** | | **€6.41** | **€64.06** | |

## Price at larger volumes

| Part | Quantity | Price per unit | Source |
|---|---|---|---|
| Thermistor | 6–14 packs of 10 | $9.20 ≈ €8.40 | [1] |
| Thermistor | 15+ packs | on request from the manufacturer | [1] |
| Piezoelectric film | 100+ / 500+ | €3.36 / €3.10 | [2] |
| Potentiostat | 100 / 1,000 | €2.99 / €2.73 | [4] |
| Potentiostat | 2,500 | €2.64 (€6,600 in total) | [4] |
| Potentiostat | full reel of 4,500 | €2.63 (€11,835 in total) | [4] |
| Battery | 100 / 1,000 | €0.66 / €0.54 | [5] |
| Bluetooth chip | 100 / 1,000 | €1.77 / €1.61 | [7] |

## Cost per patient

$$2 \times \left(€14.17 + €6.41\right) = 2 \times €20.58 \approx €41$$

That is about 0.2 % of the ~€21,000 average cost of a sepsis case. The Bluetooth chip also needs an antenna, a crystal and a few passive components, which are not included.

The microneedle chip is the most uncertain cost. Even if it cost €50 instead of €1, the total would be about €139 per patient, still under 0.7 % of the cost of a sepsis case.

## Sources

1. Wavelength Electronics, *TCS650 50 kΩ thermistor (2×3 mm), sold in 10-packs*, product page. https://www.teamwavelength.com/?p=1637 (accessed 25 Nov 2026).
2. Farnell Belgium, *TE Connectivity LDT0-028K piezoelectric sensor*, order code 2771930. https://be.farnell.com/sensor-solutions-te-connectivity/ldt0-028k/piezoelectric-sensor-voltage/dp/2771930 (accessed 25 Nov 2026).
3. M. Cinquino et al., "A novel approach for non-invasive, simultaneous, and realtime monitoring of BCG, BPW, and heart sounds with flexible and bio-compatible piezoelectric AlN sensor", *Biosensors and Bioelectronics: X* **27** (2025) 100693.
4. Mouser Electronics Europe, *Texas Instruments LMP91000SDX/NOPB*. https://eu.mouser.com/en/ProductDetail/Texas-Instruments/LMP91000SDX-NOPB?qs=7lkVKPoqpbYmeZ1SbPtg0w%3D%3D (accessed 25 Nov 2026).
5. Digi-Key Netherlands, *Jauch Quartz CR1025 coin cell*. https://www.digikey.nl/en/products/detail/jauch-quartz/CR1025/19558375 (accessed 25 Nov 2026).
6. PATH, *Measles-Rubella Microarray Patch Vaccines: A Business Case Analysis*, March 2021, p. 7. https://media.path.org/documents/PATH_MR_MAP_business_case_report_2021March.pdf (accessed 25 Nov 2026).
7. Farnell France, *Nordic Semiconductor nRF52810-QFAA-R7 Bluetooth SoC*, order code 2842314. https://fr.farnell.com/en-FR/nordic-semiconductor/nrf52810-qfaa-r7/bluetooth-soc-2mbps-2-5ghz-qfn/dp/2842314 (accessed 25 Nov 2026).
