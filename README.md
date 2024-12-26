# Hospitality Business Performance Analysis - Power BI Project
![Atliq Project cover](https://github.com/pbisht2105/atliq_hospitality_powerbi_project/blob/main/Atliq_Hospitality_Cover_Image.png)

## Project Overview

This project focuses on analyzing the performance of a hospitality business using **Power BI**. The objective is to track key performance indicators (KPIs) such as revenue, occupancy, booking status, cancellations, customer ratings, and utilization metrics. This analysis aims to provide actionable insights for improving business performance and optimizing room pricing, booking platforms, and customer engagement.

### Project Objective

The goal of this project is to provide a comprehensive analysis of the hospitality business, specifically focusing on:
1. **Revenue**: Understanding the total revenue generated and how various factors (e.g., booking platform, room class, occupancy) influence the business.
2. **Occupancy and Utilization**: Analyzing occupancy rates and room utilization to identify trends and optimize capacity.
3. **Booking Cancellations and No-shows**: Tracking cancellations and no-shows to improve operational efficiency.
4. **Customer Satisfaction**: Understanding customer satisfaction through average ratings and feedback.
5. **Time-based Metrics**: Analyzing week-over-week changes for key metrics to identify growth or decline trends.

## Data Sources

The project uses the following datasets:

1. **fact_bookings**: Contains detailed transactional data about each booking, including revenue, booking status (Cancelled, Checked Out, No Show), and customer ratings.
2. **fact_aggregated_bookings**: Provides aggregated booking data such as successful bookings, capacity, and occupancy metrics across hotels.
3. **dim_rooms**: Contains details about different room types, including room class (Standard, Elite, Premium, etc.).
4. **dim_date**: Includes date-related information (weeks, months, days) for time-based analysis.
5. **dim_hotels**: Contains details about hotels, such as location, establishment type, and other properties.

## Key Performance Indicators (KPIs)

The following KPIs are calculated to measure and track business performance:

### 1. **Total Revenue**
   - Formula: `Revenue = SUM(fact_bookings[revenue_realized])`
   - Description: The total revenue generated from all bookings.

### 2. **Total Bookings**
   - Formula: `Total Bookings = COUNT(fact_bookings[booking_id])`
   - Description: The total number of bookings made across all hotels.

### 3. **Total Capacity**
   - Formula: `Total Capacity = SUM(fact_aggregated_bookings[capacity])`
   - Description: The total available capacity (rooms) in all hotels.

### 4. **Occupancy %**
   - Formula: `Occupancy % = DIVIDE([Total Successful Bookings], [Total Capacity], 0)`
   - Description: The percentage of rooms that were occupied (successful bookings divided by total capacity).

### 5. **Average Rating**
   - Formula: `Average Rating = AVERAGE(fact_bookings[ratings_given])`
   - Description: The average rating given by customers across all bookings.

### 6. **Cancellation %**
   - Formula: `Cancellation % = DIVIDE([Total Cancelled Bookings], [Total Bookings])`
   - Description: The percentage of bookings that were cancelled.

### 7. **No Show Rate %**
   - Formula: `No Show rate % = DIVIDE([Total No Show Bookings], [Total Bookings])`
   - Description: The percentage of bookings where the customer neither attended nor cancelled their booking.

### 8. **ADR (Average Daily Rate)**
   - Formula: `ADR = DIVIDE([Revenue], [Total Bookings], 0)`
   - Description: The average amount paid for rooms sold in a given period.

### 9. **RevPAR (Revenue per Available Room)**
   - Formula: `RevPAR = DIVIDE([Revenue], [Total Capacity])`
   - Description: The revenue generated per available room, helping to assess the pricing strategy.

### 10. **Realization %**
   - Formula: `Realization % = 1 - ([Cancellation %] + [No Show Rate %])`
   - Description: The percentage of bookings that resulted in actual customer stays (i.e., checked-out bookings).

### 11. **Weekly Change Percentages** (WoW)
   - **Revenue WoW Change %:** Formula to calculate the week-over-week revenue change.
   - **ADR WoW Change %:** Formula to calculate the week-over-week change in ADR.
   - **RevPAR WoW Change %:** Formula to calculate the week-over-week change in RevPAR.
   - **DSRN WoW Change %:** Formula to calculate the week-over-week change in Daily Sellable Room Nights (DSRN).
   - **Realization WoW Change %:** Formula to calculate the week-over-week change in Realization Rate.

## Power BI Transformations & Measures

### 1. **Data Import and Transformation**
   - Imported five data sources: `fact_bookings`, `fact_aggregated_bookings`, `dim_rooms`, `dim_date`, and `dim_hotels` into Power BI.
   - Applied data transformations in Power Query to clean, filter, and format the data for analysis. This includes handling missing values, renaming columns, and converting data types where necessary.

### 2. **Data Modeling**
   - Established relationships between the tables, ensuring that dimension tables (`dim_rooms`, `dim_date`, `dim_hotels`) were properly linked to fact tables (`fact_bookings`, `fact_aggregated_bookings`).
   - Used a **star schema** approach for better data analysis and simplified querying.

### 3. **DAX Measures Creation**
   - Created custom DAX measures to calculate the KPIs mentioned above.
   - Examples of DAX formulas used:
     - `Revenue = SUM(fact_bookings[revenue_realized])`
     - `Total Bookings = COUNT(fact_bookings[booking_id])`
     - `Occupancy % = DIVIDE([Total Successful Bookings], [Total Capacity], 0)`
     - `RevPAR = DIVIDE([Revenue], [Total Capacity])`
     - `ADR = DIVIDE([Revenue], [Total Bookings], 0)`
     - `Cancellation % = DIVIDE([Total Cancelled Bookings], [Total Bookings])`
     - `Weekly Change % Measures` for various KPIs using DAX `VAR` and `DIVIDE` functions.
<pre style="overflow:auto; max-height: 300px;">
"Revenue WoW change % = 
Var selv = IF(HASONEFILTER(dim_date[wn]),SELECTEDVALUE(dim_date[wn]),MAX(dim_date[wn]))
var revcw = CALCULATE([Revenue],dim_date[wn]= selv)
var revpw =  CALCULATE([Revenue],FILTER(ALL(dim_date),dim_date[wn]= selv-1))

return

DIVIDE(revcw,revpw,0)-1"

"Occupancy WoW change % = 
Var selv = IF(HASONEFILTER(dim_date[wn]),SELECTEDVALUE(dim_date[wn]),MAX(dim_date[wn]))
var revcw = CALCULATE([Occupancy %],dim_date[wn]= selv)
var revpw =  CALCULATE([Occupancy %],FILTER(ALL(dim_date),dim_date[wn]= selv-1))

return

DIVIDE(revcw,revpw,0)-1"

"ADR WoW change % = 
Var selv = IF(HASONEFILTER(dim_date[wn]),SELECTEDVALUE(dim_date[wn]),MAX(dim_date[wn]))
var revcw = CALCULATE([ADR],dim_date[wn]= selv)
var revpw =  CALCULATE([ADR],FILTER(ALL(dim_date),dim_date[wn]= selv-1))

return

DIVIDE(revcw,revpw,0)-1"

"Revpar WoW change % = 
Var selv = IF(HASONEFILTER(dim_date[wn]),SELECTEDVALUE(dim_date[wn]),MAX(dim_date[wn]))
var revcw = CALCULATE([RevPAR],dim_date[wn]= selv)
var revpw =  CALCULATE([RevPAR],FILTER(ALL(dim_date),dim_date[wn]= selv-1))

return

DIVIDE(revcw,revpw,0)-1"

"Realisation WoW change % = 
Var selv = IF(HASONEFILTER(dim_date[wn]),SELECTEDVALUE(dim_date[wn]),MAX(dim_date[wn]))
var revcw = CALCULATE([Realisation %],dim_date[wn]= selv)
var revpw =  CALCULATE([Realisation %],FILTER(ALL(dim_date),dim_date[wn]= selv-1))

return

DIVIDE(revcw,revpw,0)-1"

"DSRN WoW change % = 
Var selv = IF(HASONEFILTER(dim_date[wn]),SELECTEDVALUE(dim_date[wn]),MAX(dim_date[wn]))
var revcw = CALCULATE([DSRN],dim_date[wn]= selv)
var revpw =  CALCULATE([DSRN],FILTER(ALL(dim_date),dim_date[wn]= selv-1))

return

DIVIDE(revcw,revpw,0)-1"
</pre>


### 4. **Power BI Dashboard Creation**
   - Designed an interactive Power BI dashboard with various visualizations for the analysis.
   - **Key Visualizations**:
     - **Total Revenue**: Line chart showing revenue trends over time.
     - **Occupancy %**: Bar chart displaying occupancy rates by hotel or room class.
     - **Cancellation and No-Show Rates**: Donut charts to visualize the percentage of cancelled or no-show bookings.
     - **ADR and RevPAR**: Bar charts comparing ADR and RevPAR for different hotels/room types.
     - **Room Utilization**: Stacked column chart showing how rooms were utilized (occupied, cancelled, no-show).
     - **Booking Metrics by Platform**: Pie chart showing the contribution of different booking platforms (e.g., MakeMyTrip, LogTrip).
     - **Weekly Change Metrics**: Line charts to track week-over-week changes for key metrics (Revenue, ADR, RevPAR).

### 5. **Dashboard Publishing**
   - Published the Power BI report to the Power BI service for sharing with stakeholders and business users.
   - The dashboard allows users to filter by different dimensions (e.g., hotel, room class, platform, date) to explore detailed insights and make data-driven decisions.

## Steps Taken in the Project

### 1. Data Import and Transformation
   - Imported five datasets into Power BI: `fact_bookings`, `fact_aggregated_bookings`, `dim_rooms`, `dim_date`, `dim_hotels`.
   - Applied necessary transformations in Power Query to clean and structure the data.

### 2. Data Modeling
   - Defined relationships between fact and dimension tables.
   - Created a star schema with dimensions like **dim_rooms**, **dim_date**, and **dim_hotels** linked to fact tables like **fact_bookings** and **fact_aggregated_bookings**.

### 3. DAX Measures Creation
   - Created DAX measures to calculate key metrics such as **Revenue**, **Occupancy %**, **ADR**, **Cancellation %**, **RevPAR**, etc.

### 4. Visualization in Power BI
   - Designed interactive visualizations and a dashboard.
   - Included key charts such as line charts, bar charts, pie charts, and donut charts to visualize trends and insights.

### 5. Dashboard Publishing
   - Published the interactive Power BI report to the Power BI service for easy access and sharing.

# Findings and Conclusion

Based on the analysis of the Atliq Hospitality Dashboard, here are some key findings:
This analysis provides valuable insights into Atliq Hospitality's performance, highlighting key metrics that drive revenue and areas for improvement. By leveraging data visualization and analysis, the company can make informed decisions to optimize operations and enhance profitability.

## Key Insights

### 1. **Revenue Drivers**

- **High Occupancy Rates**: A strong correlation exists between high occupancy rates and increased revenue. Maximizing occupancy should be a key focus for the business.
- **ADR (Average Daily Rate)**: A high ADR indicates that the properties are able to command premium prices. Maintaining and potentially increasing the ADR while maintaining occupancy is crucial.

### 2. **Operational Efficiency**

- **Booking Cancellations and No-Shows**: Analyzing cancellation trends and no-show rates helps in optimizing room utilization. Strategies to minimize cancellations, such as flexible booking policies or loyalty programs, can be implemented.

### 3. **Performance Monitoring**

- **Week-over-Week Changes**: Tracking key metrics on a week-over-week basis allows for the identification of trends and early detection of potential issues. This enables proactive responses to changing market conditions and operational challenges.

### 4. **Revenue by Category**

- **Luxury Segment Dominance**: The luxury segment contributes significantly to revenue. Further analysis can explore strategies to attract more guests in this segment and potentially expand offerings to cater to their specific needs.

### 5. **Platform Performance**

- **Realization and ADR by Platform**: Analyzing performance across different booking platforms can help in identifying the most profitable channels and optimizing online distribution strategies.

## Areas for Further Investigation

- **Competitive Analysis**: Benchmarking performance against competitors can provide valuable insights into market positioning and areas for differentiation.
- **Seasonality and Demand Forecasting**: Analyzing historical data to identify seasonal trends and forecast future demand can help in optimizing pricing, inventory management, and staffing levels.
- **Customer Segmentation and Targeting**: Understanding customer preferences and segmentation can help in developing targeted marketing campaigns and improving customer satisfaction.

By continuously monitoring these key metrics and leveraging data-driven insights, Atliq Hospitality can refine its operations, enhance guest experience, and achieve sustainable growth.
