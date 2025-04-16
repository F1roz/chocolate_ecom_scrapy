# **Chocolate Scraping Project**

This project is a web scraping solution developed using **Scrapy** to collect data from the website [https://www.chocolate.co.uk/collections/all](https://www.chocolate.co.uk/collections/all). The scraped data includes chocolate product names, prices, and URLs, which are then processed and stored in a MySQL or Postgres database.

The scraper uses proxy rotation and user agent handling to bypass any anti-scraping measures, ensuring a smooth scraping process.

## **Features**

- Scrapes chocolate product information (name, price, URL).
- Proxy rotation using **ScraperAPI** to prevent IP blocking.
- Converts GBP prices to USD.
- Filters out duplicate products based on name.
- Stores scraped data in a MySQL or Postgres database.
- Handles pagination to scrape all product pages.

## **Technologies Used**

- **Python 3.x**
- **Scrapy**: Web scraping framework.
- **ScraperAPI**: API used for proxy rotation.
- **MySQL** / **Postgres**: Databases used to store the scraped data.
- **ItemLoaders**: Used for cleaning and structuring the scraped data.
- **Item Pipelines**: Used for processing data (price conversion, duplicate removal, and database storage).

## **Installation**

1. **Clone the Repository**:
   
   ```bash
   git clone https://github.com/yourusername/chocolate-scraper.git
   cd chocolate-scraper
   ```

2. **Create a Virtual Environment** (optional but recommended):

   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
   ```

3. **Install Dependencies**:

   Make sure to install all required dependencies listed in the `requirements.txt` file:

   ```bash
   pip install -r requirements.txt
   ```

4. **Database Setup**:

   - **MySQL**:
     1. Install MySQL on your system (if you don't have it installed).
     2. Create a database named `chocolate_scraping`.
     3. Ensure you have the correct MySQL credentials in the `SavingToMySQLPipeline` class.

   - **Postgres**:
     1. Install Postgres on your system (if you don't have it installed).
     2. Create a database named `chocolate_scraping`.
     3. Ensure you have the correct Postgres credentials in the `SavingToPostgresPipeline` class.

5. **Set Your API Key**:
   
   To use **ScraperAPI** for proxy rotation, sign up at [ScraperAPI](https://www.scraperapi.com/) and obtain your API key. Set your API key in the `chocolatespider.py` file:

   ```python
   API_KEY = 'YOUR_API_KEY'
   ```

## **Running the Spider**

Once everything is set up, you can run the spider by executing the following command:

```bash
scrapy crawl chocolatespider
```

This will start the scraping process, and the spider will crawl through the pages of the chocolate collection and store the results in your specified database (MySQL/Postgres).

### **Database Structure**

- **Table Name**: `chocolate_products`
- **Fields**:
  - `name` (VARCHAR): Product name
  - `price` (FLOAT): Product price in USD
  - `url` (VARCHAR): Product URL

## **Project Structure**

```
chocolate-scraper/
├── chocolatespider.py      # Scrapy Spider definition
├── itemloader.py           # ItemLoader for cleaning and processing data
├── middlewares.py          # Middleware for proxy rotation and request handling
├── pipeline.py             # Pipelines for price conversion, duplicate filtering, and data storage
├── settings.py             # Scrapy settings for configuring the spider
├── requirements.txt        # List of Python dependencies
└── README.md               # Project documentation (this file)
```

## **Custom Pipelines**

- **PriceToUSDPipeline**: Converts the price from GBP to USD using a hardcoded exchange rate.
- **DuplicatesPipeline**: Filters out duplicate products by name.
- **SavingToMySQLPipeline** / **SavingToPostgresPipeline**: Stores the scraped data in MySQL/Postgres databases.

## **Challenges Faced**

- **Proxy Rotation**: Initially, the scraper faced issues with IP blocking due to frequent requests. This was resolved by using **ScraperAPI** for proxy rotation.
- **Pagination Handling**: Ensuring the spider handled pagination properly to scrape all products across multiple pages was challenging. This was addressed by implementing logic to follow the "next" page links.
- **Data Cleaning**: Raw product data required cleaning (e.g., removing currency symbols from price). The `ItemLoader` helped efficiently clean and structure the data.
- **Database Storage**: Ensuring the data was stored correctly in both MySQL and Postgres databases required setting up the database connection and creating the appropriate tables.

