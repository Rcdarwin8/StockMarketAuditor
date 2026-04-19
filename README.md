# 🎯 Dynamic Stock Market Auditor & Excel Reporter

## 📋 Project Overview

A comprehensive **Java-based automation utility** that scrapes dynamic stock market data from WebTables, performs real-time calculations, validates data accuracy, and generates beautifully formatted Excel audit reports.

---

## 🎯 Key Features

### 1. **Dynamic WebTable Scraping**
- ✅ Complex XPath indexing for precise data extraction
- ✅ Handles dynamic JavaScript-rendered content
- ✅ Robust error handling and fallback mechanisms
- ✅ Multi-site support (MoneyControl, NSE India, etc.)

### 2. **Real-Time Data Processing**
- ✅ Manual percentage change calculation: `((Current - Previous) / Previous) × 100`
- ✅ Advanced string cleaning (removes ₹, $, commas, special characters)
- ✅ Type casting from String to Double for mathematical accuracy
- ✅ Data validation with tolerance threshold (0.1%)

### 3. **Intelligent Filtering**
- ✅ Identifies "High Performing" stocks (>5% change)
- ✅ Validation status tracking (PASS/FAIL)
- ✅ Automated audit trail generation

### 4. **Professional Excel Reporting**
- ✅ Custom cell formatting (colors, fonts, borders)
- ✅ Currency formatting (₹#,##0.00)
- ✅ Percentage formatting (0.00%)
- ✅ Conditional formatting (green for PASS, red for FAIL)
- ✅ Auto-sized columns with padding
- ✅ Merged cells for titles and summaries

---

## 🏗️ Project Structure

```
StockMarketAuditor/
│
├── pom.xml                                    # Maven dependencies
├── README.md                                  # This file
│
└── src/main/java/com/stockauditor/
    ├── StockMarketAuditor.java               # Main entry point
    │
    ├── model/
    │   └── StockData.java                    # POJO for stock data
    │
    ├── scraper/
    │   └── WebTableScraper.java              # Web scraping logic
    │
    └── excel/
        └── ExcelReporter.java                # Excel generation logic
```

---

## 🛠️ Technical Stack

| Technology | Version | Purpose |
|------------|---------|---------|
| **Java** | 11+ | Core programming language |
| **Selenium WebDriver** | 4.16.1 | Browser automation & web scraping |
| **Apache POI** | 5.2.5 | Excel file generation (.xlsx) |
| **WebDriverManager** | 5.6.3 | Automatic ChromeDriver management |
| **Maven** | 3.6+ | Dependency management & build tool |

---

## 🚀 Setup Instructions

### Prerequisites
1. **Java JDK 11+** installed
2. **Maven 3.6+** installed
3. **Google Chrome** browser installed
4. **Internet connection** for web scraping

### Installation Steps

1. **Clone or Extract the Project**
   ```bash
   cd /home/user/StockMarketAuditor
   ```

2. **Install Dependencies**
   ```bash
   mvn clean install
   ```

3. **Run the Application**
   ```bash
   mvn exec:java -Dexec.mainClass="com.stockauditor.StockMarketAuditor"
   ```

   Or compile and run directly:
   ```bash
   mvn clean package
   java -jar target/stock-market-auditor-1.0-SNAPSHOT.jar
   ```

---

## 🌐 Target Websites

### Primary Source: MoneyControl Top Gainers
- **URL**: https://www.moneycontrol.com/stocks/marketstats/nsegainer/index.php
- **Data Extracted**:
  - Company Name (Column 1)
  - Previous Close Price (Column 3)
  - Current Price (Column 4)
  - Percentage Change (Column 5) - for validation

### XPath Strategy
```java
// Row indexing (skip header)
String rowXPath = "//table[contains(@class, 'mctable')]//tr[" + (rowIndex + 1) + "]";

// Cell extraction with column indexing
String companyNameXPath = rowXPath + "/td[1]//a";
String prevCloseXPath = rowXPath + "/td[3]";
String currentPriceXPath = rowXPath + "/td[4]";
String changePercentXPath = rowXPath + "/td[5]";
```

---

## 📊 Data Processing Pipeline

### 1. **Web Scraping Phase**
```
Navigate to URL → Wait for table load → Extract rows → 
For each row: Extract cells using XPath indexing → Clean data
```

### 2. **Data Cleaning Phase**
```java
// Remove currency symbols, commas, spaces
String cleaned = priceText.replaceAll("[₹$,\\s]", "");
double price = Double.parseDouble(cleaned);
```

### 3. **Calculation Phase**
```java
// Manual percentage change calculation
double change = ((currentPrice - previousClose) / previousClose) * 100;
```

### 4. **Validation Phase**
```java
// Compare with website value (0.1% tolerance)
double difference = Math.abs(calculated - website);
boolean isValid = difference <= 0.1;
```

### 5. **Filtering Phase**
```java
// Filter high performers (>5% change)
List<StockData> highPerformers = stocks.stream()
    .filter(stock -> stock.getCalculatedChangePercent() > 5.0)
    .collect(Collectors.toList());
```

### 6. **Excel Export Phase**
```
Create workbook → Apply styles → Write data → 
Add summary → Auto-size columns → Save to file
```

---

## 📈 Sample Output

### Console Output
```
═══════════════════════════════════════════════════════════════
🎯 DYNAMIC STOCK MARKET AUDITOR & EXCEL REPORTER
WebTable Data Auditor & Automated Reporting System
Tech Stack: Java | Selenium | Apache POI | XPath
═══════════════════════════════════════════════════════════════

🔧 STEP 1: Initializing WebDriver
─────────────────────────────────────
✅ WebDriver initialized successfully

🔧 STEP 2: Scraping Dynamic WebTable
─────────────────────────────────────
🌐 Navigating to: https://www.moneycontrol.com/...
⏳ Waiting for table to load...
📊 Found 25 stock entries
✅ Row 1: Stock[Reliance Industries | Prev: 2450.00 | Curr: 2580.00 | Change: 5.31% | ✓ MATCH]
✅ Row 2: Stock[TCS Ltd | Prev: 3200.00 | Curr: 3340.00 | Change: 4.38% | ✓ MATCH]
...

📈 HIGH PERFORMING STOCKS (Change > 5.0%):
═══════════════════════════════════════════════════════════════
Total Stocks Analyzed: 15
High Performers: 8
═══════════════════════════════════════════════════════════════

✅ Excel report generated successfully!
📁 File location: /mnt/user-data/outputs/Stock_Market_Audit_Report_2026-01-02_10-30-45.xlsx
```

### Excel Report Format
| Company Name | Previous Close (₹) | Current Price (₹) | Calculated Change (%) | Website Change (%) | Validation Status | Audit Result |
|--------------|-------------------|-------------------|----------------------|-------------------|-------------------|--------------|
| Reliance Industries | ₹2,450.00 | ₹2,580.00 | 5.31% | 5.31% | ✓ MATCH | PASSED |
| Infosys Ltd | ₹1,450.00 | ₹1,540.00 | 6.21% | 6.20% | ✓ MATCH | PASSED |

---

## ⚙️ Configuration Options

### Modify High Performance Threshold
```java
// In ExcelReporter.java
private static final double HIGH_PERFORMANCE_THRESHOLD = 5.0; // Change to 3.0, 7.0, etc.
```

### Change Output Directory
```java
// In StockMarketAuditor.java
private static final String OUTPUT_DIRECTORY = "/mnt/user-data/outputs/";
```

### Enable Headless Mode (No Browser UI)
```java
// In initializeWebDriver() method
options.addArguments("--headless");
```

### Increase/Decrease Stock Count
```java
// In WebTableScraper.java
int maxRows = Math.min(rows.size(), 15); // Change 15 to desired count
```

---

## 🧪 Testing Recommendations

1. **Unit Testing** (Optional with JUnit):
   ```java
   @Test
   public void testPercentageCalculation() {
       StockData stock = new StockData("TestStock", 100.0, 110.0);
       assertEquals(10.0, stock.getCalculatedChangePercent(), 0.01);
   }
   ```

2. **Integration Testing**:
   - Test with different websites
   - Test with edge cases (zero prices, negative changes)
   - Test Excel formatting with various data sizes

3. **Performance Testing**:
   - Measure scraping time for different stock counts
   - Monitor memory usage during Excel generation

---

## 🐛 Troubleshooting

### Issue: ChromeDriver Not Found
**Solution**: WebDriverManager should auto-download it. If not:
```bash
mvn clean install -U
```

### Issue: WebTable Not Loading
**Solution**: Increase wait time in WebTableScraper:
```java
wait.until(ExpectedConditions.presenceOfElementLocated(...), Duration.ofSeconds(30));
```

### Issue: Excel File Not Generated
**Solution**: Check output directory permissions:
```bash
mkdir -p /mnt/user-data/outputs
chmod 755 /mnt/user-data/outputs
```

### Issue: Data Cleaning Errors
**Solution**: Add debug logging:
```java
System.out.println("Original text: " + priceText);
System.out.println("Cleaned text: " + cleaned);
```

---

## 📝 Code Highlights

### Complex XPath Indexing
```java
// Navigate through table structure with precise indexing
String rowXPath = "//table[contains(@class, 'mctable')]//tr[" + (i + 1) + "]";
String cellXPath = rowXPath + "/td[" + columnIndex + "]";
```

### Data Cleaning & Type Casting
```java
// Remove special characters and convert to double
String cleaned = priceText.replaceAll("[₹$,\\s]", "").trim();
double price = Double.parseDouble(cleaned);
```

### Validation with Tolerance
```java
// Allow 0.1% tolerance for rounding differences
double tolerance = 0.1;
boolean isValid = Math.abs(calculated - website) <= tolerance;
```

### Excel Custom Formatting
```java
// Create percentage style with custom format
CellStyle percentStyle = workbook.createCellStyle();
percentStyle.setDataFormat(workbook.createDataFormat().getFormat("0.00\"%\""));
```

---

## 🎓 Learning Outcomes

By studying this project, you'll learn:
- ✅ Advanced Selenium WebDriver techniques
- ✅ Complex XPath navigation strategies
- ✅ String manipulation and data cleaning
- ✅ Apache POI for Excel generation
- ✅ OOP design patterns (Model-View separation)
- ✅ Error handling and logging best practices
- ✅ Maven dependency management
- ✅ Real-world automation workflows

---

## 🚀 Future Enhancements

1. **Database Integration**: Store historical data in MySQL/PostgreSQL
2. **Scheduling**: Run automatically using Cron jobs or Spring Scheduler
3. **Email Notifications**: Send reports via JavaMail API
4. **Charts & Graphs**: Add visual analytics using JFreeChart
5. **Multi-Threading**: Parallel scraping for faster execution
6. **REST API**: Expose functionality via Spring Boot REST endpoints
7. **Web Dashboard**: Real-time monitoring using React/Angular frontend

---

## 📄 License

This project is provided as-is for educational and commercial use.

---

## 👨‍💻 Author

**Stock Market Auditor System**  
Version: 1.0  
Created: 2026-01-02

---

## 📞 Support

For questions or issues:
1. Check the Troubleshooting section
2. Review console logs for error messages
3. Verify website structure hasn't changed
4. Test with alternative data sources

---

**Happy Auditing! 📊✨**
