# Uber Trip Dataset Analysis 

## Table of contents
- [Project Overview](#project-overview)
-	[Dataset Description](#dataset-description)
-	[Data Dictionary](#data-dictionary)
-	[Exploratory Data Analysis](#exploratory-data-analysis)
-	[Data preparation](#data-preparation)
-	[Data modelling](#data-modelling)
-	[Data Analysis](#data-analysis)
-	[Visualization or Dashboards](#visualization-or-dashboards)
-	[Key Insights](#key-insights)
-	[Recommendations](#recommendations)
-	[Challenges and Solutions](#challenges-and-solutions)  
-	[Conclusion](#conclusion)

## Project Overview 
Analyse Uber trip data using Power BI to gain insights into booking trends, revenue, and trip efficiency, helping stakeholders make data-driven decisions.

KPI’s

✔ Total Bookings  – How many trips were booked over a given period?

✔ Total Booking Value – What is the total revenue generated from all bookings?

✔ Total Trip Distance – What is the total distance covered by all trips?

✔ Average Booking Value – What is the average revenue per booking?

✔ Average Trip Distance  – How far are customers traveling on average per trip?

✔ Average Trip Time – What is the average duration of trips?

## Dataset Description
Two datasets were used:
1.	Location Table – Contains location IDs , Location and city.
2.	Trip Details Table – Contains trip ID, Pickup time, Dropoff time, Passenger count, Trip distance, Surge fee, Fare Amount, Vehicle, Payment Type, PULocationID, and
	 DOLocation
3.	Calendar Table was also created from trip dates to enable time intelligence functions.

## Data Dictionary
1.	Location Table:

o	LocationID – Unique identifier for each location.

o	Location – Name of the pick-up or drop-off point.

2.	 Trip Details Table:

o	TripID – Unique trip identifier.

o	Pickup Time – Date and time when the trip started.

o	Dropoff Time – Date and time when the trip ended.

o	PULocationID – Reference to Location Table.

o	DOLocationID – Reference to Location Table.

3.	Calendar Table:
   
o	Date – Full date field.

o	Year, Month, Quarter, Day, Weekday – Derived fields for time analysis.

## Exploratory Data Analysis
EDA involves exploring, examining the data to discover and gain insights, the following are goals to be achieved on this project:

•	Identify trends in ride bookings and revenue generation.

•	analyses trip efficiency in terms of distance and duration.

•	Compare booking values and trip patterns across different time periods.

•	Provide insights to optimize pricing models and improve customer satisfaction.

•	Detecting trends and fluctuations in daily trip volumes.

•	 Identifying peak and off-peak booking days.

•	Understanding the impact of external factors (holidays, events, weather) on ride demand.

•	Supporting strategic planning for resource allocation and pricing adjustments.

## Data preparation
	Converted date fields into proper datetime format.

	Removed duplicates and corrected null values.

	Generated Calendar table using DAX in Power BI.

	Created calculated measures in DAX

## Data modelling
The two datasets were connected in Power BI through relationships:

•	Uber Trip Details → Location Table: Connected via PickupLocationID and DropoffLocationID.

•	Uber Trip Details → Calendar Table: Connected via Pickup Time (date).

This created a star schema where the Locations Table serve as a dimension table, and the Trip Details Table served as a fact table

## Data Analysis  
### Aggregated metrics (total bookings, average trip duration, total trip distance) was created:

o	Total Bookings = COUNT(‘TripsDetails’[TripID])

o	Total Bookings Amount =  SUM('Trip Details'[fare_amount]) +SUM('Trip Details'[Surge Fee])

o	AVG.Booking Amount = DIVIDE([Total Bookings Amount],[Total Bookings],BLANK())

o	Total Trip Distance  = VAR TotalMiles=SUM('Trip Details'[trip_distance])/1000
RETURN
CONCATENATE(FORMAT(TotalMiles, "0"),"k miles")

o	AVG.Trip Distance = VAR AvgMiles=ROUND(AVERAGE('TripDetails'[trip_distance]),0)
RETURN
CONCATENATE(AvgMiles, " miles")

o	AVG.Trip Time =
VAR AvgTime=AVERAGEX('Trip Details',DATEDIFF('Trip Details'[Pickup Time],'Trip Details'[Drop Off Time],MINUTE))
RETURN
CONCATENATE(FORMAT(AvgTime,"0"), "min")

### Location based analysis using mapping visuals.

Created a Measure Selector using a Disconnected Table with the following values:

✔ Total Bookings

✔ Total Booking Value

✔ Total Trip Distance

	
Then, use a measure to dynamically update the visualizations based on user selection.

By Payment Type (Card, Cash, Wallet, etc.)

By Trip Type (Day/Night)

Drill-through details for deeper insight:

Created a grid table (matrix or table visual) to analyze key performance indicators like Total Bookings, Total Booking Value, Avg Booking Value, Total Trip Distance across different Vehicle Types in Uber trips.

### Additional Enhancements:
Ø  Dynamic Title – To update the chart title based on the selected measure.

Ø  Slicers – Added filters for Date, City, and other interactive filters for deeper analysis.

Ø  Tooltips –To show additional details like Average Booking Value or Trip Distance.

### Vehicle Type Analysis - Grid View 

Created a grid table (matrix or table visual) to analyse key performance indicators like Total Bookings, Total Booking Value, Avg Booking Value, Total Trip Distance across different Vehicle Types in Uber trips.

### Time Analysis
To understand trip patterns based on time, Uber needs to analyse ride demand and trends across different time intervals. This dashboard will help in optimizing operations, pricing, and driver availability.

Global Dynamic Measure (Filters All Charts)

A measure selector will be created for:

✔ Total Bookings

✔ Total Booking Value

✔ Total Trip Distance

This dynamic measure will update all visuals based on user selection.

 **By Pickup Time (10-Minute Intervals) - Area Chart**

•	Groups trip bookings into 10-minute intervals throughout the day.

•	Helps in identifying peak and off-peak demand periods.

**By Day Name - Line Chart**

•	Shows booking trends across Monday to Sunday.

•	Useful for analysing weekday vs. weekend demand.

**By Hour and Time - Heatmap (Matrix Grid)**

•	Rows: Hours of the Day (0–23)

•	Columns: Days of the Week (Mon-Sun)

•	Values: Selected Dynamic Measure (e.g., Total Bookings)

•	Highlights peak booking hours across different days.

**Detail Tab:**
A Grid Tab will be created. This tab will enable drill-through functionality, allowing users to access detailed records based on selections made in other dashboards:

Ø  Use a Table or Matrix Visual to display Vehicle Type with the KPIs.

Ø  Apply Conditional Formatting to highlight high and low values.

Ø  Enable Sorting & Filtering for user interaction.

Ø Clear Slicer Button

•	Add a "Clear Filters" button using a blank button with a Reset Slicers action to reset all selections in one click.

## Visualization or Dashboards
Three dashboards were created :

1.	**Overview Dashboard**
   
o	KPIs: Total Trips, Total Duration, Average Trip Time.

o	Map visualization of trips by location.

2.	**Time Analysis Dashboard**
   
o	Trips by Year, Quarter, Month, Day, Hour.

o	Weekday vs Weekend patterns.

3.	**Details Dashboard**
   
o	Drill-through reports for specific trips.

o	Table views with filters for locations and dates.

## Key Insights 
1. Overall Performance  
- 104K total bookings with $1.6M in revenue.  
- Average booking amount: $15.  
- Total trip distance: 349K miles, average of 3 miles per trip.  
- Average trip duration: 16 minutes.  

 2. Payment & Trip Type  
- 66% of trips paid via Uber Pay, showing strong digital adoption.  
- Night trips (60%) generated higher revenue compared to day trips (40%).  

 3. Vehicle Analysis  
- UberX dominates with 38K+ bookings and $0.58M in revenue, covering the highest distance.  
- Other categories (Comfort, Black, XL, Green) each contributed between $0.22M – $0.25M.  
- Uber Green, while eco-friendly, had the lowest share (~14K bookings).  

4. Location Trends  
- Most frequent pickup: Penn Station/Madison Sq West.  
- Most frequent drop-off: Upper East Side North.  
- Longest trip recorded: Lower East Side → Crown Heights North (144 miles).  

5. Time Analysis  
- Peak booking hours: 12 PM – 6 PM (lunch to evening).  
- Low demand hours: 12 AM – 5 AM.  
- Most active days: Saturday (18.7K) and Sunday (19.2K).  
- Lowest activity day: Thursday (9.3K).  

 6. Details Dashboard  
- Trip Distance Distribution: Most trips are within **0–5 miles**, showing short-distance commuting is the primary use case.  
- Revenue per Vehicle Type: UberX leads in total earnings, while Uber Black provides **higher average fares** despite fewer bookings.  
- Pickup & Drop-off Heatmaps: High concentration in **Manhattan** with extended trips going out to boroughs like Brooklyn and Queens.  
- Customer Preference Patterns: Majority of users choose **UberX** for affordability, while a smaller percentage opt for **premium (Uber Black/Comfort)**.  

•	Clear peak hours during mornings and evenings.

•	High concentration of trips around major city hubs.

•	Weekdays showed higher trip demand compared to weekends.

•	Trip durations varied significantly by location pairs.

## Recommendations
1. Fleet Optimization  
   - Focus more vehicles during **peak hours (12 PM – 6 PM)** and **weekends** to maximize earnings.  
   - Balance fleet allocation to avoid oversupply during low-demand hours.  

2. Promotions & Incentives  
   - Offer **weekday promotions** (especially Thursdays) to improve mid-week bookings.  
   - Introduce **night trip loyalty bonuses**, as night trips already drive more revenue.  

3. Location-Based Strategy  
   - Deploy more vehicles around **Penn Station/Madison Sq West** during peak hours.  
   - Improve coverage for long-distance trips like **Lower East Side → Crown Heights**, as these contribute higher fares.  

4. Sustainability Push  
   - Expand **Uber Green** promotions to encourage eco-friendly choices, as adoption is currently low.  

## Challenges and Solutions
•	Challenge: Trip duration missing for some records.

o	Solution: Calculated duration using start and end time.

•	Challenge: Dates not uniform across tables.

o	Solution: Created standardized Calendar table for consistency.

## Conclusion
This analysis provides a clear understanding of Uber’s logistics efficiency, demand behavior, and revenue drivers. With optimized fleet allocation, targeted promotions, and stronger sustainability efforts, Uber can further increase revenue and customer satisfaction.



















