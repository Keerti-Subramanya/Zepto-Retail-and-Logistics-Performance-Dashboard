# 📊 Zepto-Retail-and-Logistics-Performance-Dashboard

Welcome to the repository for the Zepto Retail & Logistics Performance Dashboard. This data engineering capstone project delivers real-time visibility into warehouse inventory health and geographical sales performance, explicitly designed for the rigorous demands of fast-paced delivery services.

<br/>

## 🖼️ Dashboard Overview
![BI Dahboard 1](https://github.com/user-attachments/assets/928920e0-8b3c-45c4-98f0-c195cc071569)

<br/>

![BI Dashboard 2](https://github.com/user-attachments/assets/b1b7038b-0972-4f92-8ef3-06afc5892a7a)

<br/>

![Bi Dashboard 3](https://github.com/user-attachments/assets/d3b0f116-8488-4e4a-af5b-4d719c8659e5)


<br/>

## 🎯 The Core Problem & Business Impact
Fast-paced delivery services like Zepto frequently face a critical "Real-Time Visibility Gap". There is a significant operational disconnect between monitoring warehouse inventory health and tracking geographical sales performance.

**Business Risk**: Without a unified, real-time data view, the business risks paralyzing capital by holding "dead stock" or directly losing revenue due to high-demand items unexpectedly going out-of-stock.

<br/>

## 🏗️ Data Architecture & ETL Pipeline
The foundational data pipeline extracts, transforms, and loads (ETL) information from three distinct, siloed sources into a unified analytical model.

### 1. Data Sources

**Products (Excel)**: The master product catalog containing category hierarchies, standard pricing, and descriptive attributes.

**Inventory (Excel)**: Dynamic warehouse metrics including stock levels, available quantities, and precise location tracking data.

**Sales (CSV)**: Geographical performance metrics, detailing city-level revenue and raw order volume.

### 2. Power Query Transformations (Data Prep)
To ensure structural integrity before modeling, the following transformations were engineered in Power Query:

**Standardization**: Renamed key identifier columns across all flat files to ensure strict relational uniformity.

**Single Source of Truth**: Removed duplicate descriptive columns from the Fact tables, enforcing that descriptive metadata resides strictly in the Dimension tables.

**Data Type Enforcement**: Audited all numerical columns and enforced correct data types to guarantee accurate mathematical aggregation downstream.

<br/>

## 🗄️ Data Modeling: Star Schema
The project utilizes a strict Star Schema architecture to optimize query performance and filtering.

**Dimension Table (Zepto_Products)**: Serves as the central analytical reference for all descriptive attributes, such as categories, pricing, and customer ratings.

**Fact Table (Zepto_Inventory)**: Contains the core quantitative warehouse metrics and links directly to the Products dimension.

**Architectural Decision - Isolated Sales Fact Table**: The Zepto_Sales table was deliberately loaded as a standalone fact table. Due to a simplified naming convention mismatch with the warehouse system, isolating it maintains strict data integrity while still enabling high-level financial reporting.

<br/>

## ⚙️ Advanced DAX Engineering
Custom Data Analysis Expressions (DAX) were engineered to derive complex business logic dynamically.

#### Iterator Functions (Row-by-Row Logic)
**Total Inventory Value**: Utilizes SUMX(Zepto_Inventory, Zepto_Inventory[Available Quantity] * Zepto_Inventory[Discounted Selling Price]). This evaluates the multiplication row-by-row before summing, guaranteeing an accurate capital calculation at the grand-total level.

**Total Warehouse Weight**: Utilizes SUMX to calculate the grams per item row (Weight (g) * Available Quantity) / 1000, converting the final output to kilograms for logistics managers.

<br/>

#### Filter Context Precision

**Out of Stock Items**: Utilizes CALCULATE(COUNTROWS(Zepto_Inventory), Zepto_Inventory[Out of Stock] == TRUE) to alter the filter context, counting only specific boolean flags to drive the health gauge needle.

**Average Discount Percentage**: Calculates AVERAGE(Zepto_Inventory[Discount Percent]) / 100 to convert whole numbers into true mathematical percentages for dashboard visualization.

<br/>

## 📈 Dashboard Views & User Interface

The visualization layer is divided into a three-focused solution architecture.

<br/>

#### Page 1: Executive Summary

**Critical KPIs**: Top-level metrics (Total Inventory Value, Stock Quantities, Warehouse Weight) instantly answer: "How much physical capital sits on shelves?".

**Warehouse Health Meter**: A Gauge chart tracking out-of-stock items against total capacity. A low needle position indicates a healthy supply chain.

**Discount vs. Value Analysis**: A Dual-Axis chart (line and clustered column) compares inventory value against the average discount to identify if the most valuable categories are being overly discounted.

<br/>

#### Page 2: Product & Warehouse Deep-Dive

**Exploratory Data Analysis (EDA)**: A scatter plot mapping average product ratings against average review volume, sized by items in stock. This visually pinpoints "liquidation candidates"—products with massive stock but terrible ratings.

**Custom Matrix Table**: Features a rigid drill-down hierarchy (Category → Sub-Category → Name). It integrates dynamic image URLs, allowing warehouse managers to visually confirm product packaging alongside raw stock counts.

<br/>

#### Page 3: Financial & Sales Summary

**Geographical Mapping**: A map visual plotting gross revenue by city, featuring specialized tooltips that reveal revenue generated by specific products in that region.

**Marketing ROI**: A Donut chart tracks total orders by influencer promo codes, proving the percentage of sales driven by paid influencers versus organic traffic.

**Revenue vs. Volume Strategy**: A Dual-axis chart plots gross revenue against total orders, allowing executives to differentiate between high-volume/low-margin and low-volume/high-margin categories.

<br/>

## 🎯 Conclusion & Business Value

This architectural implementation successfully transitions Zepto's analytics from a reliance on static spreadsheets to a dynamic, interactive decision culture. It directly provides real-time oversight of warehouse capital, pinpoints logistics risks proactively, and maps geographical sales performance with absolute data integrity.

<br/>

## 🚀 Future Implementation Roadmap

**Live API Integration**: Transition the ETL pipeline from flat-file ingestion (Excel/CSV) to a direct REST API pipeline connected to Zepto's backend servers, enabling true real-time, zero-latency dashboard refreshing.

**Competitor Web Scraping**: Deploy localized Python web scrapers to extract competitor pricing models, dynamically overlaying them against Zepto's internal discount strategy to optimize margins.

**Predictive Restocking (ML)**: Leverage the isolated Sales dataset to introduce Machine Learning forecasting models directly inside Power BI, predicting demand spikes and automating warehouse procurement.
