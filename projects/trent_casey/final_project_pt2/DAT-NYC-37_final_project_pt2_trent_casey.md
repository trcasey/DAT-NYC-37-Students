# Final Project, Part 2: Project Design Writeup

DAT-NYC-37

Trent Casey
___
## 1. Project Problem and Hypothesis

### The Problem:

Parking a car in New York City is a miserable experience that often ends with an orange envelope under your windshield wiper. With alternate-side parking regulations, unclear signage, and driveways that barely look like driveways, many residents get to participate in this thankless exercise. So, is it possible to predict the likelihood of getting a ticket at a daily level to help minimize, or even eliminate, the risk?

In an effort to focus in on an area I know well, the project will be specific to my neighborhood: Astoria, Queens. Since I have years of first-hand experience with the street parking puzzle there and can more easily visit any particularly interesting areas that might arise throughout the exploratory analysis.

### The Hypothesis:

Using knowable variables like date, time of day, location, and details about the vehicle, it will be possible to predict the number of tickets given on a certain day.

As this hypothesis is stated, a linear regression model seems to be the best option. There are a few concerns with the data that will be addressed in section 4.

#### Secondary Hypothesis:

A secondary, classification-based hypothesis is that it is possible to predict the _plate type_ of a vehicle (Passenger, Commercial, etc.) given variables such as date, time of day, location, and type of violation.

Another classification-based hypothesis is that it is possible to predict the type of violation (_Violation Code_) (Failure to Display Muni Rec, No Parking (street clean), No Standing, etc.) given variables such as date, time of day, location, and type of violation.

## 2. Datasets

The data sets for this project come from [New York City's Open Data Portal](https://data.cityofnewyork.us/):

1. [Parking Violations Issued - Fiscal Year 2016](https://data.cityofnewyork.us/City-Government/Parking-Violations-Issued-Fiscal-Year-2016/kiv2-tbus)
2. [DOF Parking Violation Codes](https://data.cityofnewyork.us/Transportation/https://github.com/tswanson/NYCParkingGeocode |DOF-Parking-Violation-Codes/ncbg-6agr)

This dataset has 43 variables, and many of them are missing a considerable amount of data. All variables to be included in the initial analysis will be bolded in the data dicitonary. The officer writing the ticket is required to complete 10 of them, so they will be included in the initial analysis along with several others.

### Data Dictionary
| Variable | Description | Type of Variable |
|---|---|---|
| **Summons Number** | Number of the ticket. Unique number. | numerical index |
| **Plate ID** | License plate of the vehicle. | numerical index |
| **Registration State** | State in which the vehicle is registered | categorical |
| **Plate Type** | Type of vehicle: commercial, passenger, etc. | categorical |
| **Issue Date** | Date the ticket was issued | datetime
| **Violation Code** | Shorthand code for the type of violation issued | categorical |
| **Vehicle Body Type** | Description of the type of vehicle: sedan, suburban, etc. Up to 4 letter code. | categorical |
| **Vehicle Make** | Company that manufactured the vehicle | categorical |
| **Issuing Agency** | 1 letter code for the agency that wrote the ticket | categorical |
| **Street Code1** | The first street code. Still trying to decipher this. numerical | categorical |
| **Street Code2** | The second street code. Still trying to decipher this. numerical | categorical |
| **Street Code3** | the third street code. Still trying to decipher this. numerical | categorical |
| **Vehicle Expiration Date** | The date the vehicle's registration expires (YYYYMMDD) | datetime |
| Violation Location | 3 or 4 digit numerical code of the precinct. | categorical |
| **Violation Precinct** | Precinct number the violation occurred in. | categorical |
| **Issuer Precinct** | Precinct number of the issuing officer. | categorical |
| **Issuer Code** | ID number for the issuing officer | continuous |
| **Issuer Command** | ID number for where the issuing officer is stationed | categorical |
| **Issuer Squad** | Squad number for the issuing officer | categorical |
| **Violation Time** | Time the ticket was issued. Four numbers and either A or P. (1230A would be 12:30 AM) | datetime |
| Time First Observed | Time the violation was first observed by the officer. Four numbers and either A or P. (1230A would be 12:30 AM) | datetime |
| Violation County | County the violation occurred in. | categorical |
| Violation In Front Of Or Opposite | additional information about where the violation was located on the street. | categorical |
| **House Number** | Street number of building the violation occurred in front of. | categorical |
| **Street Name** | Name of the street the violation occurred on. | categorical |
| Intersecting Street | Name of the intersecting street. | categorical |
| Date First Observed | Date the violation was first observed. When used the date is in the following format: YYYYMMDD. Mostly this field is 0. | datetime
| Law Section | Section of the NYC legal code that was violated. | categorical |
| **Sub Division** | Code for the NYC subdivision of the violation. Working on finding a table to decipher this. | categorical |
| Violation Legal Code | Mostly null. The non-null fields are all 78. | categorical |
| **Days Parking In Effect** | Notation of which days the parking laws are in effect. 0 to 7 Ys (YYYYYYY). Working on decoding. | categorical|
| **From Hours In Effect** | Time when parking law becomes enforceable. 4 numbers and A or P (1230P = 1230PM) | datetime |
| **To Hours In Effect** | Time the parking law is no longer in effect. 4 numbers and A or P (1230P = 1230PM) | datetime |
| **Vehicle Color** | Color of the vehicle receiving the ticket | categorical |
| **Unregistered Vehicle?** | Is the vehicle registered | categorical |
| **Vehicle Year** | Year the vehicle was manufactured. | datetime |
| **Meter Number** | Number of the meter where the violation took place. | categorical |
| Feet From Curb | How far the vehicle was from the curb. | continuous |
| **Violation Post Code** | Still looking into this one... | categorical |
| **Violation Description** | Code for and brief description of the violation | categorical |
| **No Standing or Stopping Violation** | Whether this is or isn't a standing or stopping violation. | categorical |
| **Hydrant Violation** | Whether this a hydrant violation. | categorical |
| **Double Parking Violation** | Whether this is a double parking violation | categorical |


## 3. Domain Knowledge

The extent of my domain knowledge in this area is experiential. I've had a car for the majority of the ten years I've been in NYC, and have spent countless hours on finding street parking.

There are several existing projects available online that use the NYC Parking Violations dataset from previous years. The two from I Quant NY might be good examples of exploratory analysis, but don't go into much in terms of hypothesis testing.

1. [I Quant NY Example 1: Using Open Data to Predict When You Might Get Your Next Parking Ticket](http://iquantny.tumblr.com/post/76937212765/using-open-data-to-predict-when-you-might-get-your)
2. [I Quant NY Example 2: Mapping NYC Hydrant Revenue: Upper East’s 19th Precinct is Ridiculously Ticket Happy](http://iquantny.tumblr.com/post/92116352544/mapping-nyc-hydrant-revenue-upper-easts-19th)
3. I also found a more academic paper by Samuel S. Ackerman, a doctoral student in statistics, from 2011 that digs into NYC Parking Violation data. [Download the  PDF](https://drive.google.com/file/d/0BwdmPkna7flbanAtN0JpZlJlU3M/view?usp=sharing)
4. [NYC Data Science Academy Exploratory Analysis](http://blog.nycdatascience.com/student-works/visualizing-new-york-citys-parking-violation-data/)
5. [RentHop - Parking Tickets NYC 2015](https://prezi.com/6iueyy33blfk/analysis-of-new-york-city-parking-violations-under-weather-c/)

## 4. Project Concerns

Part of my reason for focusing on Astoria is to make the data more uniform in certain ways. Each area of the city has its own mix of rules and regulations and Astoria as a neighborhood is mostly residential with concentrations of commercial which should make the analysis more straightforward. I do have some concerns that this could be too focused and not provide enough data to perform a satisfactory analysis, since there are certain variables in the dataset that are missing significant amounts of data, but fortunately there are 10 fields that officers are required to complete on each ticket.

This dataset only represents vehicles that receive tickets and there is no data about vehicles that do not receive tickets--this seems like an obvious point, but it does place limits on the analysis. There are also variables that will require a fair amount of cleaning. For example the _Vehicle Color_ variable has at least five options for black (BK, BLACK, BL (might be blue), BLK, BLCK). I also have to figure out how to handle the location data. The fields _Street Code1, Street Code2,_ and _Street Code3_ are coded in some way I haven't been able to decipher yet, so the street number and name fields may be the best way to go for location information.

Another concern is that in order to properly analyze the data for the given hypothesis, some work is going to need to be done to create a proper target variable. Total tickets per hour or day will be the target variable I plan to construct with the expectation of performing an analysis similar to the one we did in class on the Citi Bike data.

I also have concerns about this being the correct target variable to use for this project and since this data is so geographical in nature, I would like to find a way to present it visually that way.

## 5. Outcomes

I expect the project to find a small number of features with a strong correlation to number of tickets given in a particular time period. The target audience would be people that regularly have to find parking in my neighborhood, but I could see the analysis also appealing to NYC residents at large. For this audience, a more visual presentation of the outcome would be best. The model should be a relatively straight-forward linear regression with no more than 10 predictor variables--ideally closer to 5.

A "successful" project will be able to predict the number of tickets given on a certain day with a reasonable mean squared error. If the project is a bust, there will still be interesting and potentially useful information that will come out of the exploratory analysis process that could potentially be used to make adjustments for a follow-up project.
