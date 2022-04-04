### PyBer Challenge Analysis
## Overview
# Purpose of Analysis: Deliverable 1
The purpose of this analysis was to take city data and Pyber ride data to create new dataframes and a chart that effectively analyzed fare in each city type (Rural, Suburban and Urban). The first step in this analysis was to break down the data into clear data samples. The data samples we created were ride count, driver count, fare sum, and the average fare by city type. We then got average fare by driver using the following code.
avg_driver_fare = fare_sum / drivers_count
avg_driver_fare
After that we created a new DataFrame to present the data samples we created.
pyber_summary_df = pd.DataFrame({
    "Total Rides": ride_count,
    "Total Drivers": drivers_count,
    "Total Fares": fare_sum,
    "Average Fare per Ride": fare_avg,
    "Average Fare per Driver": avg_driver_fare
})
pyber_summary_df.head()
After creating the DataFrame we needed to format it to show the data in a clean manner.
pyber_summary_df['Total Rides'] = pyber_summary_df['Total Rides'].map('{:,}'.format)
pyber_summary_df['Total Drivers'] = pyber_summary_df['Total Drivers'].map('{:,}'.format)
pyber_summary_df['Total Fares'] = pyber_summary_df['Total Fares'].map('${:,.2f}'.format)
pyber_summary_df['Average Fare per Ride'] = pyber_summary_df['Average Fare per Ride'].map('${:,.2f}'.format)
pyber_summary_df['Average Fare per Driver'] = pyber_summary_df['Average Fare per Driver'].map('${:,.2f}'.format)
pyber_summary_df
<img width="676" alt="Screen Shot 2022-04-04 at 9 49 36 AM" src="https://user-images.githubusercontent.com/99200831/161570371-2d4c20a2-a713-4f10-8e72-1e2b0250fbe8.png">
# Results of Analysis: Deliverable 1
From our clean DataFrame we can see that Urban has by far the most drivers at 2,405 and total fares at $39,854.38, compared to Rural with drivers at 78 and total fares at $4,327.93. This makes sense when you take into consideration the well known fact that more people are in urban areas at a given time than in the rural areas. The other thins we notice looking at the average fare per driver is that urban drivers make significantly less per a ride than rural drivers. Urban drivers make $16.57 per ride and Rural drivers make $55.49 per ride.
# Purpose of Analysis: Deliverable 2
In deliverable 2 we were asked to create a pivot table based on date. Once we had this pivot we were asked to futher narrow it down to dates between 01-10-2019 and 04-28-2019.
jan_apr_fare_per_day = total_fare_per_day_pivot.loc['2019-01-01':'2019-04-28']
jan_apr_fare_per_day.tail()
We then had to change the date index to the datetie datatype so we could resample the data fro daily to weekly data.
jan_apr_fare_per_day.index = pd.to_datetime(jan_apr_fare_per_day.index)
jan_apr_fare_per_day.head()
jan_apr_fare_per_week = jan_apr_fare_per_day.resample("W").sum()
jan_apr_fare_per_week.head()
From here we were able to plot and format our chart to detail weekly fare by city type.
from matplotlib import style
fig, ax = plt.subplots(figsize=(15, 6))

import matplotlib.dates as mdates

ax.plot(jan_apr_fare_per_week)
ax.set_ylabel('Fare ($USD)',fontsize=14)
ax.set_xticks(pd.date_range(start = "2019-01-01", end = "2019-04-30", freq="MS"))
ax.set_title("Total Fare by City Type")
# Make ticks on occurrences of each month:
ax.xaxis.set_major_locator(mdates.MonthLocator())
# Get only the month to show in the x-axis:
ax.xaxis.set_major_formatter(mdates.DateFormatter('%b'))
ax.legend(["Rural","Suburban","Urban"])
# Import the style from Matplotlib.
from matplotlib import style
# Use the graph style fivethirtyeight.
style.use('fivethirtyeight')
# Save the figure.
plt.savefig("/Users/bkieh/Desktop/School/Classwork/Python/PyBer_Analysis/Pyber_Summary.png")
![Pyber_Summary](https://user-images.githubusercontent.com/99200831/161572332-225bbab5-5f38-445f-96dd-40cfa3172e83.png)
# Results of Analysis: Deliverable 2
From the Chart we can see that each city type does not have similar trends to the other cities except in the last weeks of February. In the last weeks of February rides increased. At the end of April both rural and urban rides see a decline while suburban rides seem to increase. Throughout the rest of the year the similarities seem slim.
