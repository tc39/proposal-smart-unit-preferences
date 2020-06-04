# Smart Unit Preferences in `Intl.NumberFormat` 

**Stage**: 0

**Champion**: Younies Mahmoud [@younies](https://github.com/younies)

**Author**: Younies Mahmoud [@younies](https://github.com/younies)

**[slide](https://bit.ly/smart-unit-preferences-in-intl-number-format)**

# Overview

Representing a quantity of a measurement unit for each locale is considered as a critical issue in i18n. For example, people in the US tend to use Fahrenheit as a measurement unit, however, people in the UK tend to use Celsius and another tends to use Kelvin.  Also, even in the same locale, and the same measurement type, people use different measurement units for different contexts. For example, in the US people use miles for a distance that is more than a quarter-mile and feet for less than that. Also, in the US, people tend to use feet and inches for the height of the adults and only feet for children. 
Therefore, in this proposal, we will present a new functionality that supports a Locale-aware Measurement Unit Formatting. Also, our design should be ready for future extensions to support inflected language and other customisations.

# Requirements

TBD

# API Design

Add `usage` function to number format that takes `string` represent the usage, for example: `"person"` . Therefore, the output units will be determined based on:

* locale
* usage
* input unit

For example, if the following scenario:

* locale
  + `"en-US"` 
* usage
  + `"person"` 
* input unit
  + `"meter"` 
* the output unit will be `"foot+inch"` , because those are the used units in the united states for measuring the person heights.
