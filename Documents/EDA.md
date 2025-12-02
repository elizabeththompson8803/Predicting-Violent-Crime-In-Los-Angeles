# Data Sources, Preparation, and Cleaning

## Data Source

The primary dataset used in this analysis is the [City of Los Angeles
Crime Data from 2020 to
Present)](https://data.lacity.org/Public-Safety/Crime-Data-from-2020-to-Present/2nrs-mtv8)
[@LACrime2020], accessed through the Los Angeles Open Data Portal and
hosted on data.gov. This dataset contains detailed, incident-level
reports of crimes recorded by the Los Angeles Police Department (LAPD)
since January 1,2020. Each row represents a single reported crime,
identified by a unique report number (DR_NO), and includes information
such as the date and time of occurrence, type of offense, crime code
description, location (latitude and longitude), police reporting area,
and basic victim demographics (age, sec, descent).\
At the time of acquisition, the dataset contained approximately 700,000
observations and over 25 variables. The structure and comprehensiveness
of this dataset made it well suited for temporal and spatial crime
pattern analysis. Its regular updates ensure that the data reflect
current trends, enabling this project to analyze shifts in crime
patterns during and after key events, such as the COVID-19 pandemic.
descent).\
At the time of acquisition, the dataset contained approximately 700,000
observations and over 25 variables. The structure and comprehensiveness
of this dataset made it well suited for temporal and spatial crime
pattern analysis. Its regular updates ensure that the data reflect
current trends, enabling this project to analyze shifts in crime
patterns during and after key events, such as the COVID-19 pandemic.\

![Raw data set](raw data.png){#fig:raw width="50%"}

## Data Preparation and Cleaning

Initial Inspection and Standardization The raw dataset was imported into
Python using Pandas and cleaned in a collaborative Jupyter/Deepnote
environment. During the initial inspection, the dataset was found to
contain inconsistent column naming conventions, placeholder timestamps,
and missing or invalid coordinates, all of which required correction
before analysis.\
Column headers were standardized to uniform, Python-friendly names by
removing extra spaces and applying consistent casing. For example,
variables such as "DATE OCC" and "TIME OCC" were remanded to
"Date_Occurred" and "Time_Occurred". This ensured compatibility across
functions and improved code readability. Duplicate records were
identified and removed based on the unique report number (DR_NO), which
serves as the official identifier for each police report

## Date/Time conversion and Temporal Feature Extraction

Both "Date Rptd" (date reported) and "DATE OCC" (date occurred) were
converted from string format to python datetime objects using the
explicit timestamp format "%m/%d/%Y %I:%M:%S %p". This conversion was
crucial for conducting time series analysis and creating derived
temporal variables. From "Date_Occurred", several new features were
extracted:\
YEAR - the calendar year of occurrence, used for long-term trend
analysis. MONTH - numerical month, used for identifying seasonal
fluctuations. DAY_OF_WEEK - categorical representation (e.g.,
Monday-Sunday), used for weekly trend visualization. HOUR - the hour of
the day, derived from both "Date_Occurred" and "Tim_Occurred" to analyze
intraday crime patterns For observations with numeric "Time_Occurred"
values in HHMM format (e.g., 2315 for 11:15 PM), the hour was extracted
by dividing by 100.\

## Handling Missing and Placeholder Values

One of the most significant challenges in this dataset involved the
placeholder timestamps recorded as 12:00:00 AM or 12:00:00 PM. These
values are commonly used in police databases when the precise time of
the crime is unknown or unreported.\
Initial hourly plots showed extreme spikes in reported crimes at
midnight and noon; indicating that many of these times were artificial
defaults rather than true crime occurrences. To avoid skewing the
time-of-day analysis, records were filtered to exclude only those
entries with exact timestamps of 12:00:00, while retaining all other
records that fell within the 12-hour period. This approach preserved
meaningful data while improving the validity of temporal visualizations.

<figure id="fig:placeholders">
<figure id="fig:placeholders-a">
<img src="Spatial Figures/Cleaning Figures/1.png" />
<figcaption>Entries where Hour = 0</figcaption>
</figure>
<figure id="fig:placeholders-b">
<img src="Spatial Figures/Cleaning Figures/1_2.png" />
<figcaption>Entries where Hour = 12</figcaption>
</figure>
<figure id="fig:placeholders-c">
<img src="Spatial Figures/Cleaning Figures/1_3.png" />
<figcaption>Time_Occurred = 1200 (placeholder)</figcaption>
</figure>
<figcaption>Effect of placeholder times on hourly counts</figcaption>
</figure>

Records missing key geographic data (latitude and longitude) were
dropped, and accurate coordinates were essential for spatial analysis.
However, these removals represented less than 2% of total records and
did not significantly alter data integrity.

## Data Type and Value Cleaning

Several non-numeric columns were converted to categorical or numerical
formats as appropriate. For example, "Victim_Age" was converted to
numeric, and invalid ages (e.g., negative values or over 120) were
removed. Similarly, "Victim_Sex" and "Victim_Descent" were transformed
into categorical variables to allow for later demographic analysis.

## Feature Engineering

To align the dataset with the project's primary research question,
predicting the likelihood that a crime is violent based on its
contextual attributes, a new binary feature, Is_Violent, was engineered.
Using Pattern matching on the "Crime_Code_Desc" field, the code
identified offenses classified as violent under the FBI's Uniform Crime
Reporting (UCR) definition: "Offenses that involve force or threat of
force, including murder and non-negligent manslaughter, rape, robbery,
and aggravated assault."\
Accordingly, Is_Violent was assigned a value of 1 (True) for
descriptions containing keywords such as Homicide, Murder, Rape,
Robbery, Assault, Battery, or Shooting, and 0 (False) for all other
crimes. This variable was later used to separate violent and non-violent
crime patterns in time series, heatmap, and spatial analyses.\
Additional derived variables included: YearMonth - combining year and
month into a single time index for rolling averages and trend
visualization. Rolling averages - computed over 3-month and 6-month
windows to smooth short-term fluctuations and highlight broader temporal
trends. Spatial aggregations - grouping incidents by "Area_Name" to
quantify total and violent crime rates across police divisions.

## Final Dataset Summary

After cleaning and preprocessing, the final dataset contained
approximately 680,000 valid records and 20 core features ready for
exploratory analysis. All temporal, categorical, and spatial variables
were standardized, ensuring consistent data formats across the project
team's Deepnote.\
The resulting dataset provided a reliable foundation for subsequent
analyses of temporal and spatial crime patterns in Los Angeles. By
systematically addressing missing data, placeholder times, and
inconsistent formatting, the cleaned dataset now accurately reflects
true reported incidents and supports deeper statistical and machine
learning exploration.\

# Exploratory Data Analysis

## Temporal Features

Following the initial cleaning of the dataset, our team aimed to explore
general temporal trends in reported crimes across different hours of the
day. To visualize these patterns, we first generated a heatmap showing
the frequency of incidents by hour for all available years.\
After cleaning and removing the placeholder 1200 time values, we created
an hourly heatmap to visualize the distribution of reported crimes
across different hours of the day for each year in the dataset
([6](#fig:hourly_heatmap){reference-type="ref+Label"
reference="fig:hourly_heatmap"}. The goal of this visualization was to
identify consistent temporal trends and potential outliers in hourly
reporting patterns over time.

![Hourly crime frequency by
year](Temporal Figures/2.png){#fig:hourly_heatmap width="90%"}

The heatmap displays hours of the day along the x-axis (0--23) and years
along the y-axis, with color intensity representing the frequency of
reported crimes. Darker shades correspond to higher crime frequencies.
From this visualization, several clear patterns emerge. Across all
years, reported crimes tend to peak during the late afternoon and
evening hours (around 16:00--20:00), indicating higher activity levels
during those periods. In contrast, early morning hours (particularly
between 03:00 and 07:00) consistently show lower frequencies, suggesting
reduced criminal activity or reporting during those times.\
Additionally, the heatmap reveals a general decline in total reported
incidents beginning in 2024, which may be attributed to delayed data
entry or incomplete reporting for more recent periods. Overall, this
visualization provides a high-level overview of daily crime rhythms in
Los Angeles and establishes a strong temporal foundation for subsequent
analyses exploring how crime patterns vary by day of the week, month, or
season.\
After examining hourly crime patterns, we next analyzed the distribution
of incidents by day of the week to identify broader temporal cycles in
criminal activity [7](#fig:dow_heatmap){reference-type="ref+Label"
reference="fig:dow_heatmap"}. The heatmap in
[6](#fig:hourly_heatmap){reference-type="ref+Label"
reference="fig:hourly_heatmap"} visualizes crime frequency across each
day of the week for all recorded years, with color intensity indicating
the relative number of reported crimes.

![Crime frequency by day of week and
year](Temporal Figures/3.png){#fig:dow_heatmap width="90%"}

This visualization reveals a consistent weekly rhythm in reported
incidents. Across nearly all years, Friday and Saturday stand out with
the highest crime frequencies, indicating an increase in criminal
activity during weekends. These peaks may correspond to higher social
activity, nightlife, and larger public gatherings in Los Angeles during
those periods. Conversely, early weekdays (Monday--Wednesday) show
noticeably lower frequencies, suggesting reduced incident volumes at the
start of the week. A gradual overall decrease in total incident counts
appears in 2024, mirroring the same reporting trend observed in the
hourly analysis. Overall, the weekly heatmap emphasizes the cyclical
nature of criminal activity, with late-week and weekend spikes being the
most consistent temporal pattern across the dataset.\
To better understand the temporal distribution of crimes across Los
Angeles, we created two complementary heatmaps (Figures 5a and 5b) that
visualize both the frequency and proportion of violent crimes per month
for each recorded year.\
Figure 5a presents the monthly share of violent crimes, calculated as
the percentage of all reported crimes in a given month that were
classified as violent. In contrast, Figure 5b displays the total number
of crime incidents reported each month, regardless of classification.
Together, these figures provide insight into both seasonal fluctuations
and year-to-year differences in crime dynamics.

<figure id="fig:monthly_crime">
<figure id="fig:monthly_violent_share">
<img src="Temporal Figures/4.png" />
<figcaption>Monthly share of violent crimes</figcaption>
</figure>
<figure id="fig:monthly_frequency">
<img src="Temporal Figures/4_1.png" />
<figcaption>Crime frequency by month and year</figcaption>
</figure>
<figcaption>Monthly crime distribution and proportion of violent
offenses</figcaption>
</figure>

Across all years from 2020 to 2023, violent crimes consistently
comprised roughly 45--50 percent of total incidents, with only small
month-to-month variation. The percentages remain relatively stable
throughout each year, suggesting that the proportion of violent to
non-violent offenses does not change significantly seasonally. However,
a noticeable decline in 2024 indicates potential reporting lags or
incomplete data capture, as total incidents (Figure 5a) drop sharply
after April 2024. Similarly, data for 2025 appear truncated, likely
reflecting only partial-year records.\
Figure 5b further highlights year-over-year variation in total incident
counts, showing elevated frequencies in 2022 and 2023 compared to
earlier years, with monthly totals exceeding 18,000 cases during peak
periods. In contrast, 2024 shows a marked reduction beginning mid-year,
aligning with the decrease in violent crime share in Figure 5a. This
pattern suggests that while overall crime rates may fluctuate annually
due to external factors (such as data completeness or seasonal effects),
the relative proportion of violent crimes remains largely consistent
across time.

<figure id="fig:violent_crime_trends">
<figure>
<img src="Temporal Figures/5.png" />
<figcaption> Monthly violent crime trends with 3-month rolling average
(2020–2025).</figcaption>
</figure>
<p><br />
</p>
<figure>
<img src="Temporal Figures/5_1.png" />
<figcaption>Year-over-year percent change in monthly violent
crimes.</figcaption>
</figure>
<figcaption>Temporal patterns in monthly violent crime frequency and
yearly percent change.</figcaption>
</figure>

To extend the monthly and yearly temporal analysis, Figures 6a and 6b
examine longer-term patterns in violent crime.\
Figure 6a displays the total monthly count of reported violent crimes in
Los Angeles from 2020 to 2025, along with a 3-month rolling average
(red) to smooth short-term fluctuations. The results show stable or
slightly increasing crime levels through 2022 and 2023 before a sharp
decline beginning in early 2024, which likely reflects delayed reporting
or incomplete data for more recent months.\
Figure 6b illustrates the year over year percentage change in monthly
violent crime counts. Positive values represent increases relative to
the same month in the prior year, while negative values indicate
declines. Between 2021 and 2023, fluctuations were relatively mild
generally within +- 20 percent but after 2024, percent change drops
steeply to near -100 percent, again suggesting incomplete data
submission rather than a true collapse in crime activity.\
Together, these two figures provide complementary perspectives on crime
dynamics: Figure 6a emphasizes absolute levels and rolling averages over
time, while Figure 6b highlights proportional change and volatility.
Both reinforce earlier findings that temporal trends in recent years are
likely influenced by partial data for 2024 and 2025 rather than a
fundamental shift in underlying patterns.

## Spatial Features

The spatial analysis explores how crime incidents are distributed across
Los Angeles and how these patterns vary by police division and over
time. By mapping total crime counts, identifying geographic hotspots,
and comparing divisions, this section highlights spatial disparities in
crime concentration. These visualizations provide a foundation for
understanding which areas experience consistently higher crime rates and
how these trends evolve year to year.

![Top 10 LAPD divisions by total reported
crimes](Spatial Figures/6.png){#fig:top10_divisions width="75%"}

[12](#fig:top10_divisions){reference-type="ref+Label"
reference="fig:top10_divisions"} and
[13](#fig:crime_type_heatmap){reference-type="ref+Label"
reference="fig:crime_type_heatmap"} explore how crime distribution
varies across LAPD divisions and by crime type. Figure 7 identifies the
top ten areas with the highest total reported crimes, while Figure 8
further examines the composition of these crimes across divisions have
higher frequencies of specific offenses. Together, these figures
highlight both the concentration and diversity of criminal activity
across Los Angeles.

![Crime-type frequency across LAPD
divisions](Spatial Figures/7.png){#fig:crime_type_heatmap width="70%"}

The series of yearly choropleth maps
([14](#fig:choropleth_all){reference-type="ref+Label"
reference="fig:choropleth_all"} and 10) illustrates how crime
distribution across Los Angeles has evolved from 2020 to 2025. While the
overall spatial pattern remains relatively consistent, certain divisions
show notable fluctuations in intensity, reflecting localized increases
or decreases in reported crime activity.\
Throughout the observed period, Central, 77th Street, Southwest, and
Newton divisions consistently appear as persistent hotspots, indicating
enduring concentrations of crime in these regions. These areas are
characterized by high population density, mixed residential-commercial
zoning, and greater socioeconomic challenges, which may contribute to
sustained high incident counts.

![Total reported crimes by division (all
years)](Spatial Figures/8.png){#fig:choropleth_all width="30%"}

<figure id="fig:yearly_maps">
<figure>
<img src="Spatial Figures/crime_map_2020.png" />
<figcaption>2020</figcaption>
</figure>
<figure>
<img src="Spatial Figures/crime_map_2021.png" />
<figcaption>2021</figcaption>
</figure>
<figure>
<img src="Spatial Figures/crime_map_2022.png" />
<figcaption>2022</figcaption>
</figure>
<figure>
<img src="Spatial Figures/crime_map_2023.png" />
<figcaption>2023</figcaption>
</figure>
<figure>
<img src="Spatial Figures/crime_map_2024.png" />
<figcaption>2024</figcaption>
</figure>
<figure>
<img src="Spatial Figures/crime_map_2025.png" />
<figcaption>2025</figcaption>
</figure>
<figcaption>Yearly distribution of reported crimes by
division</figcaption>
</figure>

Between 2020 and 2022, crime intensity expands slightly across the
central and southern regions of Los Angeles, suggesting a gradual
increase in overall reported incidents. The peak in 2022 corresponds
with visible darkening across multiple adjacent divisions, implying
elevated activity citywide. By contrast, 2023 to 2024 shows a mild
reduction in intensity.\
In 2025, overall intensity is noticeably lower, though this may be
partially attributed to incomplete reporting for the most recent year.
Importantly, even as the total number of incidents fluctuates, the
geographic distribution of hotspots remains stable, reaffirming that the
same divisions experience disproportionate levels of crime year after
year.\
The cumulative map ([14](#fig:choropleth_all){reference-type="ref+Label"
reference="fig:choropleth_all"}) reinforces these findings, showing the
highest long-term concentrations centered in Central and South Los
Angeles.\

<figure id="fig:violent_nonviolent">
<figure id="fig:vnv-nonviolent">
<img src="Spatial Figures/9.png" />
<figcaption>Non-violent</figcaption>
</figure>
<figure id="fig:vnv-violent">
<img src="Spatial Figures/9_1.png" />
<figcaption>Violent</figcaption>
</figure>
<figcaption>Locations of non-violent vs violent crimes</figcaption>
</figure>

At first glance, the spatial distributions of violent and non-violent
crimes appear highly similar, with both occurring in overlapping
geographic areas across Los Angeles. The plots show that incidents of
both types are densely clustered within the central, southern, and
eastern regions of the city, particularly around divisions such as
Central, 77th Street, Newton, and Southwest.\
This pattern suggests that spatial location alone does not distinctly
separate violent from non-violent crimes. In other words, both
categories tend to occur in the same general hotspots, indicating that
shared environmental and social conditions, such as high population
density, economic disadvantage, or commercial activity, may drive
overall crime frequency regardless of severity.\
While differences in underlying causes or crime types likely exist, they
are not easily observable through spatial distribution alone. Both
violent and non-violent crimes appear to follow the same urban
concentration pattern, highlighting that geography itself is not the
primary differentiator between them. To further distinguish between
these crime categories, additional contextual variables (e.g.,
socioeconomic indicators, time of day, or police response patterns)
would be needed beyond the spatial dimension.

## Demographic Features

This section examines the demographic characteristics of crime victims
in Los Angeles to provide insight into who is most affected across
different identity groups. By analyzing variables such as age, gender,
and ethnicity, this portion of the exploratory data analysis highlights
patterns and disparities within victim demographics. Visualizations of
these features help identify which populations are most represented in
reported crimes, reveal the age distribution of victims, and show how
gender and descent interact across the dataset. Understanding these
demographic trends provides essential context for interpreting broader
spatial and temporal crime patterns in the following sections.

![Victim gender
distribution](Demographic Figures/gender.png){#fig:gender_dist
width="70%"}

The gender distribution reveals that male and female victims make up the
majority of reported crime incidents, with males slightly more
represented overall ([19](#fig:gender_dist){reference-type="ref+Label"
reference="fig:gender_dist"}). This suggests that victimization affects
both genders at comparable rates, though males may experience a
marginally higher frequency of reported cases. A smaller portion of
records is categorized as "Unknown," which likely reflects incomplete or
unreported data rather than a true demographic pattern. Overall, the
data indicate that gender is a relevant but not sharply divided factor
in victimization trends across Los Angeles.

![Distribution of victim
ages](Demographic Figures/dist_of_ages.png){#fig:age_dist width="70%"}

The age distribution of victims is right-skewed, with most incidents
involving younger individuals
([20](#fig:age_dist){reference-type="ref+Label"
reference="fig:age_dist"}).. The mean victim age is approximately 28.8
years, indicating that victims tend to be in their late twenties on
average. A noticeable concentration appears among children under 10 and
young adults between 18 and 40, after which the frequency of
victimization declines steadily with age. The tapering tail toward older
age groups suggests that elderly individuals are less frequently
represented in reported crimes. This distribution highlights that crime
in Los Angeles disproportionately affects younger populations,
particularly those in early adulthood.

![Victim descent
distribution](Demographic Figures/victim descent.png){#fig:descent_dist
width="70%"}

The descent distribution shows that Hispanic/Latin/Mexican and White
individuals make up the largest proportion of recorded victims
([21](#fig:descent_dist){reference-type="ref+Label"
reference="fig:descent_dist"}), together accounting for a majority of
reported incidents. Black victims represent the next most frequent
group, followed by smaller counts among Asian, Pacific Islander, and
American Indian/Alaskan Native individuals. The presence of a notable
"Unknown" category likely reflects incomplete reporting rather than
demographic absence. These results broadly align with Los Angeles's
diverse population makeup but may also highlight disparities in
reporting rates, exposure, and neighborhood-level crime distribution.

![Percentage of victim gender by age
group](Demographic Figures/percent vict gen age.png){#fig:gender_by_age
width="60%"}

The results show a balanced representation between male and female
victims in most adult categories
([22](#fig:gender_by_age){reference-type="ref+Label"
reference="fig:gender_by_age"}), particularly from ages 18 to 60, where
the percentages are nearly even. However, among juvenile victims (ages
0--17), a disproportionately high share falls under the "Unknown"
category, suggesting incomplete or inconsistent gender reporting for
minors in police data. For older age groups (41--60 and 60+), male
victims become slightly more dominant, which may reflect differences in
crime exposure, lifestyle patterns, or reporting rates among older
adults. Overall, this visualization highlights that gender
representation is generally consistent across age, with the main
exception being underreported or unclassified gender data among younger
victims.

![Percentage of victim gender by
ethnicity](Demographic Figures/percent vict gender eth.png){#fig:gender_by_ethnicity
width="60%"}

This heatmap compares the proportional distribution of male, female, and
unknown gender victims across different ethnic groups
([23](#fig:gender_by_ethnicity){reference-type="ref+Label"
reference="fig:gender_by_ethnicity"}) to highlight demographic
differences in victimization patterns. Overall, most ethnicities display
a relatively balanced gender representation, with only minor variations
between groups. White and American Indian/Native Alaskan victims show a
higher proportion of males, while Black victims have a greater share of
females. Hispanic and Pacific Islander victims appear evenly distributed
between genders, suggesting minimal disparity. The "Other" and "Unknown"
categories contain unusually high proportions of unclassified gender
data, likely reflecting incomplete or inconsistent reporting. These
results suggest that while gender representation remains generally
consistent across ethnic groups, data completeness varies significantly
by category.

![Victim count by ethnicity and
gender](Demographic Figures/vict count eth and gen.png){#fig:count_by_eth_gender
width="70%"}

This heatmap displays the total number of victims by both ethnicity and
gender ([24](#fig:count_by_eth_gender){reference-type="ref+Label"
reference="fig:count_by_eth_gender"}), highlighting which demographic
groups experience the highest number of reported incidents. The largest
counts are observed among Hispanic and White victims, followed by Black
individuals, which aligns with the broader population makeup of Los
Angeles. Smaller counts appear among Asian, Pacific Islander, and
American Indian/Native Alaskan victims, reflecting their smaller
population representation. Male and female counts are generally similar
across most ethnicities, suggesting a balanced distribution of
victimization between genders. However, the substantial values within
the "Other" and "Unknown" categories indicate data reporting gaps or
inconsistencies in classification, which should be considered when
interpreting demographic trends.
