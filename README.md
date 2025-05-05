**-----Project Description-----**  
This project automates the collection of 13F holdings data from 13f.info. The script identifies investment managers, scrapes links to their quarterly filings, extracts detailed holdings information from each filing page (handling dynamic content loading), calculates quarter-over-quarter changes in share counts, infers transaction types (Buy/Sell/Hold), and consolidates the processed data into a single CSV file (Fund Manager Shares Analysis.csv) for analysis.

**-----Technical Approach-----**  
**Web Scraping:** Utilizes requests for fetching HTML content and BeautifulSoup4 (with lxml parser) for parsing HTML structure to find manager/filing links and extract metadata (fund name, filing date, quarter).  
**Dynamic Content Handling:** Detects tables loaded via AJAX (using the data-url attribute) and makes secondary requests to fetch the JSON data directly. Falls back to direct HTML table parsing (BeautifulSoup) multiple fund managers listed on 13f.info. It begins by identifying manager-specific pages from the homepage, then extracts links to their individual quarterly filings. For each filing, it scrapes the holdings table data, along with metadata like the filing date and quarter identifier. The script handles tables loaded dynamically via AJAX requests and includes fallbacks for standard HTML parsing. Finally, it consolidates all scraped data, filters for common stock holdings, calculates quarter-over-quarter share changes (change, pct_change), infers transaction types (Buy/Sell/Hold), standardizes the output format, and saves the results into a single CSV file (Fund Manager Shares Analysis.csv).  

**-----Approach-----**   
**Web Scraping:** Utilized the requests library to fetch HTML content and BeautifulSoup4 (with the lxml parser) for robust HTML parsing and element extraction.  
**Link Discovery:** Implemented a multi-stage approach: first finding manager links, then finding quarterly filing links within each manager's page.  
**Dynamic Content Handling (AJAX):** Identified tables using the data-url attribute, indicative of AJAX loading. Made secondary requests to these AJAX endpoints, parsed the resulting JSON data, and integrated it with headers scraped from the original HTML <thead>. Included a fallback mechanism to direct HTML table parsing (BeautifulSoup) if AJAX failed or was not applicable.  
**Data Storage & Manipulation:** Employed pandas DataFrames for efficient storage, cleaning, filtering (cl == 'COM'), sorting, and consolidation (pd.concat) of the scraped data.    

**-----Data Processing & Calculation-----**  
Converted relevant columns (e.g., shares, value_usd_000) to numeric types, handling potential errors and non-numeric values (pd.to_numeric, .str.replace).
Parsed unstructured quarter strings into sortable pandas.Period objects for accurate chronological ordering.
Calculated change and pct_change in shares using groupby() on fund_name and stock identifier (sym or stock_symbol), combined with .diff() and .pct_change() after careful sorting. Handled division-by-zero for percentage change.
Inferred inferred_transaction_type based on the calculated change value using numpy.select.  
**Error Handling:** Incorporated try...except blocks to manage potential network errors (requests.exceptions.RequestException), JSON parsing errors (json.JSONDecodeError), and other exceptions during scraping and processing. Implemented checks for missing columns or data.  
**Website Politeness:** Included randomized delays (time.sleep) between HTTP requests to avoid overwhelming the 13f.info server. Used a standard User-Agent header.  
**Modularity:** Structured the code into distinct functions for different tasks (e.g., scraping manager links, quarter links, table data, fund name).  
**Output:** Generated a final, cleaned CSV file (Fund Manager Shares Analysis.csv) with a predefined set of columns using utf-8-sig encoding for compatibility.
