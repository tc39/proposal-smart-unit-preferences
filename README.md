# Smart Unit Preferences in `Intl.NumberFormat` 

**Stage**: 0

**Champion**: Younies Mahmoud [@younies](https://github.com/younies)

**Author**: Younies Mahmoud [@younies](https://github.com/younies)

**[slide](https://bit.ly/smart-unit-preferences-in-intl-number-format)**

# Overview

Representing a quantity of a measurement unit for each locale is considered as a
critical issue in i18n. For example, people in the US tend to use Fahrenheit as
a measurement unit. However, people in the UK tend to use Celsius and another
tends to use Kelvin.  Also, even in the same locale, and the same measurement
type, people use different measurement units for different contexts. For
example, in the US people use miles for a distance that is more than a
quarter-mile and feet for less than that. Also, in the US, people tend to use
feet and inches for the height of the adults and only feet for children.
Therefore, in this proposal, we will present a new functionality that supports a
Locale-aware Measurement Unit Formatting.

# Requirements

1. For each locale, users should perceive the appropriate units for them.
   + Example (Distances):
     - in `en-US`
       * Users, in general, perceive the distances in `miles`
     - in `fr-FR`
       * Users, in general, perceive the distances in `kilometers`
   + Example (Weight):
     - in `en-US`
       * Users, in general, perceive the weight in `pounds`
     - in `fr-FR`
       * Users, in general, perceive the weight in `kilograms`

1. In each locale, users tend to select an appropriate unit for **each usage**:
   + Example (Person Height)
     - in `en-US` 
       * Users, in general, perceive *person heights* in `feet and inches` 
    - in `fr-FR` 
      * Users, in general, perceive *person heights* in `meters` 

1. In each locale, for each usage, users adjust their unit preferences depending
   on **the values**
   + Example (Person Height)
     - in `en-US` 
        * Users tend to use `inches` with the persons that are less than `2 feet
          (24 inches)` . And from `2 feet` , they tend to use `feet and inches`
          .
        * For instances
          * *the child is born 11 inches tall*.
          * *this person is 5 feet and 3 inches tall*
    - in `fr-FR` 
      * Users tend to use `centimeters` with the persons that are less than `1
        meter (100 centimeters)` . And from `1 meter` , they tend to use
        `meters` .
      * For instances:
        * *l'enfant est né de 60 centimètres de haut*.
        * *cette personne mesure 1,76 mètres*

1. In each locale, for each usage, for specific value range, users could
   perceive specific **accuracy**
    + Example (Person Height)
      - in `en-US` 
        * Users tend to round persons height more than `2 feet (24 inches)` to
          the nearest `inch` .
        * For instances
          * `3 feet and 2.6 inches` would be `3 feet and 3 inches` 
      - in `fr-FR` 
        * Users tend to round persons height more than `1 meter (100
          centimeters)` to the nearest `0.01` meter.

# API Design

* Add `usage` function to number format that takes `string` represent the usage,
  for example: `"person"` . Therefore, the **output unit/units** and **the
  accuracy of the output value** will be determined based on:
  + locale
  + usage
  + input unit
  + the value of the unit

* For example, if the following scenario:
  * locale
    - `"en-US"` 
  * usage
    - `"person"` 
  * input unit
    - `"meter"` 
  * the input value is `1.8 meters` 

``` javascript
  const inputValue = 1.8;

  console.log(new Intl.NumberFormat('en-US', {
      unit: 'meter',
      usage: 'person',
      unitDisplay: 'long'
  }).format(inputValue));
  // expected output: "5 feet and 11 inches"
  // NOTE: 1.8 meter equals to "5 feet and 10.866141732283452 inches"
```
