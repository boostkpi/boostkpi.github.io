---
layout: post
title:  "Ad Exchange Use Case"
categories: [ Overview ]
image: assets/images/analysis-amico.svg
beforetoc: ""
toc: false
---

Ad Exchanges are a key part of the ad tech world. They conduct real-time auctions on tens of millions of ad requests every second. These ad requests originate from millions of (mobile) apps or websites. The exchanges forward them to a subset of DSPs connected to them. (For example, a European-market focused DSP may choose to receive ad requests only from users in the EU.) The DSPs bid on these ad requests on behalf of their advertisers. The exchanges then select the ad with the highest bid that complies with the publisher rules.

Ad exchanges track many KPIs on an hourly or daily basis:
- Spend: the dollars spent by the DSP
- Ad requests: the number of ad requests sent to the DSP
- Ad responses: the number of ad responses sent by the DSP
- Wins: the number of auctions the DSP won.
- Impressions: the number of impressions that the DSP won.
- Clicks: the number of clicks that resulted.

Exchanges break down these KPIs by several dimensions:
- country
- operating_system (iOS/Android)
- os version
- adomain (the domain of the advertiser like disneyplus.com)
- DSP
- ad_format (typical formats are banner, native, interstitial or video)
- publisher (like zynga)
- app_id (like the android app id of Words with Friends)
- integration_method (sdk or server-to-server)
- campaign_id
- creative_id
- seat_id

Determining the root cause of “why did the spend for a  particular DSP drop by 5% week-over-week?” is currently a huge challenge.  It requires a lot of slicing and dicing by skilled data analysts. Often, there can be significant changes in subsets of data, which don’t get reflected in the total. Imagine 5% of a DSP’s spend is from advertising the Disney plus app on Zynga’s words-with-friends app.  If Zynga's publisher account manager flags a Disney-plus creative, the spend will stop. Current alerting systems, which alert on totals, will not detect this issue. Detecting this issue and getting the creative blocking reversed could be a huge boon. The monthly impact could exceed a million dollars.

BoostKPI alerts the DSP account managers (AMs) as soon as an anomaly appears in any subset of the data. For example, spend on a particular adomain dropped by 50%. Or, the CTR of particular video creatives went to zero on specific SDK versions. AMs can fine tune the results to improve the signal-to-noise ratio. AMs can layer business rules on top of ML-powered anomalies. Don’t send an alert if the spend impact is less than ten thousand dollars or if the event is transient. AMs can configure [personalized and smart alerts](https://blog.boostkpi.com/BoostKPIs-alerting-features/).

Doing Quarterly Business Reports (QBRs) with BoostKPI is also a breeze. AMs can compare a DSP's last quarter’s total spend against this quarter’s total spend.
BoostKPI highlights publishers, apps, and countries that have seen significant spend changes. It even shows combinations that have seen significant spend changes. AMs can repeat the analysis for weekly or monthly data.

Several big exchanges already use BoostKPI. Their AMs are more productive. Their data teams have to do less repetitive work. They don't miss any "unknown unknowns." [Contact us for a demo.](https://www.boostkpi.com/#1#schedule)
