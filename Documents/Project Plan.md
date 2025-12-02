# Background and Research Question

Violent crimes remain a public safety concern in the United States. As
defined by the FBI violent crimes involve used of force including but
not limited to murder, rape, robbery, and aggravated assault [@FBI2019].
Understanding where and when these crimes occur is critical for
improving public safety, guiding policy decisions, and informing
data-driven policing strategies. Nationally violent crimes are on the
rise with notable increase in aggravated assaults and homicides
[@FBI2021]. Urban areas experience higher rates of violent crimes than
rural areas. Studying crime trends in Los Angeles provides valuable
insight into the complex factors that influence public safety in one of
the nation's largest and most diverse cities. Los Angeles in particular
has consistently reported above average rates of violent crimes, with
incidents being concentrated in specific neighborhoods and often linked
to specific seasons or time periods [@lee2024].

Prior research indicates that these patterns are unevenly distributed
across the city and persist over time citing recurring hot spots,
seasonal trends, socioeconomic conditions, and neighborhood dynamics
[@lee2024; @rosenfeld2023] as major contributors to crime variations.
Foundational sources such as the City of Los Angeles Crime Data
[@LACrime2020], LAPD reports [@lapd2023], and research by Roses et al.
[@Roses2021], Balocchi and Jensen [@Balocchi2019] and Lee et al.
[@lee2024] provides the empirical and methodological basis for analyzing
how location, time, and context interact to shape crime dynamics in Los
Angeles. Further studies emphasize the importance of spatial and
temporal data in improving predictive accuracy
[@Balocchi2019; @Roses2021].

This study aims to identify the factors that most strongly influence and
predict violent crime across Los Angeles using spatial, temporal and
socioeconomic data.

## Sub-Question 1

Are certain neighborhoods or times of day linked to higher rates of
violent crimes?

Exploring these patterns can reveal areas where preventative measures or
community resources may be most effective. By mapping crimes across
different districts and examining trends by hour, day, or season, we can
identify recurring patterns that can further our understanding of when
and where violent crimes are most likely to occur
[@lee2024; @Roses2021]. This can help guide policing efforts and
resource use to make neighborhoods safer.\
**Hypothesis:** We expect violent crimes to occur more frequently in
neighborhoods with higher poverty levels and during late-night hours or
weekends.

## Sub-Question 2

Which machine learning models most effectively predicts the likelihood
of a violent vs. non-violent crime based on time, location, and other
key features?

By comparing models such as Logistic Regression, Random Forest, and
Naive Bayes, we aim to evaluate how well different algorithms capture
the relationships between these variables [@Roses2021].\
**Hypothesis:** We expect that Random Forest will outperform Logistic
Regression and Naive Bayes in predicting violent versus non-violent
crimes.

## Sub-Question 3

How have violent crime rates in Los Angeles changed over time since
2020?

Analyzing yearly and monthly trends can show how violent crime has
shifted during and after major events such as the COVID-19 pandemic.
Understanding these temporal changes helps identify broader social and
environmental influences on crime [@lapd2023].\
**Hypothesis:** We expect violent crime rates to have decreased in 2020
but gradually increased in the following years as normal activities
resumed.

# Summary of Literature Review

Crime patterns in Los Angeles reflect broader social and economic
inequalities that shape where and when violent incidents occur.
Neighborhoods with higher poverty, unemployment, and limited access to
social resources consistently report higher crime rates, showing that
structural disadvantages play a key role in urban violence
[@FBI2021; @lee2024]. Research by Cung [@cung2013crime] and Rosenfeld
and Austin [@rosenfeld2023] highlights how both socioeconomic status and
racial composition influence not only crime rates but also policing
intensity, with minority and low-income areas often facing greater
enforcement and surveillance. These studies emphasize the need to
examine crime within its social context rather than as an isolated
behavioral issue.

Spatial and temporal research has become essential for revealing hidden
patterns in crime data. Lee et al. [@lee2024] used clustering and
hotspot analysis to locate persistent areas of violent activity and
identify seasonal cycles across Los Angeles. Roses et al. [@Roses2021]
expanded this approach through agent-based simulations, showing how
demographic and environmental factors interact to produce crime
hotspots. Similarly, Balocchi and Jensen [@Balocchi2019] applied
Bayesian hierarchical models to account for spatial and temporal
dependencies, allowing a clearer understanding of whether local crime
surges are isolated or part of broader citywide trends.

Despite these advances, prior research often relies on older datasets or
omits key social variables, limiting the accuracy of predictive models.
Many studies treat crime as purely spatial or temporal without
integrating socioeconomic indicators such as income, education, or
population density [@cung2013crime]. Others face challenges related to
data quality, including missing coordinates or incomplete timestamps,
which can distort hotspot detection. Addressing these limitations
requires current, fine-grained data that captures both the geographic
and social dimensions of violent crime.

This project builds on these findings by combining recent LAPD data
[@LACrime2020] with spatial, temporal, and socioeconomic variables to
identify and predict violent crime patterns across Los Angeles. By
comparing machine learning models such as Random Forest, Naive Bayes,
and Logistic Regression, the study evaluates which approach most
effectively predicts violent versus non-violent incidents. The ultimate
goal is to produce evidence-based insights that can help policymakers
allocate resources more equitably and strengthen public safety
throughout Los Angeles.

# Datasets

Our primary dataset, [City of Los Angeles Crime Data from 2020 to
Present)](https://data.lacity.org/Public-Safety/Crime-Data-from-2020-to-Present/2nrs-mtv8)
[@LACrime2020], can be downloaded in multiple formats, including CSV,
JSON, XML, and RDF. Each file contains identical data, but has different
structures for use in tabular, hierarchical, or link-data analyses. We
will be importing the CSV file into Python for the purposes of this
project. The dataset is an open dataset published on the City of Los
Angeles Open Data Portal and can be accessed through
[data.gov](https://www.data.gov). It contains detailed records of
reported crimes from January 2020 to the present. The City of Los
Angeles updates this data every day, so the information is always
expanding. There are 28 columns and approximately 1,004,991 rows as of
November 3, 2025.

The columns include temporal data, spatial data, crime classification,
victim demographics, location, and situational data. We will be able to
use crime type, date, time, coordinates, weapon used, and victim
demographics in our analyses. We will use the spatial fields for mapping
and geospatial analysis or crime patterns. The dataset supports trend
analysis over time, comparisons across neighborhoods, and correlations
with demographic and socioeconomic data. It provides a solid foundation
for analyzing temporal, spatial, and categorical dimensions of crime in
Los Angeles due to its comprehensive structure and updated data.

Some records contain missing or inconsistent values. Data cleaning will
remove these entries to maintain analytical accuracy. We will
standardize fields such as data and time for temporal analysis.
Categorical data may be encoded or grouped for easier modeling. Outliers
will be identified and addressed to ensure data reliability. Our final,
cleaned dataset will help us analyze the data accurately.

# EDA and Methodology

Our analysis will be formatting in Python programming in a Jupyter
Notebook environment. Tableau may be used for cleaner visualizations.
Pandas, Matplotlib, and Seaborn will be used for data cleaning,
descriptive statistics, and visualizations. Data processing will include
filtering for violent crimes, handling missing or inconsistent values,
and converting temporal fields into usable data and time formats.
Categorical variables such as crime type and area name will be encoded
as needed for analysis.

Initial EDA will focus on identifying temporal trends from 2020 to 2025,
computing descriptive statistics, such as crime counts, rates, and
proportions, and visualizing them using graphs, charts, and heatmaps to
highlight monthly and yearly fluctuations in violent crime. Geospatial
analysis may be performed using latitude and longitude fields to map the
distribution of crimes across Los Angeles. This will help identify
regions with consistent patterns of violent crime. To find changes in
violent crime over time, we will implement time series analysis and
trend visualization techniques.

Primary techniques for analyzing violent crime patterns will include
logistic regression, Random Forest, and Naive Bayes models. Given the
growing use of machine learning in crime prediction, we remain open to
exploring the potential application of more advanced models, such as
neural networks, if they improve predictive performance on our dataset.
