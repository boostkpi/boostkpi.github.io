---
layout: post
title:  "Example BoostKPI Alert Configurations"
categories: [ Overview ]
image: assets/images/Warning-rafiki.svg
beforetoc: ""
toc: false
---

Currently, BoostKPI uses yaml files to configure alerts. In a [previous post, we provided an overview of the alert configurations] ({%post_url 2022-01-06-BoostKPIs-alerting-features %}) BoostKPI provides. In this section, we include some example configuration files. While we are working on moving towards a better user experience, these examples are intended to help you create new alerts or edit previous ones in the short term. If you have any questions about options not shown here or extending these examples to your use case, please do not hesitate to reach out.

The detection and subscription examples below use general names throughout. When building your own alert, update naming appropriately; application is your company's domain, metric is the KPI you want to monitor, and dataset is the dataset containing that metric. For example, if your company's domain is "widget_co" and you want to monitor "revenue" in your "sales_data" dataset. Then you would replace:

```
metric: metric
dataset: application.dataset
```

with:

```
metric: revenue
dataset: widget_co.revenue
```

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
