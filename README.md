# Salmonella Braenderup and melons, Germany 2021

A worked example for [Fieldwork](https://github.com/digital-twin-ph/fieldwork): a case-control study with two
control groups, built from an empty canvas.

In spring 2021, 348 cases of *Salmonella* Braenderup (sequence type 22) were reported in 12 European countries
and the UK, more than 80 in Germany. The German case-control study identified Galia melons as the vehicle, and
the outbreak strain was found in Galia melons from Honduras (Łuczyńska et al. 2023). The study compared two
ways of recruiting controls: by telephone (classical controls) and from an online panel in Lower Saxony.

## The questions

| Workflow | Nodes | What it shows |
| --- | --- | --- |
| Which melon, against classical controls? | 4 | Six melon items, the week after Easter, 32 cases against 110 classical controls |
| Which melon, against matched panel controls? | 7 | The same against the 81 panel controls matched on sex and age group |
| Does the week asked about change the answer? | 4 | The first comparison, over the week before each control's interview |

Each declares a case-control design, so only odds ratios are reported. A case-control study cannot give attack
rates.

The second workflow keeps the 32 cases from the complete file and appends the 81 matched controls from the
second file. The appended table names the source of each row and numbers the rows as a new key, because the
110 classical controls all have the `id` `NA`.

## What the app computes, against what the paper reports

The app computes crude odds ratios with 95% Wald intervals. The paper's are adjusted for age and sex by logistic
regression (Table 4), so they are not expected to be equal.

| Galia melon | Cases exposed / not | Controls exposed / not | Crude, this example | Adjusted, paper |
| --- | --- | --- | --- | --- |
| April, classical controls | 12 / 20 | 1 / 109 | 65.4 (8.05 to 531) | 55 (8.2 to 1100) |
| April, matched panel controls | 12 / 20 | 3 / 78 | 15.6 (4.02 to 60.6) | 12 (2.7 to 66) |
| July, classical controls | 12 / 20 | 3 / 107 | 21.4 (5.54 to 82.7) | 32 (7.3 to 190) |

The April control counts for all five melon items match the paper's Table 3.

## Files

| File | What it holds |
| --- | --- |
| `data/Complete_dataset_Luczynska_et_al_2022.csv` | 298 people: 32 cases, 110 classical and 156 panel controls |
| `data/FM_HuGo_controls_Luczynska_et_al_2022.csv` | The 81 panel controls selected to match the cases on sex and age group |
| `data/Codebook_Luczynska_et_al_2022.xlsx` | The variables, in German and English |
| `fieldwork-project.json.gz` | The Fieldwork project |

The three data files are as published, with Zenodo's checksums.

## Reading the data

- **`fall` is the group:** 1 case, 0 classical control, 2 panel control. **In the matched file, the panel
  controls are coded `fall` 0.**
- **Every classical control has the `id` `NA`.** `id` identifies cases and panel controls only. On import, rows
  repeating the key columns are reported in Data quality, not refused.
- **Food columns ending `2` are collapsed:** 1 for "yes" or "rather yes", 0 for "no" or "rather no".
- **Columns ending `_apr2` ask about the week after Easter Sunday (5 to 11 April 2021); those without ask about the
  7 days before the interview.** Cases were asked once, about the week before they fell ill, so their answer is the
  same in both.

## What this does not establish

- **No adjustment.** Age and sex are not adjusted for; the paper's estimates are.
- **No causes beyond the paper's.** The microbiological finding, not these odds ratios, identified the vehicle.
- **Many comparisons, no correction.** Six items per workflow, each with its own interval.
- **Not the paper's selection of the 81.** They are taken as published, not redrawn.

## References

- Łuczyńska A, Dreesman J, Mertens E, Wollenweber M, Perriat D, Rosner B. Recruiting controls from an online panel
  for a case–control study enabled a timely and reliable foodborne *Salmonella* outbreak investigation, Germany
  2021. *Epidemiology and Infection*. 2023;151:e70. [doi:10.1017/S0950268823000493](https://doi.org/10.1017/S0950268823000493)
- Łuczyńska A, Dreesman J, Mertens E, Wollenweber M, Perriat D, Rosner B. Dataset for the same, version 1. Zenodo;
  2022. [doi:10.5281/zenodo.7333295](https://doi.org/10.5281/zenodo.7333295). CC BY 4.0.
