# Data Dictionary

### Chicago crime data

- unique_key: Unique identifier for the record.
- case_number: The Chicago Police Department RD Number (Records Division Number), which is unique to the incident.
- date: Date when the incident occurred. this is sometimes a best estimate.
- block: The partially redacted address where the incident occurred, placing it on the same block as the actual address.
- iucr: The Illinois Unifrom Crime Reporting code. This is directly linked to the Primary Type and Description.
- primary_type: The primary description of the IUCR code.
- description: The secondary description of the IUCR code, a subcategory of the primary description.
- location_description: Description of the location where the incident occurred.
- arrest: Indicates whether an arrest was made.
- domestic: Indicates whether the incident was domestic-related as defined by the Illinois Domestic Violence Act.
- beat: Indicates the beat where the incident occurred. A beat is the smallest police geographic area – each beat has a dedicated police beat car. Three to five beats make up a police sector, and three sectors make up a police district. The Chicago Police Department has 22 police districts.
- district: Indicates the police district where the incident occurred.
- ward: The ward (City Council district) where the incident occurred.
- community_area: Indicates the community area where the incident occurred. Chicago has 77 community areas.
- fbi_code: Indicates the crime classification as outlined in the FBI's National Incident-Based Reporting System (NIBRS).
- x_coordinate: The x coordinate of the location where the incident occurred in State Plane Illinois East NAD 1983 projection. This location is shifted from the actual location for partial redaction but falls on the same block.
- y_coordinate: The y coordinate of the location where the incident occurred in State Plane Illinois East NAD 1983 projection. This location is shifted from the actual location for partial redaction but falls on the same block.
- year: Year the incident occurred.
- updated_on: Date and time the record was last updated.
- latitude: The latitude of the location where the incident occurred. This location is shifted from the actual location for partial redaction but falls on the same block.
- longitude: The longitude of the location where the incident occurred. This location is shifted from the actual location for partial redaction but falls on the same block.
- location: The location where the incident occurred in a format that allows for creation of maps and other geographic operations on this data portal.

### Weather data

- stn: Station number (WMO/DATSAV3 number) for the location
- wban: WBAN number where applicable--this is the historical "Weather Bureau Air Force Navy" number
- year: The year
- mo: The month
- da: The day
- temp: Mean temperature for the day in degrees Fahrenheit to tenths. Missing = 9999.9
- count_temp: Number of observations used in calculating mean temperature
- dewp: Mean dew point for the day in degreesm Fahrenheit to tenths.  Missing = 9999.9
- count_dewp: Number of observations used in calculating mean dew point
- slp: Mean sea level pressure for the day in millibars to tenths. Missing = 9999.9
- count_slp: Number of observations used in calculating mean sea level pressure
- stp: Mean station pressure for the day in millibars to tenths. Missing = 9999.9
- count_stp: Number of observations used in calculating mean station pressure
- visib: Mean visibility for the day in miles to tenths.  Missing = 999.9
- count_visib: Number of observations used in calculating mean visibility
- wdsp: Mean wind speed for the day in knots to tenths. Missing = 999.9
- count_wdsp: Number of observations used in calculating mean wind speed
- mxpsd: Maximum sustained wind speed reported for the day in knots to tenths. Missing = 999.9
- gust: Maximum wind gust reported for the day in knots to tenths. Missing = 999.9
- max: Maximum temperature reported during the day in Fahrenheit to tenths. Missing = 9999.9
- min: Minimum temperature reported during the day in Fahrenheit to tenths. Missing = 9999.9
- prcp: Total precipitation (rain and/or melted snow) reported during the day in inches and hundredths. Missing = 99.99
- sndp: Snow depth in inches to tenths--last report for the day if reported more thanonce. Missing = 999.9
- fog: Indicators (1 = yes, 0 = no/not reported) for the occurrence during the day
- rain_drizzle: Indicators (1 = yes, 0 = no/not reported) for the occurrence during the day
- snow_ice_pellets: Indicators (1 = yes, 0 = no/not reported) for the occurrence during the day
- hail: Indicators (1 = yes, 0 = no/not reported) for the occurrence during the day
- thunder: Indicators (1 = yes, 0 = no/not reported) for the occurrence during the day
- tornado_funnel_cloud: Indicators (1 = yes, 0 = no/not reported) for the occurrence during the day
- usaf: Air Force station ID
- country: country
- state: state in US
- lat: Latitude in thousandths of decimal degrees
- lon: Longitude in thousandths of decimal degrees
- elev: Elevation in meters
- begin: Beginning Period Of Record (YYYYMMDD)
- end: Ending Period Of Record (YYYYMMDD)
