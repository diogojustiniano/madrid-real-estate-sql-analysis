\# Madrid Real Estate Market Analysis (SQL \+ Python)

\#\# Project Overview  
Comprehensive analysis of 20,355 Madrid properties using \*\*SQL queries\*\* and Python to identify investment opportunities, pricing patterns, and market insights. This project demonstrates advanced SQL skills including joins, subqueries, aggregations, and complex calculations.

\#\# Key Technologies  
\- \*\*SQL (SQLite)\*\* \- Primary analysis tool  
\- \*\*Python\*\* \- Data preprocessing and visualization  
\- \*\*Pandas\*\* \- Data manipulation and SQL integration  
\- \*\*Matplotlib\*\* \- Data visualization

\#\# Dataset  
\- \*\*Source:\*\* Kaggle \- Madrid Real Estate Market  
\- \*\*Records:\*\* 20,355 properties  
\- \*\*Period:\*\* 2019 data  
\- \*\*Features:\*\* 57 columns including prices, location, size, amenities

\#\# SQL Analysis Performed

\#\#\# 1\. Neighborhood Price Analysis  
\*\*SQL Techniques:\*\* GROUP BY, aggregate functions, HAVING clause

\*\*Query:\*\*  
\`\`\`sql  
SELECT   
    subtitle AS neighborhood,  
    COUNT(\*) AS property\_count,  
    ROUND(AVG(buy\_price), 2\) AS avg\_buy\_price,  
    ROUND(AVG(buy\_price\_by\_area), 2\) AS avg\_price\_per\_sqm  
FROM properties  
WHERE buy\_price IS NOT NULL AND buy\_price \> 0  
GROUP BY subtitle  
HAVING COUNT(\*) \>= 10  
ORDER BY avg\_buy\_price DESC  
\`\`\`

\*\*Key Findings:\*\*  
\- \*\*Most expensive:\*\* Recoletos (€2.15M average, €8,779/m²)  
\- \*\*Price range:\*\* €100k \- €2.1M across neighborhoods  
\- \*\*Premium areas:\*\* Salamanca, Chamartín, El Viso, Retiro, Chamberí

\---

\#\#\# 2\. Property Features Impact Analysis  
\*\*SQL Techniques:\*\* CASE statements, multiple GROUP BY, conditional aggregation

\*\*Query:\*\*  
\`\`\`sql  
SELECT   
    CASE WHEN has\_lift \= 1 THEN 'Has Lift' ELSE 'No Lift' END AS lift\_status,  
    CASE WHEN has\_parking \= 1 THEN 'Has Parking' ELSE 'No Parking' END AS parking\_status,  
    COUNT(\*) AS property\_count,  
    ROUND(AVG(buy\_price), 2\) AS avg\_price  
FROM properties  
WHERE buy\_price IS NOT NULL AND has\_lift IS NOT NULL AND has\_parking IS NOT NULL  
GROUP BY has\_lift, has\_parking  
ORDER BY avg\_price DESC  
\`\`\`

\*\*Key Findings:\*\*  
\- \*\*Lift premium:\*\* \+226% (€458k more expensive)  
\- \*\*Parking premium:\*\* \+117% (€548k more expensive)  
\- Combined features command highest prices  
\- Critical amenities in high-rise, urban Madrid

\---

\#\#\# 3\. Best Value Properties (Subquery \+ JOIN)  
\*\*SQL Techniques:\*\* Subqueries, INNER JOIN, calculated fields

\*\*Query:\*\*  
\`\`\`sql  
SELECT   
    p.subtitle AS neighborhood,  
    p.buy\_price,  
    ROUND(((p.buy\_price \- neighborhood\_avg.avg\_price) / neighborhood\_avg.avg\_price) \* 100, 1\) AS discount\_pct  
FROM properties p  
INNER JOIN (  
    SELECT subtitle, AVG(buy\_price) AS avg\_price  
    FROM properties  
    WHERE buy\_price IS NOT NULL  
    GROUP BY subtitle  
) AS neighborhood\_avg ON p.subtitle \= neighborhood\_avg.subtitle  
WHERE p.buy\_price \< neighborhood\_avg.avg\_price  
ORDER BY discount\_pct ASC  
\`\`\`

\*\*Key Findings:\*\*  
\- Identified properties priced below neighborhood averages  
\- Discounts range from 10-90% below average  
\- Critical insight: Extreme discounts often indicate missing features or data errors

\---

\#\#\# 4\. Rental Yield Analysis (Advanced Calculations)  
\*\*SQL Techniques:\*\* Complex calculations, percentage formulas, investment metrics

\*\*Query:\*\*  
\`\`\`sql  
SELECT   
    subtitle AS neighborhood,  
    ROUND(AVG(buy\_price), 0\) AS avg\_buy\_price,  
    ROUND(AVG(rent\_price) \* 12, 0\) AS avg\_annual\_rent,  
    ROUND((AVG(rent\_price) \* 12 / AVG(buy\_price)) \* 100, 2\) AS rental\_yield\_pct  
FROM properties  
WHERE buy\_price \> 0 AND rent\_price \> 0  
GROUP BY subtitle  
HAVING COUNT(\*) \>= 5  
ORDER BY rental\_yield\_pct DESC  
\`\`\`

\*\*Key Findings:\*\*  
\- \*\*Best rental yield:\*\* San Cristóbal (6.38% \- excellent for Madrid)  
\- \*\*Investment profile:\*\* €102k buy price, €545/month rent, 15.7yr payback  
\- \*\*Market insight:\*\* Inverse relationship between property price and rental yield  
\- Affordable neighborhoods offer better cash flow returns

\---

\#\#\# 5\. Feature Value Comparison (UNION ALL)  
\*\*SQL Techniques:\*\* UNION ALL, conditional aggregation with CASE

\*\*Query:\*\*  
\`\`\`sql  
SELECT 'Has Lift' AS feature,  
    AVG(CASE WHEN has\_lift \= 1 THEN buy\_price END) AS with\_feature,  
    AVG(CASE WHEN has\_lift \= 0 THEN buy\_price END) AS without\_feature  
FROM properties  
WHERE buy\_price IS NOT NULL

UNION ALL

SELECT 'Has Parking' AS feature,  
    AVG(CASE WHEN has\_parking \= 1 THEN buy\_price END) AS with\_feature,  
    AVG(CASE WHEN has\_parking \= 0 THEN buy\_price END) AS without\_feature  
FROM properties  
WHERE buy\_price IS NOT NULL  
\`\`\`

\*\*Key Findings:\*\*  
\- Quantified exact price premium for each amenity  
\- Lift and parking are most valuable features in Madrid  
\- Data-driven feature prioritization for investors

\---

\#\# Visualizations

\!\[Rental Yields Top 10\](madrid\_rental\_yields\_top10.png)  
\*Top 10 neighborhoods by rental yield \- investment hotspots\*

\!\[Price vs Yield Scatter\](madrid\_price\_vs\_yield\_scatter.png)  
\*Inverse relationship: expensive areas \= low yields, affordable areas \= high yields\*

\!\[Features Price Impact\](madrid\_features\_price\_impact.png)  
\*Quantified price premium for key property features\*

\---

\#\# SQL Skills Demonstrated

\#\#\# Core SQL Concepts  
\- ✅ SELECT, FROM, WHERE, GROUP BY, HAVING, ORDER BY  
\- ✅ Aggregate functions: COUNT(), AVG(), SUM(), MIN(), MAX()  
\- ✅ ROUND() for formatting numeric results  
\- ✅ AS aliases for columns and tables  
\- ✅ LIMIT for result pagination

\#\#\# Advanced SQL Techniques  
\- ✅ \*\*Subqueries\*\* \- Nested SELECT statements  
\- ✅ \*\*INNER JOIN\*\* \- Combining tables  
\- ✅ \*\*CASE WHEN\*\* \- Conditional logic  
\- ✅ \*\*UNION ALL\*\* \- Combining multiple queries  
\- ✅ \*\*Conditional aggregation\*\* \- AVG(CASE WHEN...)  
\- ✅ \*\*Complex calculations\*\* \- Percentage formulas, ratios  
\- ✅ \*\*Multi-column GROUP BY\*\* \- Analyzing combinations  
\- ✅ \*\*HAVING vs WHERE\*\* \- Pre vs post-aggregation filtering

\#\#\# Business Analytics  
\- ✅ Investment analysis (ROI, yield, payback period)  
\- ✅ Comparative analysis (with vs without features)  
\- ✅ Market segmentation (by neighborhood, features)  
\- ✅ Value identification (below-average pricing)  
\- ✅ Data quality assessment (identifying outliers)

\---

\#\# Key Business Insights

\#\#\# Investment Recommendations  
1\. \*\*High-yield opportunities:\*\* San Cristóbal, southern districts (6-7% yields)  
2\. \*\*Feature priorities:\*\* Lift and parking add most value  
3\. \*\*Market segmentation:\*\* Luxury areas for appreciation, affordable areas for cash flow  
4\. \*\*Risk assessment:\*\* Properties with extreme discounts require due diligence

\#\#\# Market Characteristics  
\- Strong price stratification by neighborhood (€100k \- €2.1M range)  
\- Urban amenities (lift, parking) command significant premiums  
\- Rental yields inversely correlated with purchase price  
\- Central/premium areas: low yields, high capital growth potential  
\- Peripheral areas: high yields, steady rental income

\---

\#\# Methodology

1\. \*\*Data Import:\*\* Loaded 20,355 property records into Python DataFrame  
2\. \*\*Database Creation:\*\* Created SQLite database for SQL analysis  
3\. \*\*Data Cleaning:\*\* Filtered NULL values, invalid prices, outliers  
4\. \*\*SQL Analysis:\*\* Executed 5 complex queries covering different aspects  
5\. \*\*Validation:\*\* Cross-referenced findings with Madrid market knowledge  
6\. \*\*Visualization:\*\* Created charts to communicate insights  
7\. \*\*Documentation:\*\* Comprehensive README with SQL code examples

\---

\#\# Limitations & Context

\- \*\*Data vintage:\*\* 2019 data (pre-COVID market conditions)  
\- \*\*Purpose:\*\* SQL skills demonstration, not current investment advice  
\- \*\*Missing data:\*\* Some features have incomplete records  
\- \*\*Outliers:\*\* Extreme values identified and handled appropriately

\---

\#\# Files in Repository

\- \`madrid\_real\_estate\_sql\_analysis.ipynb\` \- Complete analysis notebook  
\- \`madrid\_rental\_yields\_top10.png\` \- Rental yield visualization  
\- \`madrid\_price\_vs\_yield\_scatter.png\` \- Price vs yield relationship  
\- \`madrid\_features\_price\_impact.png\` \- Feature impact analysis  
\- \`README.md\` \- This documentation

\---

\#\# Skills Highlighted

\- \*\*SQL proficiency\*\* \- Complex queries, joins, subqueries  
\- \*\*Data analysis\*\* \- Pattern recognition, outlier detection  
\- \*\*Business acumen\*\* \- Investment metrics, market insights  
\- \*\*Python integration\*\* \- pandas, SQLite, matplotlib  
\- \*\*Local knowledge\*\* \- Madrid real estate market context  
\- \*\*Data storytelling\*\* \- Translating queries into insights

\---

\#\# Author  
Diogo Justiniano Pinto    
\[LinkedIn\](https://www.linkedin.com/in/diogojustiniano/)

\---

\#\# Note on Data Currency  
This analysis uses 2019 data for SQL technique demonstration. Madrid's real estate market has evolved significantly since then. For current investment decisions, consult updated market data and professional advisors.  
