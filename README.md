# Stock Market Auditor & Excel Reporter

A Java automation tool that scrapes live NSE top-gainer data from MoneyControl, validates percentage change calculations, and exports high-performing stocks to a formatted `.xlsx` report.

---

## Tech Stack

| Technology | Version | Role |
|---|---|---|
| Java | 11+ | Core language |
| Selenium WebDriver | 4.16.1 | Browser automation & scraping |
| Apache POI (XSSF) | 5.2.5 | Excel generation |
| WebDriverManager | 5.6.3 | Auto ChromeDriver management |
| Maven | 3.6+ | Build & dependencies |

---

## Project Structure

```
StockMarketAuditor/
├── pom.xml
└── src/main/java/com/stockauditor/
    ├── StockMarketAuditor.java      # Entry point & orchestration
    ├── model/StockData.java         # POJO with calculation & validation
    ├── scraper/WebTableScraper.java # Selenium scraping engine
    └── excel/ExcelReporter.java     # Apache POI report builder
```

---

## Prerequisites

Java JDK 11+, Maven 3.6+.

---

## Running

```bash
# Option 1 — Maven (recommended)
mvn clean install
mvn exec:java -Dexec.mainClass="com.stockauditor.StockMarketAuditor"

# Option 2 — JAR
mvn clean package
java -jar target/stock-market-auditor-1.0-SNAPSHOT.jar
```

Output saved to: `/mnt/user-data/outputs/Stock_Market_Audit_Report_<timestamp>.xlsx`

---

## How It Works

```
    Navigate to MoneyControl 
  → Wait for table 
  → Extract rows via XPath
  → Clean strings (strip ₹, commas) 
  → Parse to Double
  → Calculate % change 
  → Validate vs website value (±0.1% tolerance)
  → Filter high performers (> 5%) 
  → Write & save Excel report

```

## License
Provided as-is for educational and commercial use.
