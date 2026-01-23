# Smart Unit Preferences in `Intl.NumberFormat` 

**Stage**: 1

**Champion**: Ben Allen [@ben-allen](https://github.com/ben-allen)

**Author**: Ben Allen [@ben-allen](https://github.com/ben-allen)

**[slides](https://docs.google.com/presentation/d/13ARM2V8v-zdrlywl3CYl_Eqca61vst6N5BVr4b0qmPA/edit?usp=sharing)**

# Overview

Representing measurements in localized units is a common need when localizing content. Most notably, the United States uses the idiosyncratic Imperial system for many measurements, whereas most of the rest of the world uses the metric system. Likewise, temperatures in the United States are generally measured in degrees Fahrenheit, whereas Celsius is used for most temperature measurements in most of the world.

However, properly localizing measurements isn't as simple as "give the U.S. measurements in Imperial, everyone else gets metric." This is because sometimes different measurement scales are used within regions depending on the type of thing that's being measured, the context in which the measurement is used, and on the value of the measurement itself. For example, although Canada largely uses the metric system for measuring lengths, Canadians typically give their heights in feet and inches. Often differently scaled units will be used for varying road distances; for example, countries that use feet and miles will typically give shorter road distances in terms of feet and longer ones in terms of miles or fractions of miles. The degree of precision used when representing distance measurements also varies based on the size of the measurement, with precision generally decreasing as distance increases.

Most often the need for context- and value-dependent measurement localization arises in regions formerly part of the British Empire. However, it is not *exclusive* to these regions. For example, the "Scandinavian mile" is used in Sweden in some contexts. This unit, equivalent to ten kilometers, is frequently used when talking colloquially about distances traveled. However, it is also used in some more formal contexts. Vehicle fuel consumption is most commonly measured in liters per Scandinavian mile, and some Swedish Tax Agency forms use measurements in Scandinavian miles when determining distances traveled for business purposes.

We propose adding locale- and usage-aware measurement formatting to ECMAScript's Intl.NumberFormat as a framework for providing users with measurements in properly localized scales. We intend to leverage the data available in CLDR's [supplemental file units.xml](https://github.com/unicode-org/cldr/blob/main/common/supplemental/units.xml)


# Requirements

1. **Localized:** for each locale, users should be presented with units
   appropriate for them. For example:
   + Length (distance):
     - `en-US`: miles for distances over half a mile, feet for shorter distances
     - `fr-FR`: kilometers
   + Mass:
     - `en-US`: pounds
     - `fr-FR`: kilograms
   + Mass of a person:
     - `en-US`: pounds
     - `fr-FR`: kilograms
     - `en-GB`: stone-and-pound
   + Person-heights:
     - `en-US`: feet-and-inches
     - `fr-CA`: feet-and-inches
     - `fr-FR`: meters-and-centimeters
     - `de-DE`: centimeters
   + Rainfall (also a "length" measurement):
     - `en-US`: inches
     - `fr-FR`: millimeters

All measurements should be at the precision appropriate for measurements of the 

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
