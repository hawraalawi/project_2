# Wind Turbines: Renewable Energy Private Equity Investment Analysis

## Problem Statement

A private equity firm focused on renewable energy aims to acquire U.S. wind farms but requires deeper insight into the specific structural characteristics and geographic locations that drive peak energy efficiency. This project analyzes turbine specifications and estimated energy output across the four U.S. Census Regions to identify which regions and turbine designs offer the strongest historical performance for a new investment.

## Executive Summary

This project analyzes the U.S. Wind Turbine Database (70,808 turbines) alongside supplementary state-level data on wind power density, electricity rates, and electricity bills. After cleaning the data (removing turbines with missing capacity/physical specs, filtering out low-confidence records, and dropping non-Census-Region territories), the working dataset was merged on state and grouped into the four Census Regions: Northeast, Midwest, South, and West.

Since actual EIA-923 Net Generation data wasn't available for this project, a **Net Generation proxy** was engineered using the industry-standard formula (rated capacity × estimated capacity factor × 8,760 hours/year), with the capacity factor scaled by each state's relative wind power density. This let us estimate energy output per turbine and compare regions on more than just installed capacity.

The analysis found that the Midwest leads by a wide margin in total installed capacity and turbine count, but when compared on a *per-turbine* basis, all four regions produce fairly similar estimated energy output. The real driver of higher output at the turbine level is **rotor diameter** - larger rotors are strongly and consistently linked to higher estimated generation, regardless of region. Combining estimated output with electricity rates, the Midwest and South offer the best balance of strong performance and low electricity costs, while the Northeast is the weakest region on every measure. Our recommendation is to prioritize large-rotor turbine projects in the Midwest and South, treat the West as a strong but costlier alternative, and deprioritize the Northeast.

## File Directory

| File / Folder | Description |
|:---|:---|
| `README.md` | This file - project overview, summary, and findings |
| `Data/wind-turbines.csv` | Original U.S. Wind Turbine Database file |
| `Data/extra/windiest-states-in-the-us.-2025.csv` | Original state-level wind speed/wind power data |
| `Data/extra/average_electricity_rates.csv` | Original state-level electricity rate data |
| `Data/extra/average_electricity_bills.csv` | Original state-level electricity bill data |
| `Data/wind_turbines_clean.csv` | Final cleaned and merged dataset used for analysis |
| `Code/EDA_Report_Template.ipynb` | Full analysis notebook: data cleaning, merging, Net Generation proxy calculation, and EDA |
| `Presentation/` | Final presentation slides (PDF) |
| `Presentation/images/` | Saved chart images (viz1-viz5) used in the presentation |

## Data & Data Dictionary

**Data sources:**
- Main turbine dataset: [United States Wind Turbine Database](https://emp.lbl.gov/publications/us-wind-turbine-database-files), Lawrence Berkeley National Laboratory
- Supplementary datasets (average electricity rates, average electricity bills, windiest U.S. states): provided by the course instructor as part of the project materials

**Data Dictionary** (final cleaned dataset, `wind_turbines_clean.csv`):

| Feature / Column | Data Type | Source | Description |
|:---|:---|:---|:---|
| case_id | int | Original | Unique stable ID for each turbine |
| t_state | string | Original | 2-letter state code where turbine is located |
| t_county | string | Original | County where turbine is located |
| t_fips | string | Original | State + county FIPS code |
| p_name | string | Original | Name of the wind power project |
| p_year | float | Original | Year turbine became operational |
| p_tnum | int | Original | Number of turbines in the project |
| p_cap | float | Original | Cumulative capacity of the whole project (MW) |
| t_manu | string | Original (cleaned) | Turbine manufacturer, with duplicate/legacy brand names consolidated (e.g. Gamesa → Siemens) |
| t_model | string | Original | Manufacturer's model name |
| t_cap | float | Original | Rated capacity of a single turbine (kW) |
| t_hh | float | Original | Hub height (m) |
| t_rd | float | Original | Rotor diameter (m) |
| t_rsa | float | Original | Rotor swept area (m²) |
| t_ttlh | float | Original | Total turbine height, ground to blade tip (m) |
| retrofit | int | Original | 0/1 flag - whether turbine was retrofitted |
| retrofit_year | float | Original | Year of retrofit (if applicable) |
| t_conf_atr | int | Original | Confidence score (1-3) for turbine attribute data |
| t_conf_loc | int | Original | Confidence score (1-3) for turbine location data |
| xlong | float | Original | Longitude |
| ylat | float | Original | Latitude |
| census_region | string | Engineered | US Census Region (Northeast/Midwest/South/West), mapped from t_state |
| avg_wind_speed_mph | float | Original (merged) | Average state wind speed (mph) |
| wind_speed_328ft | float | Original (merged) | Mean wind speed at 328ft (m/s) |
| wind_power_328ft | float | Original (merged) | Mean wind power density at 328ft (W/m²) |
| wind_speed_33ft | float | Original (merged) | Mean wind speed at 33ft (m/s) |
| rate_residential | float | Original (merged) | Residential electricity rate (cents/kWh) |
| rate_commercial | float | Original (merged) | Commercial electricity rate (cents/kWh) |
| rate_average | float | Original (merged) | Average electricity rate across sectors (cents/kWh) |
| bill_residential | float | Original (merged) | Average residential electricity bill ($) |
| bill_commercial | float | Original (merged) | Average commercial electricity bill ($) |
| bill_average | float | Original (merged) | Average electricity bill across sectors ($) |
| t_cap_mw | float | Engineered | t_cap converted from kW to MW |
| capacity_factor_est | float | Engineered | Estimated capacity factor, scaled from the US national average (34.6%) by each state's relative wind power density |
| net_gen_mwh | float | Engineered | Estimated annual net generation (MWh/year) - a proxy for actual EIA-923 net generation, calculated as t_cap_mw × capacity_factor_est × 8760 |

## Important Visualizations

- **Total Installed Capacity by Census Region** - the Midwest and South lead by a wide margin in total installed capacity.
- **Wind Speed vs Electricity Rate by Region** - the Midwest has the strongest wind but a below-average electricity rate; the Northeast has the opposite.
- **Net Generation vs Electricity Rate by Region** - the Midwest and South combine strong estimated output with the cheapest electricity.
- **Median Net Generation by Region** - per-turbine output is actually fairly close across all four regions, unlike the large gap in total capacity.
- **Rotor Diameter vs Net Generation** - a clear, strong positive relationship: larger rotors consistently produce more estimated energy.

(See `Presentation/images/` for the saved chart files and the notebook for full-size versions with code.)

## Conclusions & Recommendations

- The Midwest has the most installed wind capacity (~50,100 MW) and the strongest average wind speed (18.6 mph) of any region.
- But per-turbine, all four regions produce fairly similar estimated net generation (~5,100-5,900 MWh/year) - the Midwest's advantage is mostly about scale (number of turbines built), not per-turbine efficiency.
- Rotor diameter is the strongest driver of net generation found in the data - bigger rotors mean more energy, almost regardless of region.
- The West has the highest median net generation, but the Midwest and South offer the best combination of strong output and low electricity rates.
- The Northeast is the weakest performer overall: lowest net generation, highest electricity rate, and fewest turbines.

**Recommendations:**
- Prioritize turbine projects with large rotor diameters (100m+) regardless of region - this is the strongest single factor linked to higher energy output.
- The Midwest and South are the strongest regions overall: solid net generation combined with the cheapest electricity rates.
- The West produces the most energy per turbine, but its higher electricity rate makes it a slightly less cost-efficient choice than the Midwest/South.
- Deprioritize the Northeast - weakest wind, most expensive electricity, and lowest turbine count.

## Areas for Further Research

- Actual EIA-923 Net Generation data (MWh) wasn't available, so this analysis used a proxy built from turbine capacity and an estimated capacity factor scaled by regional wind power - not real production numbers.
- Electricity rates used are retail/consumer prices, not necessarily the wholesale price a wind farm would actually be paid for its electricity.
- This analysis compared whole Census Regions - looking at individual states could reveal more specific and precise investment locations.

## Sources

- Berkeley Lab (Lawrence Berkeley National Laboratory) (n.d.) *United States Wind Turbine Database*. Available at: https://emp.lbl.gov/publications/us-wind-turbine-database-files (Accessed: 12 July 2026).
- Supplementary datasets (average electricity rates, average electricity bills, and windiest U.S. states) were provided by the course instructor as part of the project materials.
