# Power BI Trading Strategy Analysis 📊
This Power BI project analyzes trading strategies using three datasets, combining them into a unified table with strategy labels. It visualizes key metrics like total PnL, win/loss counts, cumulative PnL, and trade trends. Users can drill through strategy details, filter by date, and explore insights interactively. 🚀 This will help document your project setup and guide others through the process.

## Overview
This project analyzes trading strategies using **Power BI** with data from three CSV files. It provides insights into strategy performance, trade statistics, and trends.

## Data Sources 📁
The project utilizes **three CSV files**:
- strategy_1.csv
- strategy_2.csv
- strategy_3.csv

Each file contains trade data with these columns:
- `symbol`
- `entry_datetime`
- `exit_datetime`
- `entry_type`
- `pnl`

## Project Setup ⚙️
### **Step 1: Import & Combine Data**
1. Open **Power BI**.
2. Click **Get Data** → **Text/CSV**.
3. Select and load all three CSV files.
4. Use **Power Query Editor** to **Append Queries** and merge them into a single table (**Trades**).
5. Add a **Custom Column** for `strategy_name`:
   ```M
   if Text.Contains([Source.Name], "strategy_1.csv") then "Strategy 1"
   else if Text.Contains([Source.Name], "strategy_2.csv") then "Strategy 2"
   else "Strategy 3"
   ```
6. Click **Close & Apply**.
7. **Another Way**
    Before Adding the csv files into one add "Strategy_Name" column to each data set then after add these three data sets into one data set Named as "Trade".

### **Step 2: Create Strategy Summary Page (Main Page)**
- **Table** containing:
  - `strategy_name`
  - **Total PnL** (`SUM(pnl)`)
  - **Win Count** (`COUNTROWS(FILTER(Trades, Trades[pnl] > 0))`)
  - **Loss Count** (`COUNTROWS(FILTER(Trades, Trades[pnl] < 0))`)
##**SUMMARYSHEET****
     ![Screenshot 2025-05-17 145759](https://github.com/user-attachments/assets/27c2cc1e-8555-47ac-846e-ca824efb2c8d)

#### **DAX Measures**
```DAX
TotalPnL = SUM(Trades[pnl])
WinCount = COUNTROWS(FILTER(Trades, Trades[pnl] > 0))
LossCount = COUNTROWS(FILTER(Trades, Trades[pnl] < 0))
```

- **Enable Drillthrough** for `strategy_name` to navigate to **Detailed Report Page**.

### **Step 3: Create Detailed Report (Drillthrough Page)**
- **Filters**:
  - Use **Drillthrough** to filter trades for selected `strategy_name`.
  - Add a **Slicer** for filtering by `entry_datetime`.

#### **New Columns**
1. **Cumulative PnL (Running Total)**
   ```DAX
   CumulativePnL = 
   CALCULATE(
       SUM(Trades[pnl]),
       FILTER(
           ALL(Trades),
           Trades[entry_datetime] <= EARLIER(Trades[entry_datetime])
       )
   )
   ```
2. **Entry Hour (Extract Hour from Datetime)**
   ```DAX
   EntryHour = HOUR(Trades[entry_datetime])
   ```

### **Step 4: Add KPIs & Visualizations**
#### **KPIs**
- **Total PnL** (`TotalPnL` measure).
- **Win Count** (`WinCount` measure).
- **Loss Count** (`LossCount` measure).

#### **Charts**
1. **Line Chart** (Cumulative PnL vs Entry Datetime)
2. **Bar Chart** (PnL by Hour of Day)
3. **Table** (Trade details: `symbol`, `entry_datetime`, `exit_datetime`, `entry_type`, `pnl`, `cumulative_pnl`).

### **Step 5: Testing & Optimization ✅**
- Validate **Drillthrough function**.
- Ensure correct **strategy and date-based filtering**.
- Style visuals for **better readability**.

##**DASH BOARD**
![Screenshot 2025-05-17 145946](https://github.com/user-attachments/assets/9f463fad-26e1-49e1-93df-36bcd68f0fea)

## **Next Steps 🚀**
- Automate data refresh.
- Add more strategy comparison insights.
- Improve UI for better interactivity.

---
### **Author** 👩‍💻
Created by **Y.Narmadha**
