# Predicting Iowa Corn Yield: Agentic AI Training on Jetstream2

## The idea

To build a scientific workflow in which an **AI agent** predicts county-level **corn yield in Iowa** from growing season weather, running on [Jetstream2](https://jetstream-cloud.org/).

The agent will load the data, build weather predictors, train a **linear regression** and a **random forest**, compare them, and explain what drives yield.

**Status:** planning stage. This repo currently holds the yield data

## Repository contents

```
data/
├── raw/
│   └── usda_nass_iowa_corn_raw.csv     # original USDA download, unchanged
└── iowa_corn_yield_2000_2025.csv       # cleaned
```

## The data

`data/iowa_corn_yield_2000_2025.csv`: 2,470 records of corn grain yield for Iowa's 99 counties, 2000–2025.

| Column | Description |
|---|---|
| `year` | Harvest year |
| `county` | County name |
| `fips` | County code (for joining with other data such as weather) |
| `yield_bu_acre` | Corn grain yield (bushels per acre) |

All 99 counties are reported through 2014. Some recent years have fewer counties because USDA does not publish county estimates when there are too few survey responses.

`data/raw/usda_nass_iowa_corn_raw.csv`: the original, unchanged download from USDA (3,620 rows, 21 columns).

### How the data was cleaned

The cleaned file was made from the raw file with these steps:

1. **Kept only grain yield:** removed 1,134 rows of silage yield (tons/acre)
2. **Removed "OTHER COUNTIES" rows:** 16 rows that combine several counties and cannot be matched to one county
3. **Kept 4 of the 21 columns:** the others were the same in every row or empty
4. **Created a county code (`fips`):** state code (19) + county code, e.g. Boone (015) → 19015
5. **Tidied formatting:** renamed columns (e.g. `Value` → `yield_bu_acre`), stored yield as a number, wrote county names in title case, and sorted by county and year

No yield values were changed, no missing years were filled in, and no unusual values (such as the 2012 drought lows) were removed.

## Planned workflow

1. Load county corn yield (this dataset)
2. Add growing-season weather for each county from NASA POWER, such as July rainfall, heat-stress days, and growing degree days
3. Train on 2000–2019 and test on 2020–2025
4. Compare linear regression and random forest
5. Have the agent explain which factors matter most, including the 2012 drought

## Possible extensions

- Add soil predictors (e.g., organic matter, water-holding capacity)
- Expand to other Corn Belt states

## Data source

USDA National Agricultural Statistics Service, Quick Stats: https://quickstats.nass.usda.gov/ (public domain). Selection: Survey → Crops → Field Crops → Corn → *CORN, GRAIN – YIELD, MEASURED IN BU / ACRE*, county level, Iowa, 2000–2025.

