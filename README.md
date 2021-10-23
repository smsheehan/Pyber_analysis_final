# PyBer_analysis



## Overview of the analysis: 


The purpose of this analysis was to evaluate ride-sharing data from the fictional ride-share company, PyBer.  Specifically, our fictional CEO was interested in understanding if any business insights could be gleaned through evaluation of the relationships between city type, fares, and number of drivers.  Additionally, since our data contains detailed information about the date of each individual ride, we were asked to evaluate the data temporally over the first trimester of 2019.  From a skills standpoint, this purpose of this challenge was to explore merging two data sets, demonstrate proficiency with utilization of the groupby() function, and to introduce us to two new functions: pivot() and resample(). 

## Results: 

In order to best present the results, I will also describe the process used to obtain the results.  The PyBer data needed for this analysis exists in two separate .csv files and I first needed to merge the files using the code shown below.  It was important to inspect the data first to ensure a) that the two data sets shared a mutual column to be merged upon, and b) that I knew what the columns were named in each file.  Since this analysis is focused on city type data, either a left join or outer join would be appropriate.  I experimented with all of the join options and in this case given the content of the two data sets, they all provided the same exact outcome for our analysis.

![image](https://user-images.githubusercontent.com/90977689/138496682-98ac7b6a-94b6-4f4e-8ca4-e113f2dc41da.png)

Now that the datasets were combined into a single dataframe, grouping the data by city type and using the count() function on ride_id revealed the first insight of this analysis:

![image](https://user-images.githubusercontent.com/90977689/138497015-6140ca34-afde-4cc3-af66-fb77a5ff874b.png)


As we can see from the above image, it is clear that most of PyBer's business, in terms of number of rides, comes from urban cities.  Suburban locations are the next busiest, followed by rural locations.  It is interesting to see that Pyber provides 2.6x more rides in urban vs suburban and 13x more rides in urban vs rural.  

Next, grouping the data by city type and using the sum() function on driver_count elucidated the next insight.  The number of drivers per city type follows a similar trend to that of number of rides.  Urban areas had the most number of drivers, followed by suburban, and then rural.  Urban had 4.9x the number of drivers vs suburban and urban had 30.8x the number of drivers vs rural. The code for this is shown below.  The important thing to note about this code is that you need to go back to the city_data_df to get this information.  If one tries to extract this information from the merged data frame, the numbers retrieved are not correct.  The reason for this is that when the data sets are merged, the driver_count gets populated into every row for each individual ride in a particular city.  For example, the city of Amandaburgh has 12 drivers and 18 rides.  If we ran this code on the merged dataframe, we would get 12x18 drivers as our number of drivers.  Running the count on the original city_data_df solves this problem.

![image](https://user-images.githubusercontent.com/90977689/138514758-a1888676-d66d-46d0-85bd-972e990d4e1a.png)

Our next insight comes from grouping by type on the merged dataframe and using the sum() function on fare.  As expected, the total amount of fare collected is correlated with the number of rides.  Urban fares collected is 2.1x higher than suburban and 9.2x higher than rural.  Interestingly, while this fold difference is directionally similar to our evaluation of number of rides, it is not exactly the same.  The code and results for this are shown below:

![image](https://user-images.githubusercontent.com/90977689/138516277-13799e4b-e13c-441c-a0cc-cfa03a383ec5.png)


Our observation from above highlights the need to understand any differences in fare per ride in the different city types.  Simply dividing total_fares by total_rides provides us with this information.  This shows us that the fare per ride is highest in rural areas, followed by suburban, and then shows urban with the lowest fare per ride.  This makes sense since places in rural areas tend to be further apart in distance than places in more densely populated areas.  The code and results are shown below:

![image](https://user-images.githubusercontent.com/90977689/138517094-fac76fa4-de3d-40de-ba36-f1a5e7f085d1.png)


The next calculation was get the average fare per driver for each city type.  This was done by taking the total_fares and dividing by total_drivers.  Again we see that the fare per driver is highest in rural areas, followed by suburban, and then by urban.  I would caution about using this information since our earlier evaluation of the data shows that in urban cities, there were many more drivers than there were actual rides.  This calculation is including the drivers that did not have any passengers so would only be relevant if PyBer was paying all drivers an hourly wage.  As we know, the current model for ride-share does not pay drivers who do not pick up passengers.  If we only focused on the drivers that collected fares, the trend would be the same but the number for urban would be a bit higher.  The code and results are shown below:

![image](https://user-images.githubusercontent.com/90977689/138518129-afdc357d-8681-4e9f-8d0f-09f3800cdb13.png)

All of this analysis was then pulled together in a summary dataframe and formatted with straightforward code to provide the table shown below:

![image](https://user-images.githubusercontent.com/90977689/138518481-22a41066-58c4-43a9-885a-57727d640052.png)

The next part of the challenge required the evaluation of the fare by city type data over the course of the first trimester of 2019 and to provide this information in a graph.  First I created a new dataframe grouped by date and type and applied the sum() function on fare.  I then reset the index using the reset_index() function as shown below:

![image](https://user-images.githubusercontent.com/90977689/138519259-49366641-2e48-4d75-8e64-7a5aaead549a.png)

This then enabled me to create a pivot table using the pivot() function, setting the date as the index, type as the columns, and fare as the values.  This was done as shown below:

![image](https://user-images.githubusercontent.com/90977689/138519425-27967d96-3c56-416a-b487-bb1631f258b1.png)

I then created a new dataframe based on the desired date range as shown:

![image](https://user-images.githubusercontent.com/90977689/138519583-f574fa1b-dcdb-42cb-bb71-00c4ce9037cf.png)

Then I converted the date index into the needed datetime datatype using the pd.to_datetime() function and then created a new data frame by using the resample() function to convert to weeks followed by the sum() function to sum the fares into the weeks.

![image](https://user-images.githubusercontent.com/90977689/138519973-8bee32a7-38ce-4d24-95c6-6ee2ce744b64.png)

![image](https://user-images.githubusercontent.com/90977689/138520049-52871db0-f307-4a35-af75-2074b4179418.png)

Having the requisite dataframe created, I was now able to create the chart:

![image](https://user-images.githubusercontent.com/90977689/138520242-133d7ac3-0f03-462c-b7f5-56826e137f90.png)

This analysis reveals that while there are fluctuations in the data over time, there are no time points where the trends we saw in our earlier analyses are broken.  All three city types experienced an uptick in fares on the February 24th timepoint, but there is not enough contextual information to read anything into this.  Both the urban and suburban data have a fluctuation range of about $750 dollars while the rural data shows a little less variability with a fluctuation range of around $400.





## Summary: 

Three business recommendations based on the above analysis are as follows:

### 1. No new investment in hiring additional drivers for Urban cities

At least until you analyze the factors that have led to having so many drivers who did not have a single ride. Are these drivers working enough hours?  As we know, drivers have the ability to decide when and how much they work.  If these "excess drivers" are not working much and thus not available to potential customers, this may signal continued opportunity to grow revenue if we had more available drivers.  If these drivers are in fact available often and just not getting work, this may signal market saturation and a potential opportunity to invest in consumer based marketing to grow revenue.

### 2. Analyze the market demographics of suburban and rural areas

Since in these regions, we know that every driver is getting fares in these city types, some additional analysis and data would be helpful to direct our next steps.  As we know, utilization of ride share services is heavily weighted toward riders of a particular age range.  Understanding what the makeup of these areas are can help us understand whether there is opportunity for additional growth in these areas through the additional hiring of drivers.  Additionally, I would recommend sending a survey to existing customers in this area to understand what factors are most important to them in choosing PyBer.  It could be that wait time is a significant factor in decision of whether or not to use our platform.  If this is the case we would want to further analyze the average wait time for these areas.  A higher density of drivers would reduce wait time and potentially increase revenue.

### 3. Run a targeted experiment in a rural area.

Invest in a campaign to attract more drivers in a targeted rural area and then monitor to see if this leads to an increased number of rides.  This would help provide additional context to the survey and wait time analysis we recommended above.
