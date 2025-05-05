# 🧾 Fund Manager Holdings Scraper

This project automates the extraction and analysis of **13F filings data** from [13f.info](https://13f.info). It scrapes holdings data from investment managers' quarterly filings, handles dynamic content loading (AJAX), calculates share count changes, and classifies transactions as Buy/Sell/Hold. The final output is a consolidated CSV file ready for investment research and analysis.

---

## 📌 Features

- 🔍 **Web Scraping** from 13f.info manager and filing pages  
- ⚙️ **Dynamic Table Handling** via AJAX detection and JSON fetching  
- 📊 **Quarter-over-Quarter Share Change Calculations**  
- 📁 **Cleaned & Consolidated CSV Output** for all managers and filings  
- 📈 **Buy/Sell/Hold Inference** based on share change  
- 🧪 **Robust Error Handling** and fallbacks for failed AJAX or HTML content  
- 🧩 **Modular Code Design** for scraping, parsing, and analysis

---

## 🛠️ Technical Approach

### Web Scraping
- Uses `requests` for HTTP requests
- `BeautifulSoup4` (with `lxml`) for parsing HTML
- Identifies manager and filing links from the homepage
- Scrapes metadata like fund name, quarter, and filing date

### Dynamic Content Handling (AJAX)
- Detects tables with `data-url` attribute (AJAX-loaded)
- Makes secondary requests to fetch JSON data
- Combines JSON rows with headers from HTML `<thead>`
- Includes fallback to standard HTML table scraping

### Data Storage & Manipulation
- Uses `pandas` for storing, cleaning, and merging data
- Filters for **common stock holdings** (where `cl == 'COM'`)
- Uses `pd.concat()` for merging across managers and quarters

---

## 🔢 Data Processing & Calculations

- Converts share and value columns to numeric using `pd.to_numeric`  
- Parses quarter info into `pandas.Period` for sorting  
- Calculates:
  - **Change** in share counts
  - **% Change**
  - **Transaction Type** (Buy/Sell/Hold) using `numpy.select`  
- Handles division-by-zero and missing values

---

## 🧯 Error Handling

- Try/Except blocks for:
  - Network errors (`requests.exceptions.RequestException`)
  - JSON errors (`json.JSONDecodeError`)
  - Missing data or invalid formats
- Logs or skips problematic filings instead of crashing

---

## 🤖 Website Politeness

- Uses randomized `time.sleep()` delays between requests
- Adds a custom `User-Agent` header for ethical scraping

---

## 📤 Output

- Final result is a UTF-8 encoded CSV:  
  **`Fund Manager Shares Analysis.csv`**
- Includes cleaned, processed, and enriched data for analysis

---

## 📂 Example Columns in Output

- `fund_name`, `filing_date`, `quarter`, `stock_symbol`, `shares`, `value_usd_000`  
- `change`, `pct_change`, `inferred_transaction_type`

---

## 📎 Dependencies

```bash
pip install requests beautifulsoup4 lxml pandas numpy
