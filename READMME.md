# Analyzing Historical Stock & Revenue Data and Building a Dashboard

This repository contains the complete implementation, scripts, and analytical breakdown for the IBM Data Science / Python for Data Science and AI capstone project: **"Analyzing Historical Stock/Revenue Data and Building a Dashboard"**.

The goal of this project is to extract historical share prices and revenue figures for **Tesla (TSLA)** and **GameStop (GME)** using two distinct extraction methods (API integration via `yfinance` and web scraping with `BeautifulSoup`), clean the extracted data, and visualize the trends using interactive Plotly dashboards.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Repository Structure](#repository-structure)
- [Prerequisites & Installation](#prerequisites--installation)
- [Lab Exercises Breakdown](#lab-exercises-breakdown)
  - [Question 1: Extract Tesla Stock Data (`yfinance`)](#question-1-extract-tesla-stock-data-yfinance)
  - [Question 2: Scrape Tesla Revenue Data (`BeautifulSoup`)](#question-2-scrape-tesla-revenue-data-beautifulsoup)
  - [Question 3: Extract GameStop Stock Data (`yfinance`)](#question-3-extract-gamestop-stock-data-yfinance)
  - [Question 4: Scrape GameStop Revenue Data (`BeautifulSoup`)](#question-4-scrape-gamestop-revenue-data-beautifulsoup)
  - [Question 5: Plot Tesla Dashboard](#question-5-plot-tesla-dashboard)
  - [Question 6: Plot GameStop Dashboard](#question-6-plot-gamestop-dashboard)
- [Key Analysis & Findings](#key-analysis--findings)
- [License](#license)

---

## Project Overview

- **Stock Extraction:** Financial APIs via Python's `yfinance` library to retrieve daily stock prices (Open, High, Low, Close, Volume, Splits).
- **Revenue Scraping:** Web scraping using `requests` and `BeautifulSoup` to parse quarterly revenue tables from archived financial records.
- **Data Wrangling:** Removing non-numeric symbols (`,`, `\$`) via regular expressions, removing null/empty rows, and standardizing datetimes.
- **Dashboard Construction:** Visualizing correlated share price and quarterly revenue performance using multi-row subplots in Plotly.

---

## Repository Structure

```text
├── question1_tesla_stock.py          # Extracts Tesla stock data via yfinance
├── question2_tesla_revenue_tail.py   # Scrapes and cleans Tesla quarterly revenue data
├── question3_gme_stock.py            # Extracts GameStop stock data via yfinance
├── question4_gme_revenue_tail.py     # Scrapes and cleans GameStop quarterly revenue data
├── question5_tesla_graph.py          # Plots the Tesla stock and revenue dashboard
├── question6_gme_graph.py            # Plots the GameStop stock and revenue dashboard
└── README.md                         # Project documentation
```

---

## Prerequisites & Installation

To run the scripts locally or inside a Jupyter / Watson Studio environment, install the required dependencies:

```bash
pip install yfinance pandas requests beautifulsoup4 plotly html5lib
```

---

## Lab Exercises Breakdown

### Question 1: Extract Tesla Stock Data (`yfinance`)
- **File:** `question1_tesla_stock.py`
- **Ticker:** `TSLA`
- **Output:** Extracts full historical records, resets the index in place, and prints the first 5 records (`head()`).

```python
import yfinance as yf

tesla = yf.Ticker("TSLA")
tesla_data = tesla.history(period="max")
tesla_data.reset_index(inplace=True)
print(tesla_data.head())
```

---

### Question 2: Scrape Tesla Revenue Data (`BeautifulSoup`)
- **File:** `question2_tesla_revenue_tail.py`
- **Target URL:** Archived Tesla revenue page
- **Output:** Parses the quarterly revenue table, strips `$` and `,`, drops null/empty rows, and inspects the last 5 rows (`tail()`). Earliest recorded revenue is **27** ($27M in Q2 2009).

```python
tesla_revenue["Revenue"] = tesla_revenue["Revenue"].str.replace(r',|\$', "", regex=True)
tesla_revenue.dropna(inplace=True)
tesla_revenue = tesla_revenue[tesla_revenue['Revenue'] != ""]
print(tesla_revenue.tail())
```

---

### Question 3: Extract GameStop Stock Data (`yfinance`)
- **File:** `question3_gme_stock.py`
- **Ticker:** `GME`
- **Output:** Extracts full historical records, resets index, and prints `head()`.
- **Note:** The first record (`2002-02-13`) shows a split-adjusted close price of **1.693353**.

```python
gme = yf.Ticker("GME")
gme_data = gme.history(period="max")
gme_data.reset_index(inplace=True)
print(gme_data.head())
```

---

### Question 4: Scrape GameStop Revenue Data (`BeautifulSoup`)
- **File:** `question4_gme_revenue_tail.py`
- **Target URL:** Archived GameStop revenue page
- **Output:** Extracts quarterly revenue table, cleans formatting, and displays the last 5 entries (`tail()`).

```python
print(gme_revenue.tail())
```

---

### Question 5: Plot Tesla Dashboard
- **File:** `question5_tesla_graph.py`
- **Dashboard Function:** `make_graph(tesla_data, tesla_revenue, 'Tesla')`
- **Visuals:** Two synchronized subplots comparing Tesla's historical share price against its quarterly revenue up to June 2021.

---

### Question 6: Plot GameStop Dashboard
- **File:** `question6_gme_graph.py`
- **Dashboard Function:** `make_graph(gme_data, gme_revenue, 'GameStop')`
- **Visuals:** Two synchronized subplots illustrating GameStop's share price performance versus cyclical retail revenue.

---

## Key Analysis & Findings

1. **Tesla (2018–2021):**
   - **Trend:** Both revenue and stock price demonstrated exponential, correlated growth.
   - **Observation:** Vehicle production scaling and global expansion directly mirrored substantial quarterly revenue climbs and an aggressive bull run in market valuation.

2. **GameStop (2020–2021):**
   - **Trend:** Stock price surged dramatically in early 2021 (the retail short squeeze event), rising well over $80, while revenue did not show a corresponding increase.
   - **Observation:** GameStop's revenue displayed typical retail seasonality (Q4 holiday spikes) with an overall gradual plateau, illustrating a sharp decoupling between market share price and business revenue fundamentals during early 2021.

---

## License

This project was developed as part of the IBM Data Science Professional Certificate curriculum. All datasets and materials are provided for educational purposes.