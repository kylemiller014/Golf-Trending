# Golf-Trending
# Golf Reporting Engine - Project Structure

## Purpose

This document defines the folder hierarchy and overall architecture for the Golf Reporting Engine project. The primary objective of the project is to create a maintainable analytics platform capable of:

- Downloading and storing PGA Tour data
- Maintaining a historical PostgreSQL database
- Calculating advanced player metrics
- Producing weekly rankings and reports
- Powering future dashboards and web applications

The project follows a layered architecture where each component has a single responsibility.

---

# Project Folder Structure

```
golf-reporting-engine/
│
├── README.md
├── PROJECT_STRUCTURE.md
├── .gitignore
├── .Renviron
├── .Rprofile
├── renv.lock
├── _targets.R
├── golf-reporting-engine.Rproj
│
├── config/
├── data/
├── database/
├── docs/
├── graphics/
├── logs/
├── R/
├── reports/
├── sql/
├── tests/
├── website/
└── references/
```

---

# Root Files

## README.md

Primary project overview.

Should contain:

- Project description
- Installation
- Running the pipeline
- Technologies used
- Future roadmap

---

## PROJECT_STRUCTURE.md

This document.

---

## _targets.R

Defines the entire data pipeline.

Responsible for orchestrating:

- Downloading data
- Cleaning data
- Updating database
- Calculating metrics
- Building reports

---

## .Renviron

Stores environment variables.

Example:

```
PGHOST
PGPORT
PGDATABASE
PGUSER
PGPASSWORD
```

Never commit this file.

---

## renv.lock

Locks package versions to ensure reproducibility.

---

# config/

Configuration files that control project behavior without changing code.

```
config/

    database.yml

    scoring_weights.yml

    api_settings.yml

    report_settings.yml

    courses.yml

    logging.yml
```

Examples:

- Database connection settings
- Metric weights
- Number of tournaments used for form calculations
- Report defaults
- Logging configuration

---

# data/

Temporary storage used during ETL.

This folder is **not** the primary database.

```
data/

    raw/

    staging/

    processed/

    exports/

    cache/

    snapshots/
```

---

## raw/

Stores data exactly as returned from the API.

No cleaning.

No modifications.

---

## staging/

Data after:

- column name cleaning
- type conversions
- basic validation

---

## processed/

Validated datasets ready to load into PostgreSQL.

---

## exports/

Analytics outputs exported from the database.

Examples:

```
player_form.parquet

course_history.parquet

power_rankings.parquet

field_summary.parquet
```

---

## cache/

Temporary API responses.

Safe to delete.

---

## snapshots/

Frozen weekly datasets.

Example:

```
2027-04-05/

2027-04-12/

2027-04-19/
```

Allows recreation of historical reports.

---

# database/

Everything related to PostgreSQL.

```
database/

    schema/

    migrations/

    seeds/

    backups/
```

---

## schema/

SQL files that create tables.

Example:

```
create_players.sql

create_rounds.sql

create_courses.sql

create_results.sql
```

---

## migrations/

Database schema changes.

Example:

```
001_create_players.sql

002_add_strokes_gained.sql

003_add_course_fit.sql
```

---

## seeds/

Optional seed data.

Examples:

- Course metadata
- Static player information
- Lookup tables

---

## backups/

Database backup files.

---

# sql/

Standalone SQL queries.

```
sql/

    analytics/

    reporting/

    validation/
```

---

## analytics/

Reusable SQL for calculations.

Examples:

```
course_history.sql

momentum.sql

recent_form.sql
```

---

## reporting/

Queries optimized for report generation.

---

## validation/

Quality assurance queries.

Examples:

- Duplicate players
- Missing rounds
- Invalid tournament IDs

---

# R/

Contains all R source code.

```
R/

    acquisition/

    validation/

    cleaning/

    warehouse/

    metrics/

    rankings/

    models/

    reporting/

    graphics/

    utilities/
```

---

## acquisition/

Responsible only for downloading data.

Examples:

```
download_players.R

download_schedule.R

download_results.R

download_rounds.R
```

---

## validation/

Checks incoming data quality.

Examples:

- Missing values
- Duplicate records
- Invalid scores

---

## cleaning/

Transforms raw data into standardized datasets.

Examples:

- Rename columns
- Standardize dates
- Convert data types

---

## warehouse/

Loads cleaned data into PostgreSQL.

Examples:

```
load_players.R

load_results.R

load_courses.R
```

---

## metrics/

Calculates descriptive player metrics.

Examples:

```
calculate_form.R

calculate_course_history.R

calculate_momentum.R

calculate_consistency.R

calculate_field_strength.R

calculate_course_fit.R
```

---

## rankings/

Builds composite rankings.

Examples:

```
form_index.R

overall_rating.R

tier_rankings.R
```

---

## models/

Future predictive analytics.

Examples:

```
expected_finish.R

cut_probability.R

top10_probability.R

win_probability.R
```

---

## reporting/

Produces final datasets used in reports.

Examples:

```
weekly_summary.R

field_preview.R

leaderboards.R
```

---

## graphics/

Creates figures.

No calculations should occur here.

Examples:

```
trend_chart.R

radar_chart.R

heatmap.R

player_card.R
```

---

## utilities/

Shared helper functions.

Examples:

```
connect_database.R

logger.R

helpers.R

dates.R
```

---

# reports/

Quarto / R Markdown reports.

```
reports/

    weekly/

    tournament/

    players/

    templates/
```

Future examples:

```
2027_masters_preview.qmd

2027_players_championship.qmd
```

---

# graphics/

Generated graphics.

```
graphics/

    weekly/

    social/

    website/

    thumbnails/
```

Output only.

No source code.

---

# docs/

Project documentation.

```
docs/

    architecture.md

    schema.md

    data_dictionary.md

    roadmap.md

    decisions.md
```

Recommended contents:

- Database schema
- Data dictionary
- Architecture diagrams
- Development roadmap
- Design decisions

---

# tests/

Automated testing.

```
tests/

    testthat/

        test_form.R

        test_courses.R

        test_rankings.R
```

Purpose:

- Prevent regressions
- Validate calculations
- Ensure pipeline stability

---

# logs/

Stores pipeline execution logs.

```
logs/

    2027-04-05.log

    2027-04-12.log
```

Useful for troubleshooting ETL failures.

---

# website/

Reserved for the future web application.

```
website/

    api/

    frontend/

    assets/

    components/
```

Possible future technologies:

- React
- Next.js
- HTML/CSS
- JavaScript
- API layer

---

# references/

Project knowledge base.

```
references/

    course_profiles/

    metric_definitions/

    research/

    formulas/

    ideas.md
```

Purpose:

- Research notes
- Metric definitions
- Formula documentation
- Course observations
- Future ideas

# Development Philosophy

This project follows several guiding principles:

1. Every folder has a single responsibility.
2. Raw data is never modified after download.
3. PostgreSQL is the single source of truth.
4. Derived metrics should be reproducible from raw data.
5. Reporting should consume analytics, not recreate calculations.
6. Configuration belongs in configuration files, not hardcoded values.
7. Reusable functions should be modular and independently testable.
8. The architecture should support future expansion to web applications, APIs, and interactive dashboards without requiring major redesign.