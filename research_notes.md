## Geocode Addresses (Geocoding): https://pro.arcgis.com/en/pro-app/latest/tool-reference/geocoding/geocode-addresses.htm ##
- Can use individual fields (Street Line 1, Street Line 2, Street Line 3, City, State, ZIP, Country) or single field.
## ArcGIS REST APIs / Geocoding Service / geocodeAddresses: https://developers.arcgis.com/rest/geocode/geocode-addresses/ ##
- Using the REST API allows for additional parameters that might be of interest:
  > matchOutOfRange: returns a match when the input house number is outside the street's house number range. ∴ Whereas a simple geocoder simply matches point addresses to points, Esri's methodology incorporates line segments as well.
  > comprehensiveZoneMatch: specifies if adjacent administrative zones (e.g. postal codes) should be disregarded when geocoding an address. ∴ Essentially this is like a loose blocking. Whereas a simple geocoder allows one to either block or not block based on standard fields (e.g. ZIP, State, County), Esri allows you to block by nearness.
## Tips for improving geocoding quality: https://pro.arcgis.com/en/pro-app/latest/help/data/geocoding/tips-for-improving-geocoding-quality.htm?utm_source=chatgpt.com ##
- Minimum match score: a threshold that allows the user to control how closely addresses must match their most likely candidate in the reference data to be considered a match.
- Minimum candidate score: determines which potential candidate(s) are returned for interactive geocoding.
- Comprehensive zone matching: allows addresses to match to cities that do not exactly match the input city name. ∴ This seems to be another instance where Esri does a pseudo-blocking. Instead of being a binary (e.g. either the city matches or it doesn't), it may be penalized heavily for differences.
## Tips for improving geocoding performance: https://pro.arcgis.com/en/pro-app/latest/help/data/geocoding/tips-for-improving-geocoding-performance.htm ##
- Set a lower default for maximum number of candidates.
## Geocoding options properties: https://desktop.arcgis.com/en/arcmap/latest/manage-data/geocoding/geocoding-options-properties.htm?utm_source=chatgpt.com ##
- "A match score between 85 and 99 can generally be considered a good match."
- Esri has a "Spelling Sensitivity" setting that "controls how many candidates the address locator considers" without affecting the actual match score of the candidate(s).
## ArcGIS Rest APIs / Geocidng service / findAddressCandidates: https://developers.arcgis.com/rest/geocode/find-address-candidates/?utm_source=chatgpt.com ##
- Esri separates ZIP code and the +4 extension. That would probably be a good thing to mimic.
- Esri's REST API allows for the return of all locations that meet the minimum candidate criteria (or a maximum number inputted by the user). Again, this would likely be a good thing to mimic.
- The "preferredLabelValues" parameter denotes which locality name (i.e. postal city) is provided. An example might be to output "Kansas City" values even for addresses in Olathe, or Pearblossom Highway instead of CA-138. It might be helpful to output both - one "official" and one "human".
  > Parameters:
    - postalCity
    - localCity
    - matchedCity
    - primaryStreet
    - matchedStreet
- The "returnMatchNarrative" parameter describeshow the geocoding result was obtained.
  > Output Options:
    - classified: input word or part was recognized as an an address value from the reference data, or an indicator of some kind.
    - matched: input word or part was matched to an address value in the reference data.
    - inexact: input word or part is spelled differently than what it matched to (e.g. avanue vs avenue).
    - partial: the input part matched to a portion of a value in the reference data.
    - splitting: indicates that a match was made to a value in the reference data by splitting one word from the input into multiple words, or by ocncatenating multiple words from the input into a single word (e.g. Mountain View vs Mountainview).
    - aliased: the input part matched to an alias of a reference data value (e.g. Blvd vs Boulevard).
    - repositioned: indicates that the input word was matched to a value in the reference data but in the wrong part of the overall address (e.g. North Main Street vs Main Street North).
    - different: indicates that the input part was recognized as a particular address component but matched to a different reference data value for that component (e.g. Morrison Street vs Morrison Avenue).
    - nearby: indicates that the input part matched to a postal code or administrative zone which is adjacent to the postal code or zone that the geocoded address is actually within (e.g. 66047 vs. 66048).
    - extrapolated: indicates that the input part matched outside of the house number range for a street address record in the reference data (e.g. a 399 address matching to a road segement that only has 300 - 350).
  > It looks like the backend has addresses parsed as far as possible. Each target is then compared to each element to see if (1) it can be classified as part of a standard address, (2) if it can be matched to an address in the target dataset. This seems like a very good methodology to replicate, though it may slow the process down a bit.
## What's included in the geocoded results: https://pro.arcgis.com/en/pro-app/latest/help/data/geocoding/what-is-included-in-the-geocoded-results-.htm?utm_source=chatgpt.com ##
- Subaddress includes apartments, suites, etc.
- Point Address represents the physical house or building.
- Street Address is interpolated based on the house number range on the street and the locations of those houses.
## Introduction to lactor properties and options: https://pro.arcgis.com/en/pro-app/latest/help/data/geocoding/introduction-to-locator-properties-and-options.htm ##


outstanding questions:
- how could i introduce a "match_only_one" type parameter where only one store matches to a given address.
  > would need to dedupe stores first
- should probably add a "match_closest" parameter or something like that in case a user wants all of their records to match to something, even if it isn't high quality.
- look at python record linkage packages to see what they use. might be helpful to use those for fields not being blocked on.
- might be helpful to make all fields optional...for example, if only matching within the same city or zip code, it might be nice to not need to write the city name to each record.
