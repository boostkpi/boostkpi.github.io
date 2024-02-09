---
layout: post
title:  "Iteratively Defining Datasets in BoostKPI"
categories: [ Overview ]
image: assets/images/dataset-iteration-pana.svg
alt: "Iteratively Defining Datasets in BoostKPI. Business Illustrations by StorySet"
beforetoc: ""
toc: false
---

The traditional use case for BoostKPI is an organization with well-defined KPIs. Its leaders are using visualization tools like tableau and PowerBI to slice and dice the data. The utility of BoostKPI in such cases is well understood. First, business users no longer have to manually look for important changes in their business. Second, data analysts no longer need to manually slice-and-dice their data. Third, all users can receive granular alerts that are customized to their needs.

Many other businesses don’t have such stable KPIs and a concrete list of dimensions to slice and dice by. They have a few dimensions in mind but are looking to expand the list. (Please consult [our documentation](https://docs.boostkpi.com/docs/data-import/guide/#importing-data-into-boostkpi) for any data modeling tips.) We advise such organizations to use BoostKPI in an iterative manner, something akin to Test-Driven-Development (TDD). In TDD, the developer writes the tests along with or even before the actual code. Similar to TDD, BoostKPI can speed up the process of an organization deciding what KPIs to monitor and what set of dimensions to use.

For example, one of our fintech customers was planning to use BoostKPI to monitor the delinquency rates among its customers. The rate could be sliced and diced by geography, the user’s credit score, as well as other internal criteria their data science had come up with. They started by using BoostKPI with a few dimensions, then expanded the list of dimensions over time.

We have really made it easy for our customers to use BoostKPI iteratively. Specifically, [BoostKPI supports a one-off mode for importing data](https://docs.boostkpi.com/docs/data-import/guide/#importing-data-into-boostkpi). Once you are happy with the KPIs and the dimensions, you can choose to import the dataset as an ongoing import.
