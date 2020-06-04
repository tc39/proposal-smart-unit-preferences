# Smart Unit Preferences in `Intl.NumberFormat` 

**Stage**: 0

**Champion**: Younies Mahmoud [@younies](https://github.com/younies)

**Author**: Younies Mahmoud [@younies](https://github.com/younies)

**[slide](https://bit.ly/smart-unit-preferences-in-intl-number-format)**

# Overview

Representing a quantity of a measurement unit for each locale is considered as a
critical issue in i18n.

 For example, people in the US tend to use Fahrenheit as
a measurement unit, whereas people in the UK tend to use Celsius. The
measurement units used also depend on what exactly is being measured (roads,
people, precipitation), not just on what kind of measurement it is (length,
mass, pressure).

For example, in the US people use miles for longer distances and feet for
shorter distances. For measuring how tall a person is, adults are measured in
feet and inches, whereas inches are sufficient for toddlers. We propose adding
locale-aware measurement formatting that takes these nuances into account.

# Requirements

1. **Localized:** for each locale, users should be presented with units
   appropriate for them. For example:
   + Length (distance):
     - `en-US`: miles
     - `fr-FR`: kilometers
   + Mass:
     - `en-US`: pounds
     - `fr-FR`: kilograms

1. **Usage:** the preferred measurement unit is dependent on what is being
   measured. Consider:
   + Typical length measurements:
     - `en-US`: miles, feet, inches
     - `fr-FR`: kilometers, meters, centimeters
   + Rainfall (also a "length" measurement):
     - `en-US`: inches
     - `fr-FR`: millimeters
       - Yearly rainfall is still presented in millimeters, even when
         measurements exceed 1 meter. Consider the rainfall in Cherrapunji:
         _"The highest recorded rainfall in a single year was 22,987 mm (905.0
         in) in 1861."_ (Wikipedia: [Rain - Wettest known
         locations](https://en.wikipedia.org/wiki/Rain#Wettest_known_locations)).

1. The choice of presentation units also depends on the size of the measurement:
   + For `en-US`, the length of toddlers is typically presented in **inches**,
     whereas adults are measured **feet and inches**. (3 feet seems a reasonable
     threshold.)
     - *The average newborn is 19.5 inches long and weighs 7.25 pounds.*
     - *Albert Einstein was 5 feet and 9 inches tall.*

1. The **precision** of the displayed number also depends on usage:
   + For `en-US`, babies would be measured to a tenth of an inch, whereas
     measurements presented in feet and inches are rounded to the nearest inch.
     are measured to the nearest inch only.
   + In metric countries, heights would be measured in centimeters, or in meters
     presented with two decimal places.

# API Design

* Add a `usage` string parameter to `Intl.NumberFormat` that indicates the
  measurement use case.
  - Examples: `person-height`, `rainfall`, `snowfall`, `road`.
  - Output units and precision will depend on:
    - Input unit (helps disambiguate the usage)
    - Usage
    - Locale
    - Quantity
  - We propose supporting CLDR's unit preferences, see [CLDR's
    units.xml](https://github.com/unicode-org/cldr/blob/.master/common/supplemental/units.xml).

* A code example adding a `usage` parameter to
  [Int.NumberFormat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/NumberFormat/NumberFormat):

  ```javascript
  const inputValue = 1.8;
  const inputUnit = "meter";

  console.log(new Intl.NumberFormat('en-US', {
      style: 'unit',
      unit: inputUnit,
      usage: 'person-height',  // NEW parameter
      unitDisplay: 'long'
  }).format(inputValue));
  // expected output: "5 feet and 11 inches"
  // (1.8 meters = 5 feet 10.8661 inches")
  ```
