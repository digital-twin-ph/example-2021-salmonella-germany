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
| Adjusted for age and sex, is Galia melon still the answer? | 4 | Logistic regression of case status on Galia melon in April, adjusted for age group (7 groups) and sex, with likelihood-ratio intervals: 96.4 (15.4–2067), the paper's 96 (15–2100) |
| Who were the cases? | 6 | The 32 cases beside the 110 classical controls by age group and by sex, each group's share within cases and within controls |
| Which foods did cases eat more often than classical controls? | 4 | All 34 fruit items of the questionnaire in one two-by-two, the week before illness or interview, in the questionnaire's order |
| Where did the cases shop? | 6 | How often the cases shop at an organic shop and at a market, counted for the cases alone |
| Where did the cases live? | 10 | The cases by federal state, mapped over Germany's 16 Länder, a Land with no case in the study shown as 0 |

Each declares a case-control design, so only odds ratios are reported. A case-control study cannot give attack
rates.

The second workflow keeps the 32 cases from the complete file and appends the 81 matched controls from the
second file. The appended table names the source of each row and numbers the rows as a new key, because the
110 classical controls all have the `id` `NA`.

## What the app computes, against what the paper reports

The two-by-two workflows compute crude odds ratios with 95% Wald intervals; the paper's are adjusted for age and sex by
logistic regression, so those are not expected to be equal. The fourth workflow fits that regression (Fieldwork's
Regression widget, experiment 167) and reproduces the paper's single-food adjusted ratio for Galia melon against
classical controls, 96 (15–2100), as 96.4 (15.4–2067), with the paper's age grouping of seven groups and
likelihood-ratio intervals. Against panel controls the same model gives 18.4 (4.6–101), the paper's 18 (4.6–100).

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
| `data/laender.csv` | The 16 federal states: the code `bula` uses, the ISO 3166-2 code and the name. Written for this example |
| `geo/germany-laender.geojson` | Germany's 16 Länder, 2021, from the Federal Agency for Cartography and Geodesy as geoBoundaries publishes it |
| `fieldwork-project.json.gz` | The Fieldwork project |

The three files from Zenodo are as published, with Zenodo's checksums. The boundary file is as geoBoundaries publishes it.

## Reading the data

- **`fall` is the group:** 1 case, 0 classical control, 2 panel control. **In the matched file, the panel
  controls are coded `fall` 0.**
- **Every classical control has the `id` `NA`.** `id` identifies cases and panel controls only. On import, rows
  repeating the key columns are reported in Data quality, not refused.
- **Food columns ending `2` are collapsed:** 1 for "yes" or "rather yes", 0 for "no" or "rather no".
- **Columns ending `_apr2` ask about the week after Easter Sunday (5 to 11 April 2021); those without ask about the
  7 days before the interview.** Cases were asked once, about the week before they fell ill, so their answer is the
  same in both.

## The four later workflows

**Who were the cases?** 19 of the 32 cases are female (59.4%), against 70 of the 110 classical controls (63.6%). The
age groups are listed in the order they first appear in the file, not by age.

**Every food.** Galia melon has the largest odds ratio, 21.4 (5.54 to 82.7). Several fruits that are in season in
July have odds ratios well below 1: strawberries 0.08 (0.03 to 0.20), cherries 0.03 (0.00 to 0.21). Cases were
interviewed from late May and asked about the week before they fell ill; classical controls were interviewed in July
and asked about the week before the interview. So the two groups' weeks fall in different seasons. Four items
(`andobst2`, `obstsalat2`, `tkobst2`, `trockenobst2`) are empty for every row and give no comparison. Blackberries
and other berries have no exposed case and give no ratio.

**Where did the cases shop?** `bioladen` and `markt` were filled in for cases only: every control is `NA`. So there
is no comparison, only counts. Neither column is in the codebook. The file's own column `bio` reads `bioladen`
1 to 4 as always, often, sometimes, never, and the table shows the two side by side: 23 of the 32 cases never shop at
an organic shop. `markt` is shown as the file spells it, 1 to 4 and 99, with no reading given for it.

**`bio` is not one question.** For cases it repeats `bioladen` (how often they shop at an organic shop); for controls
it is read from `biolm` (how often they buy organic food). Compared across cases and controls it would show 23 of 30
cases never against 0 of 108 classical controls, a difference made by the two questions. No workflow compares it.

**Where did the cases live?** `bula` is not in the codebook either. It is read here as the official two-digit code
of the federal state (Amtlicher Gemeindeschlüssel). The evidence is the controls: in all 110 classical controls,
`kreis` begins with it, as a district's official code begins with its state's. The cases have no `kreis`, so for them
the reading is the same code assumed, not checked. The cases live in seven Länder: Baden-Württemberg 11,
Nordrhein-Westfalen 6, Niedersachsen 5, Bayern 5, Schleswig-Holstein 3, Hamburg 1 and Berlin 1. `data/laender.csv`
gives the 16 codes with each Land's ISO 3166-2 code and name, written for this example. The panel controls have no
`bula`.

## Notes

The project carries five authored notes on the melons, in its Notes tab: three findings under Results (Galia melon
against the matched panel controls; cantaloupe's single control and the other melons' none; the microbiology) and two
limitations under Discussion (melons eaten together; crude ratios and many comparisons).

## What this does not establish

- **No adjustment.** Age and sex are not adjusted for; the paper's estimates are.
- **No causes beyond the paper's.** The microbiological finding, not these odds ratios, identified the vehicle.
- **Many comparisons, no correction.** Six items per workflow, each with its own interval.
- **Not the paper's selection of the 81.** They are taken as published, not redrawn.
- **Not where the outbreak was.** The map shows the 32 cases interviewed, not the 84 or more cases notified in
  Germany.
- **Not why the seasonal fruits differ.** The weeks asked about fall in different seasons; that is a reading of the
  design, not a finding of these tables.

## References

- Federal Agency for Cartography and Geodesy (BKG). Federal states of Germany, 2021, as published by geoBoundaries
  (gbOpen, DEU, ADM1, boundaryID DEU-ADM1-10402087). © GeoBasis-DE / BKG. Data licence Germany – attribution –
  version 2.0, [dl-de/by-2-0](https://www.govdata.de/dl-de/by-2-0).

- Łuczyńska A, Dreesman J, Mertens E, Wollenweber M, Perriat D, Rosner B. Recruiting controls from an online panel
  for a case–control study enabled a timely and reliable foodborne *Salmonella* outbreak investigation, Germany
  2021. *Epidemiology and Infection*. 2023;151:e70. [doi:10.1017/S0950268823000493](https://doi.org/10.1017/S0950268823000493)
- Łuczyńska A, Dreesman J, Mertens E, Wollenweber M, Perriat D, Rosner B. Dataset for the same, version 1. Zenodo;
  2022. [doi:10.5281/zenodo.7333295](https://doi.org/10.5281/zenodo.7333295). CC BY 4.0.
