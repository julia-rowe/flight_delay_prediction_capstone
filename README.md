# Flight Delay Prediction
***

### Background
---
Flight delays are very costly for airline companies and are often due to reasons outside of the air carrier's control. The US Bureau of Transportation Statistics categorizes flight delays into five categories: late-arriving aircraft, air carrier, National Aviation System (NAS), extreme weather, and security. Late-arriving aircrafts are the most common reason for flight delays, which consisted of 39.8% of flight delays in 2015. While extreme weather typically consists of only 5% of flight delays, weather contributed to 32.8% of total delay minutes in 2015. It should be noted that Airlines do not report the causes of the late-arriving aircraft. Based on this, we can speculate that a similar portion of these late-arriving aircraft is caused by weather conditions. [(“Understanding the Reporting of Causes of Flight Delays and Cancellations”, 2021)](https://www.bts.gov/topics/airlines-and-airports/understanding-reporting-causes-flight-delays-and-cancellations)

When a flight delay occurs, airlines will incur various direct and indirect costs. It has been calculated that, in 2015, each minute of aircraft block time (includes taxi and airborne time) costs \$65.43 for U.S. passenger airlines [(“Cost of Aircraft Delay to U.S. Passenger Carriers”)](http://tempstaging.a4a.teamsubjectmatter.com/data/cost-of-aircraft-delay-to-u-s-passenger-carriers/). And annually, FAA/Nextor estimated the cost of flight delays to be \$28 billion [(“U.S. Passenger Carrier Delay Costs”, 2020)](https://www.airlines.org/dataset/u-s-passenger-carrier-delay-costs/).


### Problem Statement
---
Can we predict severity of flight delays? Some flight delays can be prevented, while other times, it is inevitable due to weather conditions. Despite flight delays being caused by factors outside of their control, many large airlines will provide some type of compensation to their customers based on the length of the delay. Flight delays will also cause scheduling issues for future flights since many domestic flights are used to transport crew members to their next flight. This will in turn cause a domino effect that will delay future flights unless the airlines are able to find replacement crew to staff their future flights. If there is an algorithm that can predict the severity of flight delay, this could potentially reduce the loss in direct and indirect costs for airline companies. And depending on the severity of the delay, different actions can be taken to reduce customer refund requests or ensure crew members arrive on time to their working flights.

### Data
---
* ‘2015 Flight Delays and Cancellations’ flights data from [Kaggle](https://www.kaggle.com/usdot/flight-delays)
* Daily climate data from the National Oceanic and Atmospheric Administration (NOAA) in 2015

Due to the size of the flights data and limitations of requesting data files from NOAA, the data has been scoped down to a singular flight plan with the departure airport from LGA to the arrival airport ORD. Climate data has been retrieved from NOAA for the two cities, Queens, NY and Chicago, IL. 

### Methodology
---
For our predictions I have created three classes for the outcome based on the arrival delay values: on-time, minor, and major. Any flights that arrived early or has no delay will be labeled as on-time. The threshold for the minor and major delay were based on the U.S. minimum connection times, which range from 30 minutes to 2 hours. This is a program implemented for many domestic flights to require passengers to allow a selected minimum time before the departure of their transfer flights [(“What is Minimum Connection Time?”, 2021)](https://www.airtreks.com/go/minimum-connection-time/). Minor delays are categorized as flights with arrival delays up to 30 minutes, which will still allow passengers and crew members to board their next flight on-time. Major delays were labeled for flight with arrival delay more than 30 minutes.

Since the objective is to predict flight delays prior to the flight, we will not include features into our models which are related to data that is recorded after the flight has departed to maintain true predictive power.

During the preliminary modeling phase, ten classification models with default parameters have been fitted to the data to identify the best performing models. The performance of the models were evaluated based on the bias and variance level of the train and test data accuracy scores and the recall scores. From there, Randomized Search objects were used to further fine tune the best models with various hyperparamters.

For the purpose of predicting flight delay severity, false negatives are worse than false positives. It is more important for the model to correctly flag more incidents of flight delays than it is to accurately identify flights that will not result in delays. Therefore, the metric that was optimized for these models were recall scores. Additionally, the recall scores were computed with a macro-average weight that would apply even weight of scores to inflate the results of the minority classes.

### Conclusion
---
The baseline score of this data was 65.6% and our final models did perform better than the baseline score. From the preliminary modeling, the best models were identified to be Gradient Boosting Classifier and XGBoost Classifier, and Randomized Search helped to increase the recall score for the Gradient Boosting Classifier, and decrease overfitting for the XGBoost Classifier. Of these two, the XGBoost had the highest accuracy and recall score. The XGBoost also correctly identified the minor and major delay outcomes better than all the models.

Although the model did perform better than the baseline, the score can still be improved. Exploring other avenues to deal with imbalanced data, such as oversampling techniques and adjusting the threshold for the predictions may improve the recall scores. Additionally collecting more data specifically on weather types and flight data with complete delay reason labels would most likely succeed in improving the models. A majority of weather types and flight delay reasons were found to be null in the datasets and utlimately had to be dropped during the data cleaning process. Collecting the data on hourly climate may be provide more relevant climate data to each flights' scheduled departure and arrival times, but this may introduce additional challenges considering the volatile nature of weather. If the scores of the model could successfully improve with these methods, it would also be interesting integrate a time series model to inspect how well flight delays can be forecasted based on climate data from hours to days before the scheduled flights.





