## Geocode Addresses (Geocoding): https://pro.arcgis.com/en/pro-app/latest/tool-reference/geocoding/geocode-addresses.htm ##
- Can use individual fields (Street Line 1, Street Line 2, Street Line 3, City, State, ZIP, Country) or single field.
- # ArcGIS REST APIs / Geocoding Service / geocodeAddresses: https://developers.arcgis.com/rest/geocode/geocode-addresses/#
- Using the REST API allows for additional parameters that might be of interest:
  > matchOutOfRange: returns a match when the input house number is outside the street's house number range. ∴ Whereas a simple geocoder simply matches point addresses to points, Esri's methodology incorporates line segments as well.
  > comprehensiveZoneMatch: specifies if adjacent administrative zones (e.g. postal codes) should be disregarded when geocoding an address. ∴ Essentially this is like a loose blocking. Whereas a simple geocoder allows one to either block or not block based on standard fields (e.g. ZIP, State, County), Esri allows you to block by nearness.
## Tips for improving geocoding quality: https://pro.arcgis.com/en/pro-app/latest/help/data/geocoding/tips-for-improving-geocoding-quality.htm?utm_source=chatgpt.com ##
- Minimum match score: a threshold that allows the user to control how closely addresses must match their most likely candidate in the reference data to be considered a match.
- Minimum candidate score: determines which potential candidate(s) are returned for interactive geocoding.
- Comprehensive zone matching: allows addresses to match to cities that do not exactly match the input city name. ∴ This seems to be another instance where Esri does a pseudo-blocking. Instead of being a binary (e.g. either the city matches or it doesn't), it may be penalized heavily for differences.
## Geocoding options properties: https://desktop.arcgis.com/en/arcmap/latest/manage-data/geocoding/geocoding-options-properties.htm?utm_source=chatgpt.com ##
- "A match score between 85 and 99 can generally be considered a good match."
- Esri has a "Spelling Sensitivity" setting that "controls how many candidates the address locator considers" without affecting the actual match score of the candidate(s).
## ArcGIS Rest APIs / Geocidng service / findAddressCandidates: https://developers.arcgis.com/rest/geocode/find-address-candidates/?utm_source=chatgpt.com ##
- 

outstanding questions:
- what is returnMatchNarrative?
- how could i introduce a "match_only_one" type parameter where only one store matches to a given address.
  > would need to dedupe stores first
- should probably add a "match_closest" parameter or something like that in case a user wants all of their records to match to something, even if it isn't high quality.
- look at python record linkage packages to see what they use. might be helpful to use those for fields not being blocked on.
- might be helpful to make all fields optional...for example, if only matching within the same city or zip code, it might be nice to not need to write the city name to each record.
