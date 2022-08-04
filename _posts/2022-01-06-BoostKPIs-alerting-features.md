---
layout: post
title:  "BoostKPI's alerting features"
categories: [ Overview ]
image: assets/images/Warning-rafiki.svg
beforetoc: ""
toc: false
---

This post will help introduce you to several of the anomaly alerting features BoostKPI offers.

BoostKPI is designed to dive-in and analyze your high dimensional data. A dimension is the name of a collection of values a data point can have, often a dimension is the column name of a text or string field in your database and the dimension values are the values in that column. A KPI, or key performance indicator, is a numeric value of interest for your timepoints. Often a KPI corresponds to the name of a numeric database column, but sometimes KPIs can be derived from other KPIs, e.g, average order value is derived from total revenue and number of orders.

An anomaly is a time period where a KPI value has an unusual deviation. BoostKPI has several ways of configuring anomaly detections to help you identify the most meaningful and actionable anomalies. A detection searches for one or more anomaly type and then optionally applies filters to remove anomalies that are not important.

#### An example dataset

We will use the following example dataset to help demonstrate a few uses of BoostKPI's alerting features.

| Time               | Country | Operating system | Spend  | Clicks |
| :----------------- | :------ | :--------------- | :----- | :----- |
| 2022-01-03 5:00:00 | US      | Android          | 138.05 | 644    |
| 2022-01-03 5:00:00 | IN      | Android          | 116.22 | 378    |
| 2022-01-03 6:00:00 | US      | iOS              | 122.14 | 241    |

This dataset represents hourly aggregated advertising data. In this example, the "Country" and "Operating system" columns are dimensions while the "Spend" and "Clicks" columns are KPIs. We can also define the derived KPI "Cost per click" as "Spend" divided by "Clicks".

# Detecting anomalies

We support a variety of rules and methods for detecting anomalies. Many of the following rules are configurable to meet specific business requirements.

#### 1. Absolute change

An absolute change anomaly is when a KPI value deviates by a fixed, absolute amount from a specified offset.

##### Example use

- If we are interested in monitoring our spending, we could detect if the total "Spend" for the day rose by more than 100 compared to the total "Spend" for the same day last week.
- For each "Country", we can detect if "Clicks" drops by more than 100 from the same hour the previous day to make sure we are reaching users everywhere.

#### 2. Percentage change

A percentage change anomaly occurs when a KPI value deviates by a configurable percentage from a specified offset.

##### Example use

- As our example company grows, a fixed rise of 100 in "Spend" may not be as notable. Switching to detect percentage change anomalies in "Spend" could be more interesting. 

#### 3. Threshold

A threshold anomaly is when a KPI value exceeds a defined absolute threshold. The detection can be configured to use a maximum, minimum, or both.

##### Example use

- We can detect if "Clicks" drops below 50 for any "Country" value to make sure our ad campaigns are reaching real users.
- To make sure we are being cost effective, we can detect if "Cost per click" ever exceeds 1. 

#### 4. Mean-variance

To detect a mean-variance anomaly, we examine historical changes in your data and label time points as anomalous when they are unexpectedly high or low based on the mean and standard deviation of the historical changes. 

Mean-variance anomalies are useful when trying to find anomalies in noisy data. Our previous anomaly types are not sensitive to past data outside of the comparison offset. Mean-variance anomalies take into account how your data has been changing and will be more robust to seasonalities.

##### Example use

- If "Spend" has a large amount of day-to-day noise, we can detect mean-variance anomalies to notice when "Spend" changes by a historically unusual amount.

#### 5. Holt-Winters

Holt-Winters seasonal method is a forecasting method that describes trends in data using three components: the baseline "level" of the data, the current "trend" of linear increase or decrease of the data, and a "seasonal" component that captures a repeating cyclic change in the data. We use this seasonal method to predict individual time points and compare them to your actual data. We detect anomalies when your actual time point deviates too much from the forecasted time point.

##### Example use

- "Clicks" has a high 24-hour seasonality; when people are asleep, they do not click on ads. So, our data can move from a high number in the middle of the day to near zero in the middle of the night. Using an absolute or percentage change filter to notice unusual KPI movement would not work well. Instead, we can use Holt-Winters to more accurately identify anomalies in this type of data.

# Anomaly filters

BoostKPI supports filtering out detected anomalies to help get you the most meaningful anomalies.

#### 1. Absolute change

Anomalies can be filtered out if they do not correspond to a change of at least a certain absolute amount.

##### Example use

- When detecting percentage change anomalies, if a KPI value for a particular dimension value changes from 2 to 3, that corresponds to a large 50% change, but a very small absolute change. When other dimension values have KPI values in the hundreds or thousands, filtering out this relatively small dimension value can be done with an absolute change filter.

#### 2. Percentage change

Anomalies can be filtered out when they do not cover times when KPIs increased or decreased by at least a certain percentage.

##### Example use

-  This percentage change filter can help filter out threshold anomalies that are the result of slow, steady increases as opposed to unusual jumps.

#### 3. Threshold

Anomalies can be filtered out when their KPI value falls outside an absolute threshold. This can be done based on either the total KPI value for the anomaly or based on the hourly or daily average KPI value for the anomaly.

##### Example use

- If "Clicks" had an anomalous drop, but is still at an acceptable level we can filter out the anomaly. This might happen if "Clicks" goes up an unsually high amount. The drop back to normal might not be as notable.

#### 4. Duration

To prevent alerting on transient anomalies, BoostKPI can filter out anomalies that do not last for a minimum duration.

##### Example use

- If a mean-variance anomaly lasts only for a single time point, it may not be meaningful. We can filter out these anomalies based on how long they last.

#### 5. Sitewide impact

Occasionally, anomalies for one KPI may not be meaningful if they do not cause changes in another KPI. Our sitewide impact filter enables you to filter out anomalies in one KPI based on changes in another KPI.

##### Example use

- If "Spend" drops significantly and is detected as an anomaly, but "Clicks" remains unchanged, a sitewide impact filter can remove this anomaly.

# Additional features

#### Dimension exploration and filtering

BoostKPI supports detecting anomalies in your data broken down according to a set of dimensions which can either be manually specified or automatically generated. Using this feature will enable you to detect anomalies in important categories that might not be visible in fully aggregated data.

For example, in our advertising dataset, we could detect anomalies in "Spend" broken down by "Country" on the Android operating systems.

When a dimension has a very high number of dimension values, the signal-to-noise ratio of anomalies can drop. To handle, this BoostKPI provides several ways to limit which dimension values are examined for anomalies. We support filtering to dimension values that are in the top 50 in contributing to a KPI's total, dimension values that contribute a certain percentage to a KPI's total, or dimension values whose KPI value is above or below configurable thresholds.

#### Time

While your dataset may have a particular time granularity, the advertising example has an hourly granularity, you may wish to detect anomalies at a different granularity. BoostKPI supports this use case and allows you to detect analyze your data at coarser granularity. Currently we support aggregating up to weekly granularities for all detection types and up to monthly granularities for some detection types. Because month durations vary, some detection types can not reasonably handle aggregating to monthly granularity.

#### Severity labels

To better aid you in identifying important anomalies, BoostKPI supports labelling anomalies with severties. These severity levels are configurable based on duration and change in KPI value.

#### Automatic merging

Occasionally, unusual data or complicated detection rules can result in BoostKPI detecting anomalies that either overlap or are very close to each other. BoostKPI can optionally merge anomalies based on their duration and separation to ensure you receive a concise picture of anomalies in your data.

#### Composite alert grouping

When business goals are complicated, more sophisticated detection rules are needed. BoostKPI's alerting system supports combining multiple detection rules to meet your needs. In our advertising example, you could detect anomalies when "Spend" has risen by a percentage, but "Clicks" has remain unchanged combining a percentage change and threshold anomaly detection.

#### Offset options

When comparing to past data such as when detecting percentage change anomalies, BoostKPI supports a variety of offsets. The simplest options are basic time based offsets such as comparing the current time point to the time point from one hour, day, or week ago, but we also support more complex offsets like comparing to the mean of the last seven days, median of the last four weeks, or maximum of the last month.

# Example configurations

Currently BoostKPI uses yaml files to configure alerts. In this section, we include some example configuration files. While we are working on moving towards a better user experience, these example files are intended to help you create new alerts or edit previous ones in the short term. If you have any questions about options not shown here or extending these examples to your use case, please do not hesitate to reach out.

## Detection configurations

A detection that looks for large weekly absolute change along a dimension breakdown.

```
detectionName: 'daily_absolute_change'
description: 'This is a daily detection that looks at the absolute change of metric compared to the average of the last four weeks broken down by device type and country'

metric: metric
dataset: application.dataset

dimensionExploration: # Run this detection on a breakdown of dimension(s)
  dimensions: # The names of the dimensions to breakdown by
  - device_type
  - country
  # Examples of optional filtering parameters:
  k: 10 # Only run on the top 10 combinations of device_type and country
  minContribution: 0.15 # Only run on the device_type and country combinations that contribute 15% (0.15*100) to the total of metric
  minValue: 12 # Only run on device_type and country combinations that have a metric value of at least 12
  maxValueDaily: 100 # Only run on device_type and country combinations that have a daily metric value of at most 100

rules:
- detection:
  - name: detection_rule_1
    type: ABSOLUTE_CHANGE_RULE
    params:
      # The absoluteChange, pattern, and offset configure the test that the detection performs
      absoluteChange: 15 # Detect changes of 15...
      pattern: UP_OR_DOWN # in either direction...
      offset: mean4w # comparing with the average of the same day the last four weeks
      # The following settings monitoringGranularity, bucketPeriod, windowUnit, and windowSize
      # configure this alert to monitor at a daily level
      monitoringGranularity: 1_DAYS
    bucketPeriod: P1D
    windowUnit: DAYS
    windowSize: 1
```

A rolling detection that looks at percentage changes over the past seven days 

```
detectionName: 'rolling_7_days_percentage_change'
description: 'This is a rolling detection executed daily that looks at the percent change in metric for the last seven days'

metric: metric
dataset: application.dataset
  
rules:
- detection:
  - name: detection_rule_1
    type: PERCENTAGE_RULE
    params:
      # The percentageChange, pattern, and offset configure the test that the detection performs
      percentageChange: 0.1 # Detect changes of 10%...
      pattern: UP_OR_DOWN # in either direction...
      offset: wo1w # compared with the previous week
      # The following settings monitoringGranularity, bucketPeriod, windowUnit, and windowSize
      # configure this alert to monitor the last week of data each day
      monitoringGranularity: 7_DAYS # Tells the detection to group data to the weekly level
    bucketPeriod: P1D # Sets the detection to run daily
    windowSize: 7
    windowUnit: DAYS # These two set the data window to run the detection on

# By defauly alerts are merged, e.g., if a daily detection finds an anomaly on Monday and Tuesday in the same slice of data,
# the Monday and Tuesday anomalies will be merged. This behavior helps cut down on spam when you are already aware of an issue.
# However with rolling alerts, even if the anomalies start multiple days apart, they can still overlap. So merging can hide 
# meaningful alerts and we recommend disabling merging for rolling alerts.
merger:
  enableMerging: false
```

An AI powered detection to identify significant changes in important subdimensions

```
detectionName: 'daily_ai_powered'
description: 'This is a daily detection that uses AI to both discover important breakdown dimensions and identify anomalies in a select data segment filtered by an absolute change threshold'

metric: metric
dataset: application.dataset

filters: # Dimension filters limit the detection to only look at a segment of data along a subset of certain dimension values
  # This configuration limits the detection to finding anomalies in data from India and the US where the user was on an Android or iOS device
  country:
  - IN
  - US
  device_type:
  - Android
  - iOS

dimensionExploration: # Run this detection on a breakdown of dimension(s)
  # When no dimensions field is included, automatic dimension exploration is performed to identify important breakdown dimensions
  # Examples of optional filtering parameters:
  k: 10 # Only run on the top 10 dimension combinations
  minContribution: 0.15 # Only run on the dimension combinations that contribute 15% (0.15*100) to the total of metric
  minValue: 12 # Only run on dimension combinations that have a metric value of at least 12
  maxValueDaily: 100 # Only run on dimension combinations that have a daily metric value of at most 100

rules:
- detection:
  - name: detection_rule_1
    type: HOLT_WINTERS_RULE
    params:
      # The absoluteChange, pattern, and offset configure the test that the detection performs
      sensitivity: 4 # Sets the sensitivity for identifying anomalies. Higher means fewer anomalies identified, lower means more anomalies identified.
      pattern: UP_OR_DOWN
      # The following settings monitoringGranularity, bucketPeriod, windowUnit, and windowSize
      # configure this alert to monitor at a daily level
      monitoringGranularity: 1_DAYS
    bucketPeriod: P1D
    windowUnit: DAYS
    windowSize: 1
  filter:
  - name: filter_rule_1
    type: ABSOLUTE_CHANGE_FILTER
    params:# Filter out the anomaly unless...
      threshold: 100 # the metric value has changed by 100...
      pattern: UP_OR_DOWN # in either direction...
      offset: mean5d # compared with the average of the last five days
```

## Subscription configurations

A basic email alert subscription

```
subscriptionGroupName: 'notification_name'
application: application
# Send any new alerts every 30 minutes
cron: 0 0/30 0 ? * * *

# The list of detection names to subscribe to
subscribedDetections:
- 'detection_name_1'
- 'detection_name_2'
- 'detection_name_3'

alertSchemes:
- type: EMAIL
  params:
    recipients:
      to:
      - foo@boostkpi.com
      - bar@boostkpi.com
      cc:
      - baz@boostkpi.com

```

An email alert subscription with dimension-based routing

```
subscriptionGroupName: 'notification_name'
application: application
# Send any new alerts every 30 minutes
cron: 0 0/30 0 ? * * *

# List all alerts (detectionName) you want to subscribe to.
subscribedDetections:
- 'detection_name_1'
- 'detection_name_2'
- 'detection_name_3'

type: DIMENSIONS_ALERTER_PIPELINE
# Alert individuals based on the anomalies country dimension
# Note: the corresponding detection must do a country breakdown
dimensionRecipients:
  - dimensions:
      country: # Route anomalies with an "IN" value for the country dimension to:
      - IN
    notify:
      emailScheme:
        recipients:
          to:
          - india_recip1@boostkpi.com
          - india_recip2@boostkpi.com
  - dimensions:
      country: # Route anomalies with a "US" value for the country dimension to:
      - US
    notify:
      emailScheme:
        recipients:
          to:
          - us_recip1@boostkpi.com
          - us_recip2@boostkpi.com
alertSchemes:
- type: EMAIL
  params:
    recipients:
      to:
      - foo@boostkpi.com
      - bar@boostkpi.com
      cc:
      - baz@boostkpi.com
```

A Slack alert subscription

```
subscriptionGroupName: 'notification_name'
application: application
# Send any new alerts every 30 minutes
cron: 0 0/30 0 ? * * *

# List all alerts (detectionName) you want to subscribe to.
subscribedDetections:
- 'detection_name_1'
- 'detection_name_2'
- 'detection_name_3'

alertSchemes:
- type: SLACK
  params:
    webhooks:
      - >-
        https://hooks.slack.com/services/ABCDE123456/ABCDE123456/0123456789abcdefgABCDEFG
```

A Microsoft Teams alert subscription

```
subscriptionGroupName: 'notification_name'
application: application
# Send any new alerts every 30 minutes
cron: 0 0/30 0 ? * * *

# List all alerts (detectionName) you want to subscribe to.
subscribedDetections:
- 'detection_name_1'
- 'detection_name_2'
- 'detection_name_3'

alertSchemes:
- type: TEAMS
  params:
    webhooks:
       - 'https://boostkpi.webhook.office.com/webhookb2/abcd1234-ab12-cd34-ef56-a1b2c3d4e5f6@abcd1234-ab12-cd34-ef56-a1b2c3d4e5f6/IncomingWebhook/0123456abcdef0123456789abcdef0/abcd1234-ab12-cd34-ef56-abcd1234'
```
