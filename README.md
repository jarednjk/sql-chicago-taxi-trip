# Introduction

🚕 The [Chicago Taxi Trip](https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=chicago_taxi_trips&page=dataset) public dataset from BigQuery is a collection of data related to taxi trips taken in the city of Chicago, Illinois. The dataset contains information on over 100 million taxi trips, spanning from 01/01/2013 to 31/12/2023. It includes details such as 📍 pickup and drop-off locations, 🛣️ trip distances, 💲 fares, 💳 payment types, ⏱️ timestamps, and 🏢 company names.

### The questions I wanted to answer through my SQL queries were:

1. Which are the top performing taxi companies by revenue and how much did they make?
2. Which taxi companies made the most trips?
3. How much has the average total cost of a taxi trip increased over the last decade?
4. Which are the cheapest and most expensive time of the year to take a taxi?
5. Where are the most popular pickup and dropoff places?

Check out my data visualisation dashbboard using Tableau [here](https://public.tableau.com/app/profile/jarednjk/viz/ChicagoTaxiTripsDataVisualisationA4portrait/Dashboard2).

Check out my data presentation deck using Google Slides [here](https://docs.google.com/presentation/d/e/2PACX-1vQmT1Kpi6viLmYYBEDmf6NdQR7DEeEmAysLSxI8vhVAHG0SkBNb4551DcDjodegUYNQanNzVAYRliTx/pub?start=false&loop=false&delayms=3000).

# Tools I Used

For my deep dive into the Chicago taxi trip public dataset, I harnessed the power of several key tools:

- **SQL:** The backbone of my analysis, allowing me to query the database and unearth critical insights.
- **BigQuery:** The cloud-based big data analytics web service for processing very large read-only data sets.
- **Git & GitHub:** Essential for version control and sharing my SQL scripts and analysis, ensuring collaboration and project tracking.
- **Tableau:** The data visualisation tool used for data analysis and business intelligence to share my insights via a dashboard.

# The Analysis

Each query for this project aimed at investigating specific aspects of the Chicago taxi trip dataset. Here’s how I approached each question:

### 1. Top Performing Taxi Companies by Revenue

To identify the companies with the highest revenue, I filtered by excluding company with null values, summing the fare for each company, and ordering them by the highest revenue.

```
SELECT
    company,
    ROUND(SUM(fare),2) AS revenue
FROM
    `bigquery-public-data.chicago_taxi_trips.taxi_trips`
WHERE
    company IS NOT NULL
GROUP BY
    company
ORDER BY
    revenue DESC
LIMIT
    10
```

![Top 10 companies by revenue](https://github.com/jarednjk/sql-chicago-taxi-trip/blob/main/results/top_10_companies_by_revenue.png)

### 2. Taxi Companies with the Most Trips

To identify the companies with the most trips, I filtered by excluding company with null values, counting the total trips made for each company, and ordering them by the highest total trips made.

```
SELECT
    company,
    COUNT(*) AS total_trips
FROM
    `bigquery-public-data.chicago_taxi_trips.taxi_trips`
WHERE
    company IS NOT NULL
GROUP BY
    company
ORDER BY
    total_trips DESC
LIMIT
    10
```

![Top 10 companies by most trips](https://github.com/jarednjk/sql-chicago-taxi-trip/blob/main/results/top_10_companies_most_trips.png)

### 3. Increase in Cost of Taxi Over the Last Decade

To identify the increase in taxi cost over the last decade, I filtered by averaging the cost of each taxi trip according to the year.

```
SELECT
    EXTRACT(YEAR from DATE_TRUNC(CAST(trip_start_timestamp AS DATE), YEAR)) AS year,
    ROUND(AVG(trip_total),2) AS average_taxi_trip
FROM
    `bigquery-public-data.chicago_taxi_trips.taxi_trips`
GROUP BY
    year
ORDER BY
    year
```

![Average cost of taxi trip by year](https://github.com/jarednjk/sql-chicago-taxi-trip/blob/main/results/avg_cost_taxi_trip_by_year.png)

### 4. Cheapest and Most Expensive Months to Take a Taxi

To identify the cheapest and most expensive months to take a taxi, I filtered by averaging the cost of each taxi trip according to the month.


```
SELECT
  EXTRACT(MONTH from DATE_TRUNC(CAST(trip_start_timestamp AS DATE), MONTH)) AS month_number,
  FORMAT_DATETIME("%B", DATE_TRUNC(CAST(trip_start_timestamp AS DATE), MONTH)) AS month,
  ROUND(AVG(trip_total),2) AS average_taxi_trip
FROM
    `bigquery-public-data.chicago_taxi_trips.taxi_trips`
GROUP BY
    month,
    month_number
ORDER BY
    month_number
```

![Average cost of taxi trip by month](https://github.com/jarednjk/sql-chicago-taxi-trip/blob/main/results/avg_cost_taxi_trip_by_month.png)

### 5. Most Popular Pickup and Dropoff Places

To identify the most popular pickup and dropoff places, I filtered by excluding pickup and dropoff community areas with null values, counting the total trips made for each community area, and ordering them by the highest total trips made.

```
SELECT
    pickup_community_area,
    COUNT(*) as trip_count
FROM
    `bigquery-public-data.chicago_taxi_trips.taxi_trips`
WHERE
    pickup_community_area IS NOT NULL
GROUP BY
    pickup_community_area
ORDER BY
    trip_count DESC
LIMIT
    10
```

![Top 10 pickup community area](https://github.com/jarednjk/sql-chicago-taxi-trip/blob/main/results/top_10_pickup_locations.png)

```
SELECT
    dropoff_community_area,
    COUNT(*) AS trip_count
FROM
    `bigquery-public-data.chicago_taxi_trips.taxi_trips`
WHERE
    dropoff_community_area IS NOT NULL
GROUP BY
    dropoff_community_area
ORDER BY
    trip_count DESC
LIMIT
    10
```

![Top 10 dropoff community area](https://github.com/jarednjk/sql-chicago-taxi-trip/blob/main/results/top_10_dropoff_locations.png)

# What I Learned

Throughout this adventure, I've turbocharged my SQL toolkit with some serious firepower:

- **🕰️ Complex Datetime Conversion:** Devleoped strong mastery in datetime conversion in SQL, enabling me to efficiently handle and manipulate date and time data for more accurate and timely reporting.
- **🔢 Data Aggregation:** Got cozy with GROUP BY and turned aggregate functions like SUM(), COUNT(), ROUND(), and AVG() into my data-summarizing sidekicks.
- **💡 Analytical Wizardry:** Leveled up my real-world puzzle-solving skills, turning questions into actionable, insightful SQL queries.
- **📊 Dashboard Mastery:** Enhanced my skills in Tableau, allowing me to create more complex visualizations and insightful data dashboards to improve data-driven decision-making processes.

# Conclusions

### Insights

From the analysis, several general insights emerged:

1. **Top Performing Taxi Companies by Revenue**: Flash Cab has the highest revenue at $141 million.
2. **Most popular Taxi Company**: Flash Cab is the most popular taxi company with over 10 million trips made.
3. **Increase in Cost of Taxi Over the Last Decade**: The average cost of taking a taxi has risen significantly over the years by 64% from $15.14 in 2013 to $24.83 in 2023. The reasons could be due to inflation, rising oil prices, and a shortage of taxi drivers.
4. **Cheapest and Most Expensive Months to Take a Taxi**: October is the most expensive month to take a taxi while December is the cheapest. December to February could be cheaper due to Winter where locals travel away, resulting in a low demand for taxis. April to June could be more expensive due to Spring where tourism is at its peak, resulting in high demand for taxis. September to November could be more expensive due to Autumn where it's the most stable period of the year with less rain and the second favorite time for tourists to visit.
5. **Most Popular Pickup and Dropoff Places**: Community area 8 is the most popular pickup and dropoff point. Taxi companies can send more taxis to these locations to optimise their operations and reduce customers’ waiting time.
