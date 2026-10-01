# PGA Tour Player Statistics

A Python notebook that extracts PGA Tour player statistics from the Sportradar API, transforms the data with pandas, and loads it into a MySQL database.

This notebook is the first stage of a broader PGA Tour data project. Dashboards and player-performance analysis will be added to this repository, building on the data prepared here.

## Why I Built This

I built this project to practise taking data through a complete extract, transform, and load (ETL) workflow. Using PGA Tour statistics gave me a practical dataset for working with an external API, turning nested JSON into a structured table, and preparing data for further analysis.

The aim was to connect these steps in one project: retrieving data, selecting useful statistics, calculating performance measures, and storing the results in a relational database.

## What the Project Covers

The notebook works with one season of PGA Tour player statistics and is currently configured for **2026**.

### 1. Extract

- Request season-level player statistics from the Sportradar Golf API using an API key.
- Parse the JSON response and inspect its structure.
- Access the player records and their nested statistics.

### 2. Transform

- Extract 12 fields into a pandas DataFrame, including player names, country, world ranking, events played, wins, runner-up finishes, top-10 and top-25 finishes, cuts made, FedEx ranking, and total strokes gained.
- Inspect the table's structure, data types, and non-null counts.
- Standardise country names to title case.
- Calculate two additional performance measures, rounded to two decimal places:

| Metric | Calculation |
| --- | --- |
| Win percentage | Wins / events played x 100 |
| Top-10 percentage | Top-10 finishes / events played x 100 |

Players with zero or negative events played receive missing percentage values, avoiding division by zero.

### 3. Load

- Connect to MySQL using SQLAlchemy and PyMySQL.
- Write the 14-column DataFrame to a table named `players_statistics` without the pandas index.
- Replace the existing table when the load step is run again.

## Skills Demonstrated

| Skill | How it is used |
| --- | --- |
| Python programming | Use variables, loops, lists, tuples, and dictionaries to process player records. |
| API integration | Send an authenticated HTTP request with headers and query parameters using `requests`. |
| JSON processing | Navigate a nested API response and extract selected fields. |
| Data preparation with pandas | Build and inspect a DataFrame, format text, and calculate percentage metrics. |
| ETL workflow development | Connect extraction, transformation, and database loading in a single notebook. |
| Database integration | Load a pandas DataFrame into MySQL through SQLAlchemy and PyMySQL. |
| Configuration management | Read API and database credentials from environment variables using `python-dotenv`. |
| Jupyter notebooks | Organise the workflow into stages with executable code and inspectable outputs. |

## Tools and Libraries

- Python and Jupyter
- `requests` for API requests
- `pandas` for data transformation
- `python-dotenv` for environment configuration
- MySQL for data storage
- `SQLAlchemy` and `PyMySQL` for the database connection and loading

The notebook's first cell installs its Python dependencies. It also installs and imports `mysql-connector-python`, although the load step uses PyMySQL.

## Scope and Limitations

The current notebook focuses on collecting, preparing, and storing data. Dashboards and further analysis of player performance will be added to this repository as the project develops; these are planned additions and are not included yet.

Each load replaces the existing `players_statistics` table, so the notebook does not retain previous season snapshots.

The notebook assumes that the API request succeeds and each player contains the expected fields. Explicit response validation, retries, and handling for missing fields would make the workflow more robust. Its database connection is built as a URL string, so credentials containing URL-reserved characters need appropriate encoding.

The output provides a structured starting point for further SQL queries or player-performance analysis. Results depend on the season and the data returned by the API when the notebook is run.
