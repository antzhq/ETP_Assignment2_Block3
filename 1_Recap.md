# Hybrid CSP + PVT + Wind + Biomass Plant for Santiago: Recap

**Assignment 2, Q6–Q10.** Renewable Energy Conversion, Energy Transition Perspectives minor. This extends the Assignment 1 model (`plant_sizing.py`); the CSP and PVT physics are unchanged.

---

## 1. Bottom line

| | Assignment 1 | Assignment 2 | Change |
|---|---|---|---|
| **Wind turbines (Q6)** | – | **12** | new |
| **Heliostats (Q8)** | 215 | **562** | +347 |
| **PVT panels (Q8)** | 9,559 | **9,549** | −10 |
| **Ethanol burned (Q7)** | – | **10,963 kg (June) · 10,190 kg (December)** per day | new |
| Electricity demand met | 48% (Jun) · 57% (Dec) | **100% on both days, every hour** | |
| Surplus electricity | 148 / 214 GJ | 548 / 841 GJ (1.3× / 1.6× demand) | |
| CSP share of all electricity produced | 63% / 63% | **60.0% (June, binding) / 62.2% (December)** | |

- **Every hour of both days is covered.** Ethanol fills what sun and wind cannot, so unmet demand is zero.
- **Objective.** A1 minimised cost. A2 has no prices, so it minimises **unmet demand first and surplus second**.
- **60% rule:** CSP must *produce* at least 60% of **all** electricity produced that day (CSP + PVT + wind + ethanol), surplus included.
- **What sizes each technology:**
  - heat demand sets the **PVT panels**;
  - "preference to wind" (wind ≥ ethanol each day) sets the **turbines**;
  - the 60% rule sets the **heliostats**. CSP must produce 1.5 GJ for every GJ the other sources produce, because 60/40 = 1.5.

---

## 2. How the numbers lock into each other (June; December in brackets)

| Step | Set by | Result |
|---|---|---|
| 1. PVT panels | Heat demand. PVT is the only heat source. | 9,549 panels → **134 GJ** of electricity (195), all during sunlit hours |
| 2. The gap | Night and evening hours that sun cannot serve | Filled by wind and ethanol |
| 3. Wind vs ethanol | Wind ≥ ethanol each day | 12 turbines: wind delivers **130 GJ**, ethanol **123 GJ** (206 vs 115) |
| 4. Heliostats | CSP ≥ 1.5 × (PVT + wind + ethanol produced) | 1.5 × (134 + 137 + 123) = **592 GJ** of CSP → 562 heliostats |

- **The 60% rule makes every other source "cost" mirrors.**
  - Each GJ of PVT, wind or ethanol produced requires 1.5 GJ of CSP production.
  - CSP only produces in the sunlit hours, when PVT and wind already cover much of demand. Most of that CSP output is surplus: CSP produces 592 GJ but only 177 GJ is used (June).
- **This is why the plant uses few turbines.** One turbine adds its own surplus *and* forces about 10 more heliostats (from 0 to 12 turbines, heliostats rise from 441 to 562). So the model uses the smallest wind fleet that still beats ethanol.
- **Ethanol is now the largest source after wind.** It produces only what it delivers, but it still counts in the 60% total.

---

## 3. Answers

### Q6: number of wind turbines = **12**
"Preference to wind" is applied as the brief words it: when deciding the *share* of wind and biomass, wind must deliver **at least as much electricity as ethanol on each day**. The model picks the fewest turbines that satisfy this. Every extra turbine adds surplus, both directly and through the extra mirrors the 60% rule then needs.

- **June is binding:** wind delivers 130.0 GJ against 123.3 GJ of ethanol.
- **December clears it easily:** 205.6 vs 114.6 GJ.

### Q7: rate of ethanol combustion
| | June | December |
|---|---|---|
| Hours burning | 19:00–03:59 and 06:00 (10 h) | 19:00–04:59 (10 h) |
| Electricity from ethanol | 123.3 GJ (28.1% of demand) | 114.6 GJ (21.1%) |
| Ethanol per day | **10,963 kg** | **10,190 kg** |
| Average rate while burning | 1,096 kg/h = **0.305 kg/s** | 1,019 kg/h = **0.283 kg/s** |
| Peak rate | 3,199 kg/h = 0.889 kg/s (19:00) | 2,855 kg/h = 0.793 kg/s (21:00) |

- **Ethanol per GJ of electricity:** $1000 / (\eta_{\text{Rankine}} \cdot \text{LHV}) = 1000 / (0.42 \times 26.77) = 89$ kg/GJ.
- **The shared Rankine cycle needs no extra capacity:**
  - ethanol firing peaks at 36.0 GJ/h (June) and 32.1 GJ/h (December);
  - the turbine already handles the solar peak of 72.9 and 99.4 GJ/h.

### Q8: change in heliostats and PVT panels
- **Heliostats: 215 → 562.**
  - CSP must now produce 60% of all electricity in the day, including the night-time wind and ethanol and the surplus.
  - Mirrors only work in daylight, so CSP must produce 1.5× everything the other sources produce, within the sunlit hours.
- **PVT panels: 9,559 → 9,549.**
  - June heat still sets the count, and 9,549 is the exact minimum.
  - In A1 the cost optimiser added 10 panels (+€6,000) because that saved one heliostat (−€17,600).
  - In A2 there are no prices, so that trade no longer exists. Fewer panels also means less PVT electricity, which means fewer mirrors under the 60% rule.

### Q9: hourly output vs demand, and comparison with Assignment 1
`Q9_Hourly_Supply_vs_Demand.png` · `Q9_Comparison_with_Assignment_1.png`

| Whole day (24 h) | A1, June | **A2, June** | A1, December | **A2, December** |
|---|---|---|---|---|
| Demand covered | 212 GJ (48%) | **438 GJ (100%)** | 310 GJ (57%) | **542 GJ (100%)** |
| Unmet electricity | 226 GJ | **0** | 232 GJ | **0** |
| Surplus electricity | 148 GJ | 548 GJ | 214 GJ | 841 GJ |
| of which CSP / PVT / wind | 54 / 94 / – | 415 / 127 / 7 | 82 / 133 / – | 651 / 182 / 8 |
| Covered ÷ surplus | 1.43 | 0.80 | 1.45 | 0.64 |
| Demand met by CSP / wind / ethanol / PVT | | 40 / 30 / 28 / 2% | | 39 / 38 / 21 / 2% |
| Heat | unchanged: covered in total over the sunlit hours; midday surplus, none in the evening or at night | | | |

**Both days together:** A1 covers 53% of demand with 363 GJ of surplus. A2 covers 100% with 1,390 GJ of surplus. The extra 458 GJ covered costs 1,027 GJ of extra surplus.

- **A1** covered demand only around midday. Nights, mornings and the 19:00 peak were short.
- **A2** covers every hour:
  - CSP and wind: the day;
  - wind: the windy early-morning hours;
  - ethanol: the calm night and the evening peak.
- **Almost all of the surplus is CSP (75%) and PVT (23%).**
  - CSP's surplus is forced by the 60% rule.
  - PVT's surplus is forced by heat demand.
  - Wind is almost fully used (7 GJ curtailed), because the model only builds turbines whose output can be used.

### Q10: total electricity output vs turbine efficiency (75–95% of Betz)
`Q10_Wind_Efficiency_Sweep.png` (stacked bars: total produced, by source) · `Q10_Wind_vs_Ethanol.png` (lines: wind vs ethanol produced)

- **(a) Same plant (12 turbines).** Wind output is proportional to $C_p$. Ethanol fills whatever wind does not, so the total rises only gently:
  - June: from 973 to 1,000 GJ/day;
  - December: from 1,361 to 1,405 GJ/day;
  - wind output itself: +27% from 75% to 95% (June 121 → 153 GJ; December 188 → 238 GJ);
  - ethanol falls slightly (June 126 → 120 GJ; December 118 → 112 GJ). In June, wind *produced* overtakes ethanol at about 78% of Betz; December wind is above ethanol at every efficiency.
  - **The fixed plant drifts against the 60% rule.** Above 85%, wind's extra output pushes June's CSP share just below 60% (59.2% at 95%; 60.8% at 75%). Measured on electricity *delivered* (how the wind rule is defined), June wind falls under ethanol below about 81% (115 vs 126 GJ at 75%).
- **(b) Plant re-sized.** The turbines needed fall from **13 at 75%** to **11 at 95%**. Heliostats rise slightly, from 558 to 566, because more efficient turbines produce more electricity that CSP must match at 1.5×.

---

## 4. Where the surplus comes from, and the wind–ethanol trade-off

| Surplus (GJ/day) | June | December |
|---|---|---|
| CSP mirrors | 415 | 651 |
| PVT electricity | 127 | 182 |
| Wind | 7 | 8 |
| **Total** | **548** | **841** |

**Trade-off** (workbook sheet *Wind-ethanol trade-off*; not used as a report figure). The turbine count is fixed and everything else re-optimised. Totals are June + December:

| Turbines | Heliostats | Surplus | Ethanol-fired electricity | Surplus added per GJ of ethanol avoided |
|---|---|---|---|---|
| 0 | 441 | 827 GJ | 338 GJ | – |
| 10 | 537 | 1,278 GJ | 249 GJ | 5 |
| **12 (design)** | **562** | **1,390 GJ** | **238 GJ** | 10 |
| 20 | 668 | 1,863 GJ | 204 GJ | 14 |
| 30 | 802 | 2,471 GJ | 174 GJ | 20 |
| 40 | 972 | 3,179 GJ | 150 GJ | 30 |
| 80 | 1,661 | 6,089 GJ | 112 GJ | 77 |

- **Minimum surplus and preference to wind conflict.**
  - Zero turbines gives the least surplus but ignores the brief.
  - Each turbine also drags 10–15 heliostats with it, so surplus climbs steeply.
- **The design takes the smallest wind fleet that still gives wind the larger share:** 12 turbines.
- **This surplus is unavoidable without storage.** With no storage in the brief, energy produced in the wrong hour cannot be moved to the right one. Thermal storage on the CSP plant (Lecture 2) would be the natural improvement, as the next step beyond this assignment.

---

## 5. Assumptions

| Assumption | Why |
|---|---|
| **60% rule:** CSP *produces* ≥ 60% of **all** electricity produced in the day (CSP + PVT + wind + ethanol), surplus included | Group's reading of the brief. Ethanol produces only what it delivers. |
| **Preference to wind:** wind delivers ≥ ethanol on each day | The brief speaks of the *share* of wind and biomass. No prices are given, so the preference is a rule. |
| **Objective:** no prices; minimise unmet demand, then surplus | Group's choice. |
| **Dispatch order where several renewables could serve:** wind, then CSP, then PVT | It does not change the total surplus, only which source is curtailed. PVT is there for heat. |
| Electricity met **hour by hour**, 24 h | Possible now that ethanol is dispatchable. |
| Heat met over the sunlit hours (as in A1) | PVT is the only heat source; the TA ruled out condenser heat. |
| Air density 1.225 kg/m³; wind data km/h → m/s | Sea-level value from Lecture 6; air density is not given in the brief. |
| Ethanol LHV 26.77 MJ/kg; all combustion heat goes to the steam | Consistent with Q5; no boiler efficiency is given. Q5 uses 26.75 MJ/kg (water ratio rounded to 1.18); the difference is 0.07%. |
| 450,000 m² limit applies to collectors; turbines stand outside the solar site | 12 turbines need roughly 2–3 km² of spacing. |
| No storage | Not part of the brief. |
| Q10(a): fixed plant dispatched without re-balancing the 60% and wind rules | With the units fixed, the rules cannot be re-balanced. The sheet reports the resulting CSP share. |

---

## 6. Folder contents

| Folder | Contents |
|---|---|
| `2_Math_Model` | Full formulation (PDF and LaTeX source) |
| `3_Code` | `plant_sizing_A2.py`: run it to reproduce every number, figure and the workbook (`pip install numpy scipy matplotlib openpyxl`) |
| `4_Visualizations` | Q9 hourly supply vs demand (electricity and heat, used vs surplus) · Q9 comparison with A1 · Q10 total output vs efficiency (stacked bars) · Q10 wind vs ethanol produced vs efficiency (lines) |
| `5_Data` | Input data · `Results_Hybrid_Plant_A2.xlsx` (design, hourly dispatch for both days, Q10 sweep, wind–ethanol trade-off) |
