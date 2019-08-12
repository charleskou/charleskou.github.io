---
title: Grafana
date: 2019-08-10 13:40:21
tags: TODO, 工具
---

## Time series
- return column named time or time_sec (in UTC), as a unix time stamp or any sql native date data type. You can use the macros below.
- return column(s) with numeric datatype as values
Optional:
  - return column named metric to represent the series name.
  - If multiple value columns are returned the metric column is used as prefix.
  - If no column named metric is found the column name of the value column is used as series name
Resultsets of time series queries need to be sorted by time.

## 时间选择

