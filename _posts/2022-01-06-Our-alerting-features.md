---
layout: post
title:  "Alerting options"
categories: [ Overview ]
image: assets/images/Boostkpi-overview.png
beforetoc: ""
toc: false
---

This post will help introduce you to several of the anomaly alerting features BoostKPI offers.

BoostKPI is designed to dive-in and analyze your high dimensional data. A dimension is a collection of dimension values a data point can have, often a dimension is the name of a text database column and the dimension values are the values in that column. A KPI, or key performance indicator, is a numeric value of interest for your timepoints. Often a KPI corresponds to the name of a numeric database column, but sometimes KPIs can be derived from other KPIs.

An anomaly is a time period where a KPI value has an unusual deviation. BoostKPI has several ways of configuring anomaly detections to help you identify the most meaningful and actionable anomalies.

## An example dataset

We will use the following example dataset to help demonstrate example uses of BoostKPI's alerting features.

| Time               | Country | Operating system | Spend  | Clicks |
| :----------------- | :------ | :--------------- | :----- | :----- |
| 2022-01-03 5:00:00 | US      | Android          | 138.05 | 644    |
| 2022-01-03 5:00:00 | IN      | Android          | 116.22 | 378    |
| 2022-01-03 6:00:00 | US      | iOS              | 122.14 | 241    |

This dataset represents hourly aggregated advertising data. In this example, the "Country" and "Operating system" columns are dimensions while the "Spend" and "Clicks" columns are KPIs. We can also define the derived KPI "Cost per click" as "Clicks" divided by "Spend".

# Detecting anomalies

We support a variety of rules and methods for detecting anomalies. Many of the following rules are configurable to meet specific business requirements.

## Absolute change

An absolute change anomaly is when a KPI value deviates by a fixed, absolute amount from a specified offset.

### Example use

- If we are interested in monitoring our spending, we could detect if the total "Spend" for the day rose by more than 100 compared to the total "Spend" for the same day last week.
- For each "Country", we can detect if "Clicks" drops by more than 100 from the same hour the previous day to make sure we are reaching users everywhere.

## Percentage change

A percentage change anomaly occurs when a KPI value deviates by a configurable percentage from a specified offset.

### Example use

- As our example company grows, a fixed rise of 100 in "Spend" may not be as notable. Switching to detect percentage change anomalies in "Spend" could be more interesting. 

## Threshold

A threshold anomaly is when a KPI value exceeds a defined absolute threshold. The detection can be configured to use a maximum, minimum, or both.

### Example use

- We can detect if "Clicks" drops below 50 for any "Country" value to make sure our ad campaigns are reaching real users.
- To make sure we are being cost effective, we can detect if "Cost per click" ever exceeds 1. 

## Mean-variance

To detect a mean-variance anomaly, we examine historical changes in your data and label time points as anomalous when they are unexpectedly high or low based on the mean and standard deviation of the historical changes. 

Mean-variance anomalies are useful when trying to find anomalies in noisy data. Our previous anomaly types are not sensitive to past data outside of the comparison offset. Mean-variance anomalies take into account how your data has been changing and will be more robust to seasonalities.

### Example use

- If "Spend" has a large amount of day-to-day noise, we can detect mean-variance anomalies to notice when "Spend" changes by a historically unusual amount.

## Holt-Winters

Holt-Winters seasonal method is a forecasting method that describes trends in data using three components: the baseline "level" of the data, the current "trend" of linear increase or decrease of the data, and a "seasonal" component that captures a repeating cyclic change in the data. We use this seasonal method to predict individual time points and compare them to your actual data. We detect anomalies when your actual time point deviates too much from the forecasted time point.

### Example use

- "Clicks" has a high 24-hour seasonality; when people are asleep, they do not click on ads. So, our data can move from a high number in the middle of the day to near zero in the middle of the night. Using an absolute or percentage change filter to notice unusual KPI movement would not work well. Instead, we can use Holt-Winters to more accurately identify anomalies in this type of data.

# Anomaly filters

BoostKPI supports filtering out detected anomalies to help get you the most meaningful anomalies.

## Absolute change

Anomalies can be filtered out if they do not correspond to a change of at least a certain absolute amount.

### Example use

- When detecting percentage change anomalies, if a KPI value for a particular dimension value changes from 2 to 3, that corresponds to a large 50% change, but a very small absolute change. When other dimension values have KPI values in the hundreds or thousands, filtering out this relatively small dimension value can be done with an absolute change filter.

## Percentage change

Anomalies can be filtered out when they do not cover times when KPIs increased or decreased by at least a certain percentage.

### Example use

-  This percentage change filter can help filter out threshold anomalies that are the result of slow, steady increases as opposed to unusual jumps.

## Threshold

Anomalies can be filtered out when their KPI value falls outside an absolute threshold. This can be done based on either the total KPI value for the anomaly or based on the hourly or daily average KPI value for the anomaly.

### Example use

- If "Clicks" had an anomalous drop, but is still at an acceptable level we can filter out the anomaly. This might happen if "Clicks" goes up an unsually high amount. The drop back to normal might not be as notable.

## Duration

To prevent alerting on transient anomalies, BoostKPI can filter out anomalies that do not last for a minimum duration.

### Example use

- If a mean-variance anomaly lasts only for a single time point, it may not be meaningful. We can filter out these anomalies based on how long they last.

## Sitewide impact

Occasionally, anomalies for one KPI may not be meaningful if they do not cause changes in another KPI. Our sitewide impact filter enables you to filter out anomalies in one KPI based on changes in another KPI.

### Example use

- If "Spend" drops significantly and is detected as an anomaly, but "Clicks" remains unchanged, a sitewide impact filter can remove this anomaly.

# Additional features

## Dimension exploration and filtering

BoostKPI supports detecting anomalies in your data broken down according to a set of dimensions which can either be manually specified or automatically generated. Using this feature will enable you to detect anomalies in important categories that might not be visible in fully aggregated data.

For example, in our advertising dataset, we could detect anomalies in "Spend" broken down by "Country" on the Android operating systems.

When a dimension has a very high number of dimension values, the signal-to-noise ratio of anomalies can drop. To handle, this BoostKPI provides several ways to limit which dimension values are examined for anomalies. We support filtering to dimension values that are in the top 50 in contributing to a KPI's total, dimension values that contribute a certain percentage to a KPI's total, or dimension values whose KPI value is above or below configurable thresholds.

## Time

While your dataset may have a particular time granularity, the advertising example has an hourly granularity, you may wish to detect anomalies at a different granularity. BoostKPI supports this use case and allows you to detect analyze your data at coarser granularity. Currently we support aggregating up to weekly granularities for all detection types and up to monthly granularities for some detection types. Because month durations vary, some detection types can not reasonably handle aggregating to monthly granularity.

## Severity labels

To better aid you in identifying important anomalies, BoostKPI supports labelling anomalies with severties. These severity levels are configurable based on duration and change in KPI value.

## Automatic merging

Occasionally, unusual data or complicated detection rules can result in BoostKPI detecting anomalies that either overlap or are very close to each other. BoostKPI can optionally merge anomalies based on their duration and separation to ensure you receive a concise picture of anomalies in your data.

## Composite alert grouping

When business goals are complicated, more sophisticated detection rules are needed. BoostKPI's alerting system supports combining multiple detection rules to meet your needs. In our advertising example, you could detect anomalies when "Spend" has risen by a percentage, but "Clicks" has remain unchanged combining a percentage change and threshold anomaly detection.

## Offset options

When comparing to past data such as when detecting percentage change anomalies, BoostKPI supports a variety of offsets. The simplest options are basic time based offsets such as comparing the current time point to the time point from one hour, day, or week ago, but we also support more complex offsets like comparing to the mean of the last seven days, median of the last four weeks, or maximum of the last month.
