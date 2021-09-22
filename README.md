# Flight Delay Prediction



**Objective:** To create a multi-class classification model to predict the severity of flight delays in the U.S. categorized to three outcomes: on-time, minor delay, and major delay.






### Background
---
Flight delays are very costly for airline companies and are often due to reasons outside of the air carrier's control. The US Bureau of Transportation Statistics categorizes flight delays into five categories: late-arriving aircraft, air carrier, National Aviation System (NAS), extreme weather, and security. Late-arriving aircrafts are the most common reason for flight delays, which consisted of 39.8% of flight delays in 2015. While extreme weather typically consists of only 5% of flight delays, weather contributed to 32.8% of total delay minutes in 2015. It should be noted that Airlines do not report the causes of the late-arriving aircraft. Based on this, we can speculate that a similar portion of the flights in the late-arriving aircraft category is caused by weather conditions. [(“Understanding the Reporting of Causes of Flight Delays and Cancellations”, 2021)](https://www.bts.gov/topics/airlines-and-airports/understanding-reporting-causes-flight-delays-and-cancellations)

When a flight delay occurs, airlines will incur various direct and indirect costs. It has been calculated that, in 2015, each minute of aircraft block time (includes taxi and airborne time) costs \$65.43 for U.S. passenger airlines [(“Cost of Aircraft Delay to U.S. Passenger Carriers”)](http://tempstaging.a4a.teamsubjectmatter.com/data/cost-of-aircraft-delay-to-u-s-passenger-carriers/). And annually, FAA/Nextor estimated the cost of flight delays to be \$28 billion [(“U.S. Passenger Carrier Delay Costs”, 2020)](https://www.airlines.org/dataset/u-s-passenger-carrier-delay-costs/).


### Problem Statement
---
Can we predict severity of flight delays? Some flight delays can be prevented, while other times, it is inevitable due to weather conditions. Despite flight delays being caused by factors outside of their control, many large airlines will provide some type of compensation to their customers based on the length of the delay. Flight delays will also cause scheduling issues for future flights since many domestic flights are used to transport crew members to their next flight. This will in turn cause a domino effect that will delay future flights unless the airlines are able to find replacement crew to staff their future flights. If there is an algorithm that can predict the severity of flight delay, this could potentially reduce the loss in direct and indirect costs for airline companies. And depending on the severity of the delay, different actions can be taken to reduce customer refund requests or ensure crew members arrive on time to their working flights.

### Data
---
* Flight data from all major airlines in 2015
    * flights.csv (file exceeds GitHub maximum file size limit. Data can be downloaded [here](https://www.kaggle.com/usdot/flight-delays) from Kaggle)
* Daily climate data from the National Oceanic and Atmospheric Administration (NOAA) in 2015
    * [chi_ny.csv](./data/flight_delay_cleaned.csv)

Due to the size of the flight data and limitations of requesting files from NOAA, the data has been scoped down to a singular flight plath with flights departing from the LGA and arriving at ORD. Climate data has been retrieved from NOAA for two cities, Queens, NY and Chicago, IL.

#### Data Dictionary:

Data dictionary of the cleaned dataset ([flight_delay_cleaned.csv](./data/flight_delay_cleaned.csv)) used for modeling.

|Feature|Type|Description|
|---|---|---|
|date|object|date of the flight (YYYY-MM-DD format)|
|delay_severity|object|target variable indicating the level of delay of the flight arrival (0 = on-time, 1 = minor-delay, 2 = major delay)|
|avg_wind_speed_lga|float|average daily wind speed (miles per hour) in Queens, NY|
|precipitation_lga|float|total daily precipitation (in inches) in Queens, NY|
|snowfall_lga|float|total daily snowfall (in inches) in Queens, NY|
|snow_depth_lga|float|total daily snow depth (in inches) in Queens, NY|
|average_temp_lga|float|average daily temperature (Fahrenheit) in Queens, NY|
|max_temp_lga|float|maximum daily temperature (Fahrenheit) in Queens, NY|
|min_temp_lga|float|minimum daily temperature (Fahrenheit) in Queens, NY|
|wind_direction_fastest_2min_lga|float|direction of daily fastest 2-minute wind (degrees) in Queens, NY|
|wind_direction_fastest_5sec_lga|float|direction of daily fastest 5-second wind (degrees) in Queens, NY|
|wind_speed_fastest_2min_lga|float|daily fastest 2-minute wind speed (miles per hour) in Queens, NY|
|wind_speed_fastest_5sec_lga|float|daily fastest 5-second wind speed (miles per hour) in Queens, NY|
|avg_wind_speed_ord|float|average daily wind speed (miles per hour) in Chicago, IL|
|precipitation_ord|float||total daily precipitation (in inches) in Chicago, IL|
|snowfall_ord|float|total daily snowfall (in inches) in Chicago, IL|
|snow_depth_ord|float|total daily snow depth (in inches) in Chicago, IL|
|average_temp_ord|float|average daily temperature (Fahrenheit) in Chicago, IL|
|max_temp_ord|float|maximum daily temperature (Fahrenheit) in Chicago, IL|
|min_temp_ord|float|minimum daily temperature (Fahrenheit) in Chicago, IL|
|wind_direction_fastest_2min_ord|float|direction of daily fastest 2-minute wind (degrees) in Chicago, IL|
|wind_direction_fastest_5sec_ord|float|direction of daily fastest 5-second wind (degrees) in Chicago, IL|
|wind_speed_fastest_2min_ord|float|daily fastest 2-minute wind speed (miles per hour) in Chicago, IL|
|wind_speed_fastest_5sec_ord|float|daily fastest 5-second wind speed (miles per hour) in Chicago, IL|
|month|int|month of the year|
|day|int|day of the month|
|airline_AA|int|binary value to indicate whether the airline was American Airlines|
|airline_NK|int|binary value to indicate whether the airline was Spirit Airlines|
|airline_OO|int|binary value to indicate whether the airline was Skywest Airlines|
|airline_UA|int|binary value to indicate whether the airline was United Airlines|
|Monday|int|binary value to indicate whether the flight departed on a Monday|
|Tuesday|int|binary value to indicate whether the flight departed on a Tuesday|
|Wednesday|int|binary value to indicate whether the flight departed on a Wednesday|
|Thursday|int|binary value to indicate whether the flight departed on a Thursday|
|Friday|int|binary value to indicate whether the flight departed on a Friday|
|Saturday|int|binary value to indicate whether the flight departed on a Saturday|
|Sunday|int|binary value to indicate whether the flight departed on a Sunday|
|6am-9am_dep|int|binary value to indicate whether the flight departed between 6am-9am|
|9am-12pm_dep|int|binary value to indicate whether the flight departed between 9am-12pm|
|12pm-3pm_dep|int|binary value to indicate whether the flight departed between 12pm-3pm|
|3pm-6pm_dep|int|binary value to indicate whether the flight departed between 3pm-6pm|
|6pm-9pm_dep|int|binary value to indicate whether the flight departed between 6pm-9pm|
|9pm-12am_dep|int|binary value to indicate whether the flight departed between 9pm-12am|
|6am-9am_arr|int|binary value to indicate whether the flight arrived between 6am-9am|
|9am-12pm_arr|int|binary value to indicate whether the flight arrived between 9am-12pm|
|12pm-3pm_arr|int|binary value to indicate whether the flight arrived between 12pm-3pm|
|3pm-6pm_arr|int|binary value to indicate whether the flight arrived between 3pm-6pm|
|6pm-9pm_arr|int|binary value to indicate whether the flight arrived between 6pm-9pm|
|9pm-12am_arr|int|binary value to indicate whether the flight arrived between 9pm-12am|

### Methodology
---
For our predictions I have created three classes for the outcome based on the arrival delay values: on-time, minor, and major. Any flights that arrived early or has no delay have been labeled as on-time. The threshold for the minor and major delay classes were based on the U.S. minimum connection times, which can range from 30 minutes to 2 hours. This is a program implemented for many domestic flights to require passengers to allow a selected minimum time between the scheduled flight arrival time and the departure time of their transfer flights [(“What is Minimum Connection Time?”, 2021)](https://www.airtreks.com/go/minimum-connection-time/). Minor delays have been categorized as flights with arrival delays up to 30 minutes, which will still allow passengers and crew members to board their next flights on-time. Major delays were labeled for flights with arrival delays of more than 30 minutes.

Since the objective is to predict flight delays with information available prior to each flight, we will not include features into our models which are related to data that is recorded after the flight has departed to maintain true predictive power.

During the preliminary modeling phase, ten classification models with their default parameters have been fitted to the data to identify the best performing models for this data. The performance of the models were evaluated based on the bias and variance levels of the train and test data accuracy scores and the recall scores. From there, Randomized Search objects were used to further fine tune the best models with various hyperparamters.

For the purpose of predicting flight delay severity, false negatives are worse than false positives. It is more important for the model to correctly flag more incidents of flight delays than it is to accurately identify flights that will not result in delays. Therefore, the metric that was optimized for these models were recall scores. Additionally, the recall scores were computed with a macro-average weight that would apply an even weight of scores to each class to inflate the results of the minority classes.

### Conclusion
---
The baseline score of the data was 65.6% and our final models did perform better than the baseline score. From the preliminary modeling, the best models were identified to be Gradient Boosting Classifier and XGBoost Classifier, and Randomized Search helped to increase the recall score for the Gradient Boosting Classifier, and decrease overfitting for the XGBoost Classifier. The final Gradient Boosting Classifier model performed at an accuracy of 71.5% with a recall score of 52.6%. The accuracy of the XGBoost Classifier model was 72.1% with a recall score of 55%. Of the final two models, the XGBoost model had the highest accuracy and recall score. And the confusion matrix revealed that the final XGBoost model correctly identified the minor and major delay outcomes better than all the models.

Although the model did perform better than the baseline, the score can still be improved. Exploring other avenues to deal with imbalanced data, such as oversampling techniques and adjusting the threshold for the predictions may improve the recall scores. Additionally collecting more data specifically on weather types and flight data with complete delay reason labels would most likely succeed in improving the models. A majority of weather types and flight delay reasons were found to be null in the datasets and utlimately had to be dropped during the data cleaning process. Collecting the data on hourly climate may be provide more relevant climate data to each flights' scheduled departure and arrival times, but this may introduce additional challenges considering the volatile nature of weather. If the scores of the model could successfully improve with these methods, it would also be interesting integrate a time series model to inspect how well flight delays can be forecasted based on climate data from hours to days before each scheduled flight.





