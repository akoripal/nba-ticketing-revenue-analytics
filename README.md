
# NBA Ticketing & Revenue Analytics Platform (Tableau + Python)

**Tech:** Python (Colab), Dimensional Modeling, Tableau Public  
**Focus:** Ticket revenue, utilization KPIs, demand elasticity, and forecast validation

## Overview
This project builds a warehouse-style analytics model and an executive Tableau dashboard to analyze **ticket demand drivers** and **revenue performance** across an NBA season.

The pipeline:
1) pulls real NBA game logs (season + team)  
2) engineers demand drivers (win%, streaks, opponent effects, promotions)  
3) generates realistic ticketing metrics (tickets sold, utilization, pricing)  
4) produces a dimensional model (fact + dimensions)  
5) delivers a Tableau dashboard for **forecast validation** and **diagnostics**

## Dashboard
**Tableau Public:** [Paste your Tableau Public link here]  
**Key Views:**
- Revenue (Actual vs Predicted)
- Demand Elasticity (Win% vs Utilization)
- Revenue Variance by Opponent
- Utilization Pattern (Day-of-week × Month)
- Executive KPI row

## Business Questions Answered
- How closely does expected revenue match actual revenue across the season?
- How strongly does team performance correlate with utilization (demand elasticity)?
- Which opponents consistently over/under-perform expectations (variance diagnostics)?
- What scheduling patterns (weekday/month) influence utilization?

## Data Model (Dimensional)
This project uses a warehouse-style schema to prevent metric duplication and to keep fact tables clean.


### Tables
- **fact_ticket_sales_v2**
  - Keys: GAME_ID, DATE_KEY, OPPONENT_ID
  - Metrics: REVENUE, PREDICTED_REVENUE, REVENUE_VARIANCE, UTILIZATION_PCT, AVG_TICKET_PRICE, REV_PER_SEAT
  - Flags: PROMOTION_FLAG, RIVALRY_FLAG, BIG_MARKET_OPP
- **fact_team_performance**
  - WIN_FLAG, RUNNING_WIN_PCT, WIN_STREAK, POINT_DIFF, PTS
- **dim_game**
  - GAME_DATE, HOME_AWAY, MATCHUP, SEASON_ID, DATE_KEY
- **dim_date**
  - DAY_OF_WEEK, MONTH, YEAR
- **dim_opponent**
  - OPPONENT_ABBR, OPPONENT_ID

## Methodology
### Feature Engineering (Demand Drivers)
- Rolling win percentage
- Win streak indicator (3+ games)
- Weekend effect
- Rivalry / big-market opponent flags
- Promotion flag (simulated)

### Forecast Model
A simple regression model estimates **PREDICTED_REVENUE** using demand drivers:
- RUNNING_WIN_PCT
- WEEKEND_FLAG
- RIVALRY_FLAG
- PROMOTION_FLAG
- BIG_MARKET_OPP

**REVENUE_VARIANCE = REVENUE - PREDICTED_REVENUE** enables opponent-level diagnostics.

> Note: Ticketing metrics are simulated to resemble realistic market behavior, while game performance data is sourced from NBA game logs.

## Repository Structure
├── data/
│ ├── dim_date.csv
│ ├── dim_game.csv
│ ├── dim_opponent.csv
│ ├── fact_team_performance.csv
│ └── fact_ticket_sales_v2.csv
├── notebooks/
│ └── NBA_Ticketing_Tableau.ipynb
├── tableau/
│ └── [optional] dashboard.twbx
├── images/
│ ├── dashboard.png
│ └── model.png
└── README.md


## How to Reproduce (Google Colab)
1. Open `notebooks/NBA_Ticketing_Tableau.ipynb` in Colab
2. Run all cells to generate CSVs
3. Download the CSVs into `/data`
4. Open Tableau → connect to `fact_ticket_sales_v2.csv`
5. Add other CSVs and create relationships:
   - fact_ticket_sales_v2.GAME_ID = dim_game.GAME_ID
   - dim_game.DATE_KEY = dim_date.DATE_KEY
   - fact_ticket_sales_v2.OPPONENT_ID = dim_opponent.OPPONENT_ID
   - fact_team_performance.GAME_ID = dim_game.GAME_ID

## Results (Example Insights)
- Revenue spikes align with higher utilization and stronger demand drivers (weekend + opponent effects).
- Variance by opponent reveals matchups that over/under-perform model expectations.
- Demand elasticity view shows utilization sensitivity relative to rolling performance.

## Next Improvements
- Replace simulated ticketing with real ticketing/attendance data if available
- Move transformations into SQL (dbt/views) for a full analytics engineering workflow
- Add model evaluation metrics (MAE/MAPE) and cross-validation

## Author
**Anurag Koripalli**  
GitHub: [your profile link]  
Portfolio: [your website link]
