
# Niagara NEI extraction: README

DataraFlow Founder Challenge 2026 (Niagara Succession Signal). Owner: NEI dataset (dataset 3).

## 1. Files

| File | What it is |
|---|---|
| `Consolidated_NEI_Opendata.csv` | Niagara Employment Inventory, 7 survey years, 12,582 rows |
| `niagara_labour_jobs_extraction.xlsx` | Tables extracted from 5 Niagara Economic Development PDFs (labour force, jobs by sector and industry, business counts, municipal split of top manufacturers) |
| `nei_eda.py` | EDA script (legacy-business exposure, top-3 sectors) |

## 2. What the NEI file actually contains

- **Niagara Falls only.** Every row is Niagara Falls. No other municipality appears in any year.
- **Seven survey years:** 2017, 2018, 2019, 2022, 2023, 2024, 2025 (no 2020-2021). About 1,550-1,960 rows per year.
- **A panel:** 2,331 unique `NEI_ID`s tracked across years. Carry-over between consecutive surveys is 83-99%.
- **Five sectors only:** retail trade, accommodation and food services, other services, health care and social assistance, manufacturing.
- **Top 3 by business count is stable in every year:** retail trade, accommodation and food services, other services (accommodation overtakes retail from 2023).
- **Status is recorded.** `Size Range of Employees` includes "Permanently Closed or Vacant", so closures are observed directly rather than inferred.
- **Coordinates `x`, `y`** are Web Mercator (EPSG:3857).

**2025 snapshot (1,717 rows):**

| Sector | Records | Closed/vacant | Closed/vacant (%) |
|---|---|---|---|
| Accommodation and food services | 552 | 166 | 30.1 |
| Retail trade | 510 | 171 | 33.5 |
| Other services | 303 | 102 | 33.7 |
| Health care and social assistance | 222 | 47 | 21.2 |
| Manufacturing | 130 | 34 | 26.2 |
| **All** | **1,717** | **520** | **30.3** |

Persistence: of 421 records flagged closed in 2024, 399 are still closed in 2025 (8 reopened, 14 left the file). Of 1,097 open in 2024 and still listed, 72 (6.6%) are flagged closed in 2025.

## 3. Relevance to the challenge

- **The closed/vacant flag is the closest direct signal of business exit in the data.** It is a status, not a cause: it does not say whether an owner retired, failed or relocated.
- **Sector exposure.** The PDFs show accommodation and food services was hit hardest in 2019-2021 (employment -34.6%, real GDP -32.5%), retail trade moderately (-8.0%, -6.9%), other services barely (+1.1%, -5.3%). Closed/vacant shares in the NEI are highest in other services and retail, so recovery and vacancy tell different stories by sector.
- **Firm age** (year opened) is a proxy for succession horizon, not owner age. Pair it with teammates' owner-age data.
- **Geography.** The NEI cannot show differences between municipalities. That comparison has to come from teammates' datasets; the NEI supports a Niagara Falls deep-dive and neighbourhood mapping.
- **Labour context** (PDF tables): employment fell from 204.3k (2019) to 190.6k (2020) and reached 230.7k by 2022 Q2; unemployment was 5.8% in 2019 and 4.9% in 2022 Q2; employer businesses were 13,850 in 2022.

## 4. Pointers

1. Filter out "Permanently Closed or Vacant" for active-business counts, and keep it for vacancy analysis. Also review "Undetermined" (47-444 records a year).
2. Treat `year opened = 0` as missing (3,738 rows). Invalid share falls from 39.3% (2017) to 18.9% (2025).
3. Lowercase and trim sector labels first ("Accommodation and Food Services" appears as a stray variant).
4. Drop the 2 duplicate `(NEI_ID, Year)` rows.
5. Remove name, address, postal code and website columns before sharing anything. Aggregate only.
6. Use the `NEI_ID × Year` panel for transitions (open to closed, new entrants), and note the 2019-2022 gap.
7. Label every NEI output "Niagara Falls". Agree shared keys with the team: `year`, `municipality`, `sector`.
8. Suppress cells with fewer than 10 businesses.

## 5. Flags (including corrections to earlier work)

- **Earlier aggregate numbers overstate active businesses.** The 2025 counts (550 / 510 / 303), legacy shares (37% / 43% / 43%) and jobs proxy all include closed/vacant records. Rerun on active records only. Records not flagged closed in 2025: food 386, retail 339, other services 201.
- **Correction:** I earlier said the NEI does not record closures and suggested flagging disappearing IDs. This file has an explicit status, so use that first.
- **Correction:** a region-wide rerun is not possible, because only Niagara Falls exists. The `nan` Cramér's V was this, not a bug: sector × municipality needs more than one municipality.
- **Closed/vacant share is not smooth across years:** 12.1% (2017), 2.3% (2018), 18.6% (2019), 31.0% (2022), 24.4%, 26.7%, 30.3% (2025). Check for coding changes between survey years before comparing.
- **Size bands are coarse.** The jobs proxy uses midpoints ("Small 5-99" becomes 52; "Large 500+" becomes 500), so it is a rough measure only. Some bands ("Small", "Aggregate", "Zero Employees") are rare.
- **Workbook caveats:** the `Top3_Crosswalk` sheet uses the 2025 totals that include closed/vacant records. Annual and quarterly labour series come from different StatCan tables. Sector estimates for a metro area are noisy, and two PDF typos were resolved using chart values (see the workbook's README sheet).
- **Not in the PDFs:** monthly jobs, and a full municipal jobs table. The goods-movement report has no job counts.
- **Team size:** the challenge document caps teams at about four. Confirm the 7-8 person group with mentors.
=======
# Third-Data-Source

