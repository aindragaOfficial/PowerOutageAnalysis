# Introduction
I chose to do analysis on a dataset of power outages. The question I decided to answer was whether the proportion of major power outages were similar across different NERC regions. 
This is an important question because it allows us to understand the lop-sidedness of major power outages and bring us to further question where exactly we can narrow the focus
of certain issues. There are 1534 rows and 13 relevant columns.

# Cleaning and EDA (Exploratory Data Analysis)
 The major cleaning steps I took revolved around understanding my question and selecting the columns that I potentially needed. There was a surplus of columns that other columns could potentially 
 hold information for. For example, I had a year and month column that I dropped because I could hold that information in a datetime object in outage restoration/start columns. The percent columns
 were also easily calculable by the vectorized operations pandas has and allowed me to reduce the dimensions of the columns significantly. This also led me to changing certain column datatypes such
 as the outage start/restoration time columns to make them viable to store the necessary date information. 


| U.S._STATE   | POSTAL.CODE   | NERC.REGION   | CLIMATE.REGION     |   ANOMALY.LEVEL | OUTAGE.START.TIME   | OUTAGE.RESTORATION.TIME   | CAUSE.CATEGORY     |   OUTAGE.DURATION |   DEMAND.LOSS.MW |   CUSTOMERS.AFFECTED |   RES.CUSTOMERS |   COM.CUSTOMERS |   IND.CUSTOMERS |   TOTAL.CUSTOMERS |
|:-------------|:--------------|:--------------|:-------------------|----------------:|:--------------------|:--------------------------|:-------------------|------------------:|-----------------:|---------------------:|----------------:|----------------:|----------------:|------------------:|
| Minnesota    | MN            | MRO           | East North Central |            -0.3 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00       | severe weather     |        51         |              nan |                70000 |     2.30874e+06 |          276286 |           10673 |       2.5957e+06  |
| Minnesota    | MN            | MRO           | East North Central |            -0.1 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00       | intentional attack |         0.0166667 |              nan |                  nan |     2.34586e+06 |          284978 |            9898 |       2.64074e+06 |
| Minnesota    | MN            | MRO           | East North Central |            -1.5 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00       | severe weather     |        50         |              nan |                70000 |     2.30029e+06 |          276463 |           10150 |       2.58690e+06 |
| Minnesota    | MN            | MRO           | East North Central |            -0.1 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00       | severe weather     |        42.5       |              nan |                68200 |     2.31734e+06 |          278466 |           11010 |       2.60681e+06 |
| Minnesota    | MN            | MRO           | East North Central |             1.2 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00       | severe weather     |        29         |              250 |               250000 |     2.37467e+06 |          289044 |            9812 |       2.67353e+06 |


The plot below is a graph of the observed distribution of outage times. This gave insight into how I might define a major power outage as, which I chose as the 75th percentile of this distribution. 
<iframe src="assets/outage_duration.html" width=800 height=600 frameBorder=0></iframe>

The map below is a choropleth representing a graph of average outage times across every state. This allows us to get a feel for the general duration of outages in certain areas and allows us to understand regional outages better.
<iframe src="assets/usa_map.html" width=800 height=600 frameBorder=0></iframe>


The aggregate table below shows us the reasons for outages in climate regions and how long on average those outages took. This is a table that could be used to better understand what kind of problems each climate region of the country is experiencing and hindering them. 

| CLIMATE.REGION     |   equipment failure |   fuel supply emergency |   intentional attack |   islanding |   public appeal |   severe weather |   system operability disruption |
|:-------------------|--------------------:|------------------------:|---------------------:|------------:|----------------:|-----------------:|--------------------------------:|
| Central            |             5.36667 |             167.254     |             5.76765  |   2.08889   |         23.5    |          54.1668 |                        44.92    |
| East North Central |           440.589   |             566.188     |            39.6008   |   0.0166667 |         12.2167 |          73.9136 |                        43.5     |
| Northeast          |             3.59667 |             243.826     |             3.26641  |  14.6833    |         44.25   |          73.8317 |                        12.8917  |
| Northwest          |            11.7     |               0.0166667 |             6.2302   |   1.22222   |         14.9667 |          80.6333 |                         2.35    |
| South              |             4.92963 |             291.375     |             5.42679  |   8.225     |         19.3996 |          73.1892 |                        14.4346  |
| Southeast          |             9.24167 |             nan         |             8.41111  | nan         |         47.7567 |          44.376  |                         2.82187 |
| Southwest          |             1.89667 |               1.26667   |             4.42787  |   0.0333333 |         37.9167 |         192.882  |                         5.48704 |
| West               |             8.74683 |             102.577     |            14.2946   |   3.58095   |         33.8019 |          48.8062 |                         6.06111 |
| West North Central |             1.01667 |             nan         |             0.391667 |   1.13667   |          7.325  |          40.7083 |                       nan       |

# Assessment of Missingness
A potential column that could be classified as 'NMAR' in the dataset could be the 'OUTAGE.START.TIME' column. The missing data could be based on certain parts of an energy grid that could have failed, leading to this data becoming uncaptureable. If the reliance of capturing the time was based on certain electrical standards that even data recording mechanisms have dependency on, it could be said that outside factors led to the missing data, meaning it classifies as 'NMAR'.   

This plot is cause of outage against the missingness of demand loss. This gives us a general feel of the data distribution. 
<iframe src="assets/cause_demand.html" width=800 height=600 frameBorder=0></iframe>

This graph is the empirical distribution of the cause of outage against demand loss missingness. This resulted in a p-value of 0.246.
<iframe src="assets/cause_demand_emp.html" width=800 height=600 frameBorder=0></iframe>

With this result, we support that the missingness is completely random, or unrelated to both the observed and unobserved data. This shows us that there may not be a trend of missing demand loss data with cause of outage. We can see that the response of this estimation is not based on this column. It could lead us to exploring other factors that could have led to this column having significant missing data that would have allowed us to determine another way to determining a definition of a major power outage. 

# Hypothesis Testing
H~o -> There is no difference in the proportions of major power outages in each NERC region.
H~a -> There is a difference in the proportions of major power outages in each NERC region.
Significance Level = 0.05 -> this is used as the baseline for many hypothesis tests
Test Statistic: TVD -> we are working with categorical data, making this the optimal statistic
Observed Statistic: 0.32979664014146765
p-value = 0.0
Conclusion: We reject H~o and believe there could be a difference in proportions of major power outages in each NERC region.

The plot below is the result of the hypothesis test.
<iframe src="assets/hyp.html" width=800 height=600 frameBorder=0></iframe>