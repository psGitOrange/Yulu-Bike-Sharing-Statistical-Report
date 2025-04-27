# Hypothesis Testing on Yulu - Bike Sharing Service

### **About Yulu**

Yulu is India’s leading micro-mobility service provider, which offers unique vehicles for the daily commute. Starting off as a mission to eliminate traffic congestion in India, Yulu provides the safest commute solution through a user-friendly mobile app to enable shared, solo and sustainable commuting.

Yulu zones are located at all the appropriate locations (including metro stations, bus stands, office spaces, residential areas, corporate offices, etc) to make those first and last miles smooth, affordable, and convenient!

Yulu has recently suffered considerable dips in its revenues. They have contracted a consulting company to understand the factors on which the demand for these shared electric cycles depends. Specifically, they want to understand the factors affecting the demand for these shared electric cycles in the Indian market.

<img src="https://yulu-blogs-cdn.yulu.bike/large_Whats_App_Image_2023_10_30_at_16_14_34_03ea1f9a_3640299873.jpg" alt="walmart_img" width="720"/>

## Business Problem Statement:
The company wants to know:

- Which variables are significant in predicting the demand for shared electric cycles in the Indian market?
- How well those variables describe the electric cycle demands

**Concepts Used:**

- Bi-Variate Analysis
- 2-sample t-test: testing for difference across populations
- ANNOVA
- Chi-square

### Dataset:</br>
Dataset Link: [yulu_data.csv](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/001/428/original/bike_sharing.csv?1642089089)

**Column Profiling:**

- datetime: datetime </br>
- season: season (1: spring, 2: summer, 3: fall, 4: winter) </br>
- holiday: whether day is a holiday or not (extracted from http://dchr.dc.gov/page/holiday-schedule) </br>
- workingday: if day is neither weekend nor holiday is 1, otherwise is 0.</br>
- weather: </br>
  1. Clear, Few clouds, partly cloudy, partly cloudy
  2. Mist + Cloudy, Mist + Broken clouds, Mist + Few clouds, Mist
  3. Light Snow, Light Rain + Thunderstorm + Scattered clouds, Light Rain + Scattered clouds
  4. Heavy Rain + Ice Pallets + Thunderstorm + Mist, Snow + Fog</br>
- temp: temperature in Celsius</br>
- atemp: feeling temperature in Celsius</br>
- humidity: humidity</br>
- windspeed: wind speed</br>
- casual: count of casual users</br>
- registered: count of registered users</br>
- count: count of total rental bikes including both casual and registered

### Univariate analysis
**Histograms**

- **Temperature**: Values range from 0 to 41°C, with a normal distribution and a mean of 20°C.
- **atemp** (feeling temperature): Similar distribution to temperature, with a mean of 24°C.
- **Humidity**: Ranges from 0 to 100, with a high standard deviation of 19.25 and slightly left-skewed data, with a mean of 61.89.
- **Wind speed**: Ranges from 0 to 57, and is left-skewed.

**Casual users**, **registered users**, and the **count of bikes shared** are all highly left-skewed.

- **Casual users**: Ranges from 0 to 367, with a median of 17 and a standard deviation of 49.
- **Registered users**: Ranges from 0 to 886, with a median of 118 and a standard deviation of 151.
- **Count of bikes shared**: Ranges from 1 to 977, with a median of 145 and a standard deviation of 181.

**Line plots**

- **Temperature (`temp` and `atemp`)**: Both variables follow a cyclic variation based on **seasonal** changes.
- **Humidity**: Shows some faint peaks and crests, with **high variability**, jumping between 20 and 100.
- **Wind speed**: **Fluctuates** between 0 and 40.
- **Casual user count**: Highly volatile, with many users frequently using and stopping the bike-sharing service. Shows **periodic variation**, often syncing with dropping temperatures—suggesting that users may avoid riding in cold weather.
- **Registered user count**: Historically increases, but follows a similar trend to casual users. The number of users **decreases** during **cold** weather.
- **Count variable**: **Correlated** with both casual and registered user counts. As shown in the line plot, it follows the same trend as registered user counts.

**Correlation** matrix shows:
- **Count**:It is highly correlated to `registered`, and `casual`. With correlation value between 0.75 and 1.
- **temp & atemp**: Both are postively correlated to `count`
- **Holiday**: Not correlated with the `count` variable.
- **Humidity**: Inversely proportional to the count of bikes shared, meaning that as humidity increases, the count of bikes shared decreases.

## Q) Is Working day correlated to Count of Yulu Bike shared?
### 2 Sample t-test
**Null Hypothesis (H0):** Bike share mean of Weekdays is equal to Weekends
$$ H_0: \mu_{weekdays} = \mu_{weekends} $$
The null hypothesis states that there is no significant difference between the mean number of bike shares per hour on weekdays and weekends

**Alternative Hypothesis (H1):** Bike share mean of Weekdays is not equal to Weekends
$$
H_1: \mu_{weekdays} \neq \mu_{weekends} $$
The alternative hypothesis states that there is a significant difference between the mean number of bike shares per hour on weekdays and weekends.

Consider the significance level $\alpha = 0.05$.

### Conclusion of T-test on Working day

Only 2 out of 10 test has p value less than 0.05. But this can be a random chance.

**Failing to Reject the Null Hypothesis:** In most of the iterations (8 out of 10) the p-value is above the significance level of 0.05, so we fail to reject the null hypothesis in these cases.

The Weekdays and Weekend variables don't have significant difference in their mean of count of bike shared per day per hour

This suggests that the **mean** number of Yulu bike shares per hour is **statistically similar** on both **weekdays** and **weekends**. This implies that the demand for Yulu bikes remains consistent regardless of whether it's a working day or a weekend.

Yulu should maintain a consistent level of bike availability, maintenance, and staffing across the entire week.

## Q) Does number of Bikes rented change depending on different Weather condition?
### Annova Test

**Null Hypothesis (H0):** The mean bike shares per hour are equal across all weather conditions.
$$
H_0: \mu_{weather\_1} = \mu_{weather\_2} = \mu_{weather\_3}
$$
The null hypothesis states that there is no significant difference in the mean number of bike shares per hour among the three weather conditions.

**Alternative Hypothesis (H1):** At least one of the mean bike shares per hour is different across the weather conditions.
$$
H_1: \text{At least one } \mu_{weather\_i} \text{ is different}
$$
The alternative hypothesis states that there is a significant difference in the mean number of bike shares per hour between at least two of the weather conditions.

Consider the significance level $\alpha = 0.05$.

### Conclusion of annova test on weather conditions

**ANOVA Test Results Between Weather Conditions**

1. **Weather_1 vs. Weather_2:**
   - **F-statistic**: 53.73
   - **p-value**: 2.55e-13

2. **Weather_1 vs. Weather_3:**
   - **F-statistic**: 248.13
   - **p-value**: 9.15e-55

3. **Weather_2 vs. Weather_3:**
   - **F-statistic**: 131.57
   - **p-value**: 9.44e-30

**Conclusion:**

For all pairs of weather conditions, the p-values are much smaller than the significance level of 0.05. This allows us to reject the null hypothesis in each case. Therefore, there is a significant difference in the mean number of bike shares per hour between each pair of weather conditions. Specifically:

- The mean bike shares per hour differ significantly between Weather_1 and Weather_2, between Weather_1 and Weather_3, and between Weather_2 and Weather_3.

We can't say much about weather condition 4 but, these results suggest that weather conditions have a significant impact on the mean number of bike shares per hour.

This suggest that one weather condition results in significantly higher or lower bike shares compared to others, this weather condition could be a key factor influencing bike usage patterns.

From the histogram above we can say that light weather condition are more favourable to customer to share a Yulu bike ride. The mean count of bike share increases as weather conditions are become less extreme. i.e. $$ \mu_{weather\_1} > \mu_{weather\_2} > \mu_{weather\_3} > \mu_{weather\_4} $$

## Q) Does number of Bikes rented change depending on seasons
### Annova Test
**Null Hypothesis (H0):** The mean bike shares per hour are equal across all seasons.
$$
H_0: \mu_{season\_1} = \mu_{season\_2} = \mu_{season\_3} = \mu_{season\_4}
$$
The null hypothesis states that there is no significant difference in the mean number of bike shares per hour among the four seasons.

**Alternative Hypothesis (H1):** At least one of the mean bike shares per hour is different across the seasons.
$$
H_1: \text{At least one } \mu_{season\_i} \text{ is different}
$$
The alternative hypothesis states that there is a significant difference in the mean number of bike shares per hour between at least two of the seasons.

Consider the significance level $\alpha = 0.05$.

### Conclusion of annova test on Seasons variable

We conducted paired one-way ANOVA tests as well for all possible combinations of seasons, and saw that p-values are less than the significance level of 0.05 in every case.

**Conclusion:**

Since all p-values are smaller than the significance level, we reject the null hypothesis for each paired comparison. This indicates that there is a **significant difference** in the **mean** number of bike shares per hour **between** each pair of seasons. In other words, the mean bike share rate for every season differs significantly from that of the others.

This suggest that one Season condition results in significantly higher or lower bike shares compared to others, this Season condition could be a key factor influencing bike usage patterns.

From the histogram above we can say that the mean count of bike share increases in the order below: $$ \mu_{season\_3} > \mu_{season\_2} > \mu_{season\_4} > \mu_{season\_1} $$

The mean bike share count is highest in the fall (Season 3) and Summer (Season 2). This could be due to favorable weather conditions, longer daylight hours, and possibly more outdoor activities that encourage biking.

The mean bike share count is lowest in winter (Season 4) and Spring (Season 1). This could be due to colder temperatures, shorter days, and potentially less favorable weather conditions in these seasons might discourage biking.

## Q) Are Weather and Season dependent on each other
### Chi-Square Test

**Null Hypothesis (H0):** There is no association between weather and season; the distribution of weather types is independent of the season.
$$
H_0: \text{Weather and season are independent.}
$$
The null hypothesis states that the distribution of weather types does not vary across different seasons, implying no significant association between weather and season.

**Alternative Hypothesis (H1):** There is an association between weather and season; the distribution of weather types depends on the season.
$$
H_1: \text{Weather and season are not independent.}
$$
The alternative hypothesis states that the distribution of weather types varies across different seasons, implying a significant association between weather and season.

Consider the significance level $\alpha = 0.05$.

### Conclusion of Chi Square test on dependability of Season and Weather

- **Chi-Square Statistic**: 28.83
- **p-value**: 6.56e-05
- **Degrees of Freedom (df)**: 6

**Reject the Null Hypothesis:** Since the p-value (6.56e-05) is much smaller than the significance level of 0.05, we reject the null hypothesis.

This indicates that there is a **significant association** between **weather** and **season**. In other words, the distribution of weather types depends on the season.

**Naturally** the distribution of weather types varies with the season.

This implies that **different** types of **weather** are more likely to occur in **specific seasons**, which in turn **affects** the number of Yulu bike shares per hour.

## Future Work:
1) Time series analysis on the `count` variable with respect to different datetime attributes, e.g., daily bike share trends for a week.

2) Predicting bike share count on specific **date, time, week, or month**.
