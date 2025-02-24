# commodities_scraper
Extract Myanamr Agriculural Commodities Prices from Htwet Toe Website - https://htwettoe.com/market

## Overview

This Python script is designed to extract Myanmar's commodity pricing data from Htwet Toe website and load it into a PostgreSQL database. The data is extracted using **BeautifulSoup** and **requests**, then processed and inserted into the database for further analysis.

## Key Features
- **Web Scraping**: The script retrieves data from the target website using BeautifulSoup.
- **Data Transformation**: The scraped data is cleaned and transformed, including price parsing and Unicode character removal.
- **PostgreSQL Integration**: Extracted data is loaded into a PostgreSQL database. The script uses a **DimMarket** and **DimCommodity** structure for dimension tables and inserts commodity price data into a **FactPricing** table.
- **Handling of Multiple Dates**: The script fetches data for the current month and inserts missing dates into the database.

## Requirements
- Python 3.x
- Libraries: `requests`, `BeautifulSoup4`, `pandas`, `psycopg2`, `tqdm`
- PostgreSQL Database with tables: `DimMarket`, `DimCommodity`, `FactPricing`
- A file named `pgsql_credentials.txt` containing PostgreSQL connection parameters (host, port, username, password).

## Installation

1. **Install the required libraries** using `pip`:
   ```bash
   pip install requests beautifulsoup4 pandas psycopg2 tqdm
   ```

2. **Prepare PostgreSQL**:
   Ensure that the PostgreSQL database is set up with the necessary tables:
   - `DimMarket` for storing market names.
   - `DimCommodity` for storing commodity names.
   - `FactPricing` for storing the pricing data.
   
   You can modify the database connection details in the script (via `pgsql_credentials.txt`).

3. **Set Up Credentials**:
   Create a text file named `pgsql_credentials.txt` with the following structure (replace with your own values):
   ```
   host
   port
   username
   password
   ```

## How It Works

### 1. **Connecting to PostgreSQL**
   The `connect_to_pgsql()` function establishes a connection to the PostgreSQL database using credentials from the `pgsql_credentials.txt` file.

### 2. **Web Scraping**
   - The `connect_to_website(url)` function sends a GET request to retrieve the HTML content of the page.
   - The `extract_data(bsObj, current_date)` function parses the HTML and extracts relevant data: market names, commodity names, units of measurement, and prices.

### 3. **Data Transformation**
   - Extracted data is processed to clean the price information and remove unwanted characters.
   - Prices are split into maximum and minimum prices, and data is structured into a Pandas DataFrame.

### 4. **Loading Data into PostgreSQL**
   - **DimMarket** and **DimCommodity** tables are populated with new market and commodity values, ensuring no duplicates.
   - The **FactPricing** table is populated with the pricing data, associating it with the correct market and commodity IDs.

### 5. **Handling Missing Dates**
   - The script generates a list of all dates for the current month.
   - It then checks which dates have already been inserted into the `FactPricing` table and processes only the missing dates.

### 6. **Error Handling**
   - The script handles various errors such as connection issues or missing data, and it logs these errors for debugging.

## How to Run

1. **Set the URL** of the website you wish to scrape. The default URL in the script is set to:
   ```
   url = "https://htwettoe.com/market?date="
   ```

2. **Run the script**:
   ```bash
   python script_name.py
   ```

3. The script will process each date, fetch the data, and load it into PostgreSQL. It pauses for 1 second between each request to prevent server overload.

## Data Files

- The script generates Excel files with the scraped data for each day that can be found in the script's directory.
  - The file is named `Crop Data <current_date>.xlsx`.

## Notes

- The script assumes that data extraction can fail due to temporary issues with the website. It retries the extraction process for each date.
- Ensure that PostgreSQL is running and accessible from the machine where the script is executed.
- Modify the database schema or query statements as needed to match your database structure.

## License

This script is provided as-is. You may modify it for personal or commercial use but ensure proper attribution where applicable.

## Accessing the Extracted Data

The extracted commodity pricing data can be easily accessed and visualized using Looker Studio. To view and interact with the data, click the following link:

View Extracted Data in Looker Studio >> https://lookerstudio.google.com/s/uGooY3Iqa7Q

This interactive dashboard allows you to explore various data points, trends, and analysis for a deeper understanding of the commodity pricing information.
