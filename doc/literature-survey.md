# Literature Survey
> LI LINGYU, LIU YICHAO, WU JINGYAN, YANG QINGSHAN, YE FANGDA (Group 5)

## Summary of Existing Systems and Their Effectiveness

Currently, three predictive policing systems stand out for their widespread application and research value: PredPol and HunchLab in US, and Crime Anticipation System (CAS) in Netherlands.

PredPol, initially introduced in 2012, originated as a research project at the University of California. It has since evolved into the most widely implemented predictive policing algorithm in the United States. The system leverages three simple primary types of historical data: crime type, crime location, and crime time. By analysing these data, PredPol generates crime risk maps that indicates areas with high crime probability. The algorithm has proven effective in optimizing police resource allocation and reducing crime rates in certain areas.

HunchLab is another popular predictive policing system in US but more complicated. It aims to predict how likely a particular crime type is to occur at various locations across a time period. HunchLab is based on machine learning and requires non-crime data. Same as PredPol, HunchLab will first collect crime related event data including location and time. The system can also process complicated geographic data contains points, lines and polygons, temporal data sets like weather data and school schedule, census data like house vacancy status, and natural terrain data.

Crime Anticipation System (CAS) is a similar system used in Netherlands, and it relies on historical crime data to generate a heatmap to identify short-term crime risks. CAS operates on the principle that crimes are not randomly distributed but are concentrated in certain geographic and temporal clusters. The system integrates geographic clustering methods with temporal crime trends, recognizing that recent crimes often predict future crimes in nearby areas within a short time window. Same as PredPol, CAS will also collect basic crime data including type, location and time. Besides, CAS also process geospatial data like neighborhood layouts and temporal data like holidays and festivals.

## Review of the Modelling Approaches
PredPol uses a straightforward statistical approach assuming that crime event follows a statistical distribution with two parameters: space and time. It believes past crimes are indicative of future crimes trends in nearby areas.

## Feature Engineering Techniques

## Evaluation Metrics and Their Appropriateness

## Definition&Comparison between 4 main topics
1. 🌍 Predicting Places of Increased Crime Risk
2. 👤 Predicting Potential Offenders
3. 👥 Predicting Group/Population Crime Patterns
4. ⚠ Predicting Potential Victims

| Topic | Unit of Analysis | Key Driver | Ethical Risk |
| --- | --- | --- | --- |
| 1. Places | Geography/Time | Environmental factors | Over-policing marginalized areas |
| 2. Offenders | Individuals | Behavioral/demographic traits | Racial/profiling bias |
| 3. Group Patterns | Networks/Populations | Social/economic interactions | Stereotyping demographic groups |
| 4. Victims | Individuals/Groups | Vulnerability/exposure | Privacy violations |
### Column-to-Topic Mapping & Possible Feature Engineering 

| Column                            | Relevant Topics          | Feature Engineering Priorities                                               | Key Concerns/Limitations                              |
|-----------------------------------|--------------------------|------------------------------------------------------------------------------|------------------------------------------------------|
| **Date**                           | 1 (Places), 3 (Group Patterns) | Temporal features: `hour`, `day_of_week`, `month`, `holiday_flag`, `time_since_last_crime` | Seasonality effects (e.g., summer crime spikes).      |
| **Block**                          | 1 (Places)                | Geospatial clustering (e.g., `hotspot_flag`), `distance_to_landmarks` (e.g., bars, transit hubs) | Partial redaction limits address precision.          |
| **IUCR**                           | 1 (Places), 3 (Group Patterns) | Encode crime type hierarchies (e.g., `violent_flag`, `property_flag`).       | IUCR codes may change over time.                     |
| **Primary Type**                   | 1 (Places), 3 (Group Patterns), 4 (Victims) | One-hot encoding for crime types (e.g., `THEFT`, `ASSAULT`).                  | Broad categories may obscure nuances (e.g., "theft" includes shoplifting and carjacking). |
| **Location Description**           | 1 (Places), 4 (Victims)   | Categorical encoding (e.g., `residence`, `street`, `park`), `is_high_risk_location` flag. | Missing or vague entries (e.g., "other").            |
| **Arrest**                         | 2 (Offenders)             | Temporal lag features (e.g., `prior_arrests_in_block`, `arrest_rate_per_district`). | Arrests ≠ crimes (biased policing may inflate counts). |
| **Domestic**                       | 4 (Victims)               | Flag for `repeat_domestic_incidents` (if address/block is repeated).          | Underreporting of domestic incidents.                |
| **Beat/District/Ward/Community Area** | 1 (Places), 3 (Group Patterns) | Aggregate crime counts by area (e.g., `crimes_per_1000_residents` using census data). | Community areas may proxy for race/income (ethical bias risk). |
| **Latitude/Longitude**              | 1 (Places)                | Spatial grids (e.g., hexbin aggregates), `distance_to_police_stations`.        | Coordinate accuracy varies (e.g., redacted blocks).  |
| **Year**                            | 3 (Group Patterns)        | Long-term trends (e.g., `crimes_year_over_year`).                             | Limited utility if using finer-grained `Date`.       |