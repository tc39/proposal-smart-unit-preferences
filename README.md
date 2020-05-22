# Proposal for Adding `usage` to `Intl.NumberFormat` 

**Stage**: 0

**Champion**: Younies Mahmoud [@younies](https://github.com/younies)

**Author**: Younies Mahmoud [@younies](https://github.com/younies)

**[slide](https://bit.ly/intl-number-format-usage)**

# Overview

Representing a quantity of a measurement unit for each locale is considered as a critical issue in i18n. For example, people in the US tend to use Fahrenheit as a measurement unit, however, people in the UK tend to use Celsius and another tends to use Kelvin.  Also, even in the same locale, and the same measurement type, people use different measurement units for different contexts. For example, in the US people use miles for a distance that is more than a quarter-mile and feet for less than that. Also, in the US, people tend to use feet and inches for the height of the adults and only feet for children. 
Therefore, in this proposal, we will present a new functionality that supports a Locale-aware Measurement Unit Formatting. Also, our design should be ready for future extensions to support inflected language and other customisations.

