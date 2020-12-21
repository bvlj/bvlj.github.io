---
layout: project
title:  "Diab"
subtitle: "Android app for diabetes management"
---

![diab]({{ site.baseurl }}/assets/images/projects/hero_diab.png)

**Diab** is a smart [open-source](https://github.com/bvlj/diab) application
that helps you manage your diabetes by keeping track of your glucose values
and insulin injections.

# Features

## Keep track of your glucose values

Keep all your glucose readings and related insulin injections dosages handy
by archiving them in the app so you can easily find them whenever you want.

## Insulin suggestions

Once you've gathered enough data inside the app, you'll be able to
[build](https://github.com/bvlj/diab/blob/staging/ml/Readme.md) your own insulin
suggestion plugin basing for smart and context-aware hints.

The plugin is created by using a machine-learning model created with data
from the app which is tested against a wide set of possible scenarios.

Beware that this is feature can cause serious injuries if misused. Do not
blindly rely on it nor take it as medical advice.

## Easily share your records

It's possible to export your data as a simple electronic file sheet so
you can easily share it to your doctor or consult it on your own computer.

_Note: Microsoft Excel, LibreOffice Calc or Google Sheets are required to open
the exported file_

## Fitness Services integration

Optionally, sync your data with your fitness service (Google Fit is available
at the time of writing) to better coordinate health tracking on your device.

_Note: this feature may not be available oss-only builds_

# Changelog

- **1.3.5** [2020-01-05]
  - Insulin management improvements
  - Android 10 support
  - Performance improvements
  - Bug fixes
- **1.3** [2019-01-26]
  - Redesigned UI for easier reachability
  - Export data as an electronic file sheet
  - Performance improvements
  - Bug fixes
- **1.2** [2018-12-07]
  - New plugin format
  - Dark mode
  - Low and high glucose thresholds are now customizable
  - Support for Google-Fit-less builds
  - Support for android 6.0 M
  - New icon
  - Performance improvements for low-end devices
  - Bug fixes
- **1.1** [2018-08-18]
  - Google Fit integration management
  - Better insulin manager
  - Insulin suggestions as external plugins
  - OnBoarding
  - Performance improvements
  - Bug fixes
- **1.0** [2018-06-19]
  - Save glucose and insulin records
  - Weekly overview
  - Insulin suggestions
  - Google Fit integration

<a href="https://f-droid.org/packages/it.diab"><img src="https://f-droid.org/badge/get-it-on.png" alt="Get it on F-Droid" height="80"></a>
