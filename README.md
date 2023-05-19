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


<iframe src="assets/outage_duration.html" width=800 height=600 frameBorder=0></iframe>