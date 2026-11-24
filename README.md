# The hidden decline: end-of-life and performance loss in Pakistan's consumer-led solar fleet

Code and data for the manuscript:

> [Author 1], [Author 2], and [Author 3]. *The hidden decline: end-of-life and performance loss in Pakistan's consumer-led solar fleet.* Submitted to *Joule* (2026).

This repository contains a cohort-based model of Pakistan's distributed solar photovoltaic (PV) fleet. The fleet totals an estimated 51 GW installed by March 2026. The model estimates:

1. when the installed modules will reach end of life, and how much mass they represent;
2. the material content of the fleet (glass, polymers, aluminium, silicon, copper, silver);
3. how much of the fleet's capacity and generation is lost to module failure and degradation;
4. how much new capacity is needed each year just to keep output constant.

Only modules installed by March 2026 are modelled. All results therefore describe the fleet that already exists, not a forecast of future installations.

## Repository structure

```
.
├── model/
│   ├── pv_eol_model.py      # the simulation: inputs, failure model, all calculations
│   └── make_figures.py      # draws Figures 1–4 from the output tables
├── outputs/                 # results written by pv_eol_model.py
│   ├── table_cohorts.csv               # capacity installed per fiscal year (Figure 1)
│   ├── table_eol_scenarios.csv         # cumulative end-of-life GW and kt (Table 2)
│   ├── series_eol_annual.csv           # annual and cumulative end-of-life, 2026–2060 (Figure 2)
│   ├── table_effective_capacity.csv    # effective capacity and lost generation (Figure 3, Table 4)
│   ├── table_replacement_need.csv      # replacement capacity per year (Table 4)
│   ├── table_sensitivity_2040.csv      # one-at-a-time sensitivity (Figure 4)
│   ├── summary.json                    # key numbers (fleet mass, materials, half-fleet years)
│   └── run_log.txt                     # console output of the model run
├── figures/                 # Figures 1–4 as 600-dpi PNG and vector PDF
├── requirements.txt
├── CITATION.cff
└── LICENSE
```

## How to run

Requires Python 3.10 or later.

```bash
pip install -r requirements.txt
cd model
python3 pv_eol_model.py     # writes ../outputs/
python3 make_figures.py     # writes ../figures/
```

The model is deterministic: it has no random sampling, so every run gives the same numbers. A full run takes a few seconds.

The results in the manuscript were produced with Python 3.13, NumPy 2.5.3, pandas 3.0.5 and Matplotlib 3.11.2.

## Method in brief

**Installation cohorts.** Nine cohorts are reconstructed from published stock estimates: 7.7 GW (end FY2022), 11.0 GW (end FY2023), 38.0 GW (end FY2025) and 51.0 GW (March 2026). Pakistan's fiscal year runs from July to June. Additions in FY2024–FY2025 are split in proportion to fiscal-year module imports (16.0 GW and 17.9 GW). The pre-FY2023 stock is spread evenly over FY2018–FY2022.

**Failure model.** The cumulative probability that a module has reached end of life at age *t* follows the Weibull function of IRENA and IEA-PVPS (2016):

F(t) = 1 − exp[−(t / T)^α]

| Scenario | Shape factor α | Lifetime T (years) |
|---|---|---|
| Regular loss | 5.3759 | 30 |
| Early loss | 2.4928 | 30 |
| Early loss | 2.4928 | 25 |
| Early loss (stress) | 2.4928 | 20 |

**Degradation.** Surviving modules lose output at 0.50% per year (global median), 0.88% per year (hot-climate fleet median) or 1.50% per year (stress assumption).

**Mass and materials.** The specific power is 19.1 W kg⁻¹ (52.4 t MW⁻¹). Composition by mass is 76% glass, 10% polymers, 8% aluminium, 5% silicon and 1% copper. Silver is about 10.3 mg W⁻¹.

**Generation.** The capacity factor is 16.71%, i.e. 1,464 kWh kW⁻¹ y⁻¹.

All input values are defined at the top of `model/pv_eol_model.py`. Full sources are in Table 1 of the manuscript.

## Data sources

All inputs come from public sources. No new data were collected.

| Input | Source |
|---|---|
| Deployment, imports, grid sales, distributed generation | Renewables First, *Pakistan Electricity Review 2026*; Ember and Renewables First, *The solarisation of Pakistan's energy economy* (2026); Renewables First, *The Many Dividends of Solar Rush in Pakistan* (2025) |
| Market quality, warranties, standards | Competition Commission of Pakistan, *Unlocking Green Potential: A Market Competition Study of Solar Energy in Pakistan* (2026) |
| Weibull failure functions, module composition | IRENA and IEA-PVPS (2016), *End-of-Life Management: Solar Photovoltaic Panels*, Report T12-06:2016 |
| Degradation rates | Jordan & Kurtz (2013), Prog. Photovolt. 21, 12–29; Jordan et al. (2016), Prog. Photovolt. 24, 978–989; Jordan et al. (2022), Prog. Photovolt. 30, 1166–1175 |
| Module specific power | Bolson & Iqbal (2026), Mater. Res. Express 13, 055501 |
| Silver intensity | VDMA (2026), ITRPV 17th edition |

## Limitations

- Failure and degradation parameters come from the international literature. Field measurements of Pakistan's distributed fleet do not yet exist.
- Deployment figures are non-official estimates. The June 2025 range is 27.5–38 GW.
- Informal repair, reuse and resale are not modelled. They would delay waste but not reduce its total.
- A single national capacity factor is used. Curtailment, soiling and orientation are not modelled.

## Citation

If you use this code, please cite the manuscript (details will be updated on publication) and this repository (see `CITATION.cff`).

## License

The code is released under the MIT License (see `LICENSE`). The input data belong to their original publishers; please cite them as listed above.

## Contact

[Name], [Institution], [email]
