# Airline-Flights-Analysis

# Overall Insights from Airline Dataset Analysis

1. Pricing Patterns- Across all airlines, the cheapest routes are consistently Delhi → Hyderabad and Mumbai →
Chennai, while the most expensive are Delhi → Bangalore and Mumbai → Kolkata.- Non-stop flights are generally cheaper than flights with one or more stops.- Air India offers the highest-priced tickets, while AirAsia is the most budget-friendly carrier overall.
2. High-Value Routes- Premium-priced routes include Bangalore → Mumbai ( 40,354), Delhi → Chennai ( 35,145), and
Kolkata → Delhi ( 34,944).- These routes present opportunities for targeted marketing, loyalty programs, or premium cabin
promotions.
3. Booking Behavior & Timing- Early bookings (11+ days in advance) average around 5,680 — significantly lower than
last-minute bookings (≤3 days), which average 13,502. Price gap exceeds 7,800.- Morning (38,235), Evening (38,121), and Early Morning (38,081) departures are the busiest slots.- Night and Late Night flights have the lowest traffic, suggesting potential for discounted fares or
cargo operations.
4. Travel Time Insights- Fastest routes: Mumbai → Hyderabad, Bangalore → Mumbai, Bangalore → Hyderabad.- Longest durations: Delhi → Bangalore, Delhi → Chennai, Kolkata → Mumbai.
5. Airline Coverage- Most airlines serve 23 unique city pairs, except SpiceJet which serves 20, indicating slightly less
 market penetration.

# Business Recommendations

- Promote Early Booking Campaigns: Highlight cost savings of 7K+ to attract price-sensitive customers.

- Target Premium Routes: Use loyalty programs and exclusive offers for Bangalore–Mumbai, Delhi–Chennai, and Kolkata–Delhi travelers.

- Optimize Scheduling: Allocate more operational resources during Morning, Evening, and Early Morning peaks; explore cargo/low-fare sales for night slots.

- Leverage Non-stop Pricing: Market direct flights aggressively as both cheaper and faster on select routes.

- Expand SpiceJet's Coverage: Closing the route gap could improve competitiveness.


# SQL QUERY

Q1. Cheapest Airline for Each Route
→ Helps customers and platform pricing team identify budget-friendly airlines.

```
select
	airline,
    source_city,
    destination_city,
    min(price),
    dense_rank() over(partition by airline order by min(price) asc) as rn
from airline_data
group by 1,2,3
order by 1, 4 ;
```

# Observation
Air_India - cheapest route is Mumbai - Chennai and Delhi - hyderabad and expensive route is Delhi - bangalore and Mumbai - kolkata

AirAsia - cheapest route is Mumbai - Chennai and Delhi - Chennai and Expensive route is Delhi - Bangalore and Mumbai - Kolkata

Go_First - cheapest route is Delhi - Mumbai and Mumbai - Banglore and Expensive route is Delhi - bangalore and Mumbai - Kolkata

Indigo - cheapest route is Mumbai - Chennai and Delhi - Chennai and Expensive Route is Mumbai - Kolkata and Delhi - bangalore

spicejet - cheapest route is Delhi - Hyderabad and Mumbai - bangalore And Expensive Route is Mumbai - Kolkata

Vistara - cheapest route is Delhi - hyderbad and delhi - Chennai and Expensive route is Delhi - Kolkata and Mumbai - Kolkata  

# Insight
Cheapest route for every airline is Delhi - Hyderabad and Mumbai - Chennai
Expensive Route for Every Airline is Delhi - Bangalore and Mumbai - Kolkata

-- ----------------------------------------------------------------------------------------

Q2. Price Impact of Stops
See if non-stop flights really save customers money.

```
select distinct  stops from airline_data;

select stops , avg(price) as average_price
from airline_data
group by 1;
```

Yes, Zero Stop flights avg_price is  cheaper then one stop and two_or_more

-- ------------------------------------------------------------------------------------------------------

Q3. Top 5 Most Expensive Routes
→ Focus marketing on these premium routes.

```
Select 
	source_city,
    destination_city,
    max(price) as highest_price
from airline_data
group by source_city, destination_city
order by highest_price desc
limit 5;
```

Bangalore to	Mumbai price 40354
Delhi to	Chennai price 35145
Kolkata to Delhi price 34944
Mumbai to Bangalore price 34188
Bangalore to Delhi price 34158

-- -------------------------------------------------------------------------------------------------------------------------------------

Q4. Price Change Trend by Days Left
→ Verify last-minute booking surge.

```
select
	days_left,
    round(avg(price) ,2) as avg_price
from airline_data
group by 1;
```

Last Minute Booking average ticket price is higher than advance booking  

-- -------------------------------------------------------------------------------------------------------------- 
    
Q5. Price Comparison Between Classes
→ Show value gap between Economy & Business.

```
select
	class,
    avg(price) as avg_price
from airline_data
group by 1;
```	

-- -------------------------------------------------------------------------------------------------------------- 

Q6. Busiest Departure Times
→ Help airports manage crowding.

```
select
	departure_time,
    count(*) as Total_flight
from airline_data
group by departure_time
order by Total_flight desc;
```

Insight
Morning Flights are the busiest time with 38,235 departure 
closely followed by Evening (38,121) and Early_Morning (38081)
Night and Late_Night is very less busiest , indicating reduced demand during these hours.

-- --------------------------------------------------------------------------------------------------------------------

Q7. Fastest Flight Options for Each Route
→ Improve recommendations for time-conscious travelers.

```
select 
	source_city, 
    destination_city, 
    min(duration) as min_duration
from airline_data
group by 1,2
order by 3;
```

insights
Mumbai -> Hyderabad, Bangalore -> Mumbai, bangalore -> Hyderabad are fastest Flight  
Delhi -> Bangalore, Delhi -> Chennai, Kolkata -> Mumbai is take more time to reach the destination

-- -----------------------------------------------------------------------------------------------------------------------

Q8. Price Ranges per Airline
→ Identify high and low-cost carriers.

```
select
	airline,
    max(price) as high_price,
    min(price) as low_price
from airline_data
group by 1
order by 3 ;
```

insights
AirIndia airline have the most expensive ticket price
AirAsia airline have the cheapest ticket price  

-- ----------------------------------------------------------------------------------------------------------------------------

Q9. Booking Window Categories
→ Segment price averages for last-minute, medium, and early bookings.

```
SELECT
	CASE WHEN days_left <= 3 THEN "Last-minute"
		 WHEN days_left BETWEEN 4 AND 10 THEN 'Medium'
         ELSE 'Early booking'
	END AS Booking_category,
    avg(price) as average_price
from airline_data
group by Booking_category
order by average_price;
```

Insight
Early_booking (5,680) average price is very low and affordable for everyone , closely followed by medium booking (10,683)
Last-minute (13,502) booking average price is very high and expensive 

-> that means Early booking is good for middle class people 
-> average price difference for Early_booking and last_minute booking is 7800+

-- ---------------------------------------------------------------------------------------------------------------------- 

Q10. Airlines with Maximum Route Coverage
→ Identify the most versatile carriers.

```
select
	airline,
    count(distinct CONCAT(source_city, '-', destination_city)) as total_flight
from airline_data
group by 1
order by  total_flight desc;-- 
```

insights
Every airline cover same number of Route Coverage 23 except spicejet which cover only 20 Route 

-- ---------------------------------------------------------------------------------------------------------------

# Overall Insights from Airline Dataset Analysis

-- 1. Pricing Patterns
-- Across all airlines, the cheapest routes are consistently Delhi → Hyderabad and Mumbai → Chennai, while the most expensive are Delhi → Bangalore and Mumbai → Kolkata.
-- Non-stop flights are generally cheaper than flights with one or more stops, contradicting the common belief that direct flights are always more expensive.
-- Air India offers the highest-priced tickets, while AirAsia is the most budget-friendly carrier overall.

-- 2. High-Value Routes
-- Premium-priced routes include Bangalore → Mumbai (₹40,354), Delhi → Chennai (₹35,145), and Kolkata → Delhi (₹34,944).
-- These routes present opportunities for targeted marketing, loyalty programs, or premium cabin promotions.

-- 3. Booking Behavior & Timing
-- Early bookings (11+ days in advance) have an average ticket price around ₹5,680 — significantly lower than last-minute bookings (≤3 days), which average ₹13,502.
--    → Price gap exceeds ₹7,800, confirming a strong last-minute price surge.
-- Morning (38,235), Evening (38,121), and Early Morning (38,081) departures are the busiest slots, requiring higher airport and ground staff allocation.
-- Night and Late Night flights have the lowest traffic, which could be leveraged for discounted fares or cargo operations.

-- 4. Travel Time Insights
-- Fastest routes include Mumbai → Hyderabad, Bangalore → Mumbai, and Bangalore → Hyderabad.
-- Longest durations are seen in Delhi → Bangalore, Delhi → Chennai, and Kolkata → Mumbai, suggesting possible indirect routings or congested airspace.

-- 5. Airline Coverage
-- Most airlines serve 23 unique city pairs, Except SpiceJet, which serves only 20, indicating slightly less market penetration.

-- 💡 Business Recommendations

-- Promote Early Booking Campaigns: Highlight cost savings of ₹7K+ to attract price-sensitive customers.
-- Target Premium Routes: Use loyalty programs and exclusive offers for Bangalore–Mumbai, Delhi–Chennai, and Kolkata–Delhi travelers.
-- Optimize Scheduling: Allocate more operational resources during Morning, Evening, and Early Morning peaks; explore cargo/low-fare sales for low-demand night slots.
-- Leverage Non-stop Pricing: Market direct flights aggressively as both cheaper and faster on select routes.
-- Network Expansion for SpiceJet: Closing the gap in route coverage could improve competitiveness. 
