# Introduction

Studying crime trends in Los Angeles provides valuable insight into the
complex factors that influence public safety in one of the largest and
most diverse cities in the country. According to the Federal Bureau of
Investigation (2019) [@FBI2019], violent crime includes murder, rape,
robbery, and aggravated assault; offenses that pose the greatest threat
to community well-being. Understanding where and when these crimes occur
is critical for improving public safety, guiding policy decisions, and
informing data-driven policing strategies. In recent years, national
increases in homicides and aggravated assaults have underscored the
urgency of developing predictive and preventive approaches to crime
reduction [@FBI2021]. This study aims to explore patterns in Los Angeles
crime using spatial, temporal, and demographic data to better understand
the factors associated with violent versus nonviolent incidents.
Foundational sources such as the City of Los Angeles Crime Data (2025)
[@LACrime2020], LAPD reports (2024) [@lapd2023], and research by Rosés
et al. (2021) [@Roses2021], Balocchi and Jensen (2019) [@Balocchi2019],
and Lee et al. (2024) [@lee2024] provide the empirical and
methodological basis for analyzing how location, time, and context
interact to shape crime dynamics in Los Angeles.

# Background

Crime in the United States has always reflected broader social and
economic conditions. In the last few decades, national crime rates have
fluctuated in response to economic and demographic trends and policy
changes. The national crime rate peaked during the 1980s and early
1990s. Since then, violent crime has declined in many areas of the
country. However, major cities continue to face challenges tied to
national issues, including inequality, housing instability, and limited
access to social resources. Economic recessions and policy reforms have
also played substantial roles in shaping when and where crimes occur
[@FBI2019; @FBI2021].

Policing in the United States adapted as social and economic conditions
changed. In the late nineteenth and early twentieth centuries, cities
established official police departments in response to growing
populations. Organizations that had previously been informal "watch
groups" evolved into formal agencies focused on maintaining order in
increasingly complex environments [@potter2013history]. Over time,
reforms introduced new expectations around accountability and training,
particularly in response to corruption and social unrest. These
institutional changes often reflected broader social transformations,
illustrating how shifts in public trust and civic engagement directly
influenced policing practices [@cung2013crime; @potter2013history].
These changes laid the foundation for the modern relationship between
law enforcement and the public, a relationship that continues to be
redefined by both policy and community pressure.

Los Angeles' policing history is intertwined with its rapid growth and
deep social divisions. From early efforts to establish consistent
patrols to later controversies over civil rights and police misconduct,
the Los Angeles Police Department (LAPD) has contributed to national
debates about justice and authority [@cung2013crime]. The LAPD's
evolution has included periods of reform aimed at restoring public
confidence, particularly after high-profile incidents that exposed
systemic problems within the department. Scholars note that the LAPD's
trajectory also reflects national tensions around race, class, and
power, as policing often mirrored the city's segregated geography and
unequal access to resources [@cung2013crime; @potter2013history].

The civil rights era caused greater scrutiny within the LAPD. Later, the
"War on Drugs", a federal campaign beginning in the 1970s aimed at
reducing illegal drug use and trafficking, reshaped enforcement
priorities, emphasizing control and deterrence but also deepening
mistrust between officers and marginalized communities. This shift,
often linked to federal initiatives beginning in the 1970s and 1980s,
disproportionately impacted low-income and minority neighborhoods,
reinforcing cycles of incarceration and surveillance
[@FBI2019; @potter2013history]. By the 1990s and early 2000s, Los
Angeles began experimenting with community policing strategies meant to
rebuild trust and encourage collaboration with residents [@lapd2023].
These efforts have yielded mixed results, but they highlight a growing
recognition that public safety depends as much on relationships and
transparency as on enforcement [@lee2024].

Today, conversations about policing, across the country and in local
communities, focus on finding a balance between keeping people safe and
holding police accountable. Los Angeles shows how changes in population,
social movements, and history come together to influence how crime is
viewed. The city's experience illustrates that policing in the United
States is constantly adapting to societal pressures. Recent projections
indicate that this evolution will continue as data-driven approaches and
decarceration policies influence how cities manage crime and public
safety in the coming years [@LACrime2020; @rosenfeld2023].

According to crime reports on Los Angeles, neighborhood conditions and
socioeconomic factors influence where crime occurs and who is affected.
Areas with higher poverty, unemployment, and limited access to social
services often have elevated crime rates, indicating structural
inequalities in the city [@cung2013crime]. Cung (2013) found that
low-income neighborhoods consistently had higher reported crime rates
than wealthier areas, indicating that crime is not evenly distributed
across Los Angeles. These patterns suggest that understanding crime
requires considering the social and economic context of different
communities.

Neighborhood racial composition also affects crime patterns and policing
practices. Rosenfeld and Austin (2023) [@rosenfeld2023] examined how the
racial makeup of neighborhoods influences crime reporting and law
enforcement. They found that minority communities often experience
higher levels of policing, even when crime rates are similar to those in
predominantly White or higher-income neighborhoods. This suggests that
enforcement practices are shaped by more than just crime rates and that
systemic factors influence how police interact with different
communities. Overall, research indicates that both socioeconomic status
and racial composition are significant factors in where and how crime
occurs and is addressed by law enforcement
[@cung2013crime; @rosenfeld2023]. Neighborhoods with concentrated
poverty and large minority populations may face both higher crime risk
and more intensive policing, while wealthier areas experience fewer
interventions. Understanding these patterns is important for designing
policies and policing strategies that consider the social context of
crime. Data-driven approaches, such as spatial models and agent-based
simulations, are increasingly used to examine these dynamics and
identify patterns in urban crime [@lee2024].

Mapping and clustering techniques have become indispensable for
revealing the fine-grained spatial and temporal structures of urban
crime that simple aggregate tables conceal. Geospatial visualizations
(choropleth maps, kernel density surfaces) show where indecent density
concentrations occur, while clustering and hotspot methods (e.g.,
Mean-Shift, spectral clustering, scan statistics, kernel density
estimation, and space-time cubes) formally identify persistent and
emerging clusters. Recent applications to Los Angeles illustrate the
value of these tools. Lee et al. (2024) [@lee2024] used Mean-Shift and
spectral clustering on hundreds of thousands of LAPD incidents to locate
stable violent and property crime hotspots and to expose seasonal cycles
that aggregate yearly rates miss. Rosés et al. (2021) [@Roses2021]
demonstrated how agent-based simulations, informed by demographic and
environmental inputs, can reproduce observed spatial hotspots and
temporal fluctuations and thus serve as testbeds for interventions.
Complementary approaches such as Bayesian hierarchical spatial-temporal
models [@Balocchi2019] add statistical rigor by explicitly modeling
spatial autocorrelation and temporal trends, enabling inference about
whether local increases are part of broader regional patterns or truly
local phenomena. Together, mapping and clustering methods provide both
the exploratory maps that guide intuition and the formal models that
quantify spatial-temporal dependence, making them central to any project
that aims to predict where and when violent incidents are most likely to
occur.

Despite their strengths, prior spatial-temporal studies of urban crime
exhibit recurring limitations that motivate our capstone focus. Many
analyses rely on historical or truncated timeframes, which can miss
recent shifts in crime dynamics driven by policy, economic change, or
social events; this is important because up-to-date data can change
hotspot locations and model performance. Several studies also stop short
of fully integrating demographic and socioeconomic covariates; for
example, treating hotspots as purely spatial phenomena without examining
how poverty, population density, or land use explain clustering, thereby
limiting understanding of root causes and the equity implications of
predictive models [@cung2013crime]. Data quality issues are another
common constraint: geolocation errors or missing coordinates, incomplete
or imprecise timestamps, and underreporting or bias in police records
(e.g., differential reporting across neighborhoods) can distort both
maps and model estimates. Finally, methodological choices about spatial
aggregation (county vs. neighborhood vs. block) and temporal resolution
(daily vs. monthly) introduce scale-dependent artifacts (the Modifiable
Areal Unit Problem) and can mask short-term dynamics that are critical
for prediction. These data directly inform our research focus: by using
very recent, fine-grained LAPD [@LACrime2020] incident data and
explicitly incorporating demographic/contextual variables (while
carefully preprocessing to address geocoding and timestamp gaps), we aim
to produce spatial-temporal predictive models that are both current and
better grounded in the social contexts that drive violent crime.

# Methodologies

Research methods for studying violent crime have changed significantly
over the past several decades. Early studies mostly used descriptive
statistics and regression models to look for links between demographic
factors and crime rates [@Hipp2020]. These methods summarized patterns
in the data but did not account for the spatial or temporal aspects of
criminal activity, thereby limiting their usefulness for understanding
cities. The development of spatial criminology enabled researchers to
examine how neighborhood layouts and the relationships between places
shape the distribution and timing of crime. As technology improved and
more geographic data became available, researchers could analyze crime
at finer scales, such as block groups or street segments [@Hipp2020].
This enabled the study of how crime patterns vary across neighborhoods
and over time. More recently, researchers have used machine learning
models to analyze large datasets and uncover patterns that traditional
regression might miss [@Hipp2020].

As methods developed, researchers began to study both where crime occurs
and how these patterns change over time. For example, Balocchi and
Jensen (2019) [@Balocchi2019] used Bayesian hierarchical spatial
modeling to analyze crime trends in Philadelphia. They found that crime
rates did not decline evenly across the city: some neighborhoods showed
steady declines, while others remained hotspots. Their work shows that
crime trends can differ from one neighborhood to another, even within
the same city, and that both location and time matter in crime analysis.

Cheng et al. (2022) [@Cheng2022] studied crime in Liangshan Prefecture,
China, using over 11,000 criminal judgment records. They used several
spatial analysis tools to look at how crime clusters and shifts over
time. They found that violent crimes happened most often at night and in
winter, while property crimes were more common in autumn and in urban
areas like Xichang City. This shows that these models can help identify
when and where crime is most likely to happen, and how season and
location affect crime rates.

Other researchers have begun using several machine learning models and
socioeconomic data to study crime. These methods can handle large
datasets and often detect patterns that older methods might miss
[@Hipp2020]. For instance, Kshatri et al. (2022) used Naive Bayes,
Random Forests, and meta-classifiers to predict and classify crime
trends using data from the National Crime Records Bureau of India. Their
combined model, called an ensemble, achieved an accuracy of 96.6 %,
demonstrating that combining different types of algorithms can improve
predictive performance.

Rosés et al. (2021) [@Roses2021] developed a simulation model to examine
how offenders move and make decisions within a city. They used three
main theories from environmental criminology, Routine Activity, Crime
Pattern, and Rational Choice, to guide the actions of virtual offender
agents in a simulated version of New York City. The model relied on real
mobility data to predict where and when crimes might occur, based on
factors such as how easy it is to reach a location and how many
potential targets are present. By combining theory with real data, their
model matched actual robbery patterns.

One limitation of previous research, including the studies by Kshatri et
al. (2022) [@Sapna2022] and Rosés et al. (2021) [@Roses2021], is that
they often relied on secondary data sources that did not precisely match
the crime data. This sometimes reduced the accuracy of their models.
Cheng et al. (2022)[@Cheng2022] also noted that spatiotemporal models
can be sensitive to the quality and scale of geographic data, so even
minor errors in location or time can yield misleading results.

# Conclusion

Studies show that violent crime in Los Angeles follows specific social
and geographic patterns
[@cung2013crime; @lapd2023; @lee2024; @rosenfeld2023]. For example,
neighborhoods with more poverty or less stable housing tend to have
higher crime rates. Time also plays a role, as some periods of the day
or year see more incidents than others
[@Balocchi2019; @Cheng2022; @cung2013crime]. Earlier research using
regression or hotspot analysis identified these trends, but some studies
used outdated or incomplete data, limiting how well the results apply
today. Many also did not include enough demographic or socioeconomic
details to explain why crime is concentrated in specific areas.

This project addresses those gaps by using recent LAPD data from 2020 to
2025 [@LACrime2020], combined with information about locations, time
periods, and local socioeconomic factors. We compare several machine
learning models, including random forest, naive bayes, and logistic
regression, to determine which best predict violent and non-violent
crime patterns across the city. Our goal is to provide insights that can
guide fairer, more effective public safety policies across Los Angeles.
Right now, enforcement and prevention efforts are often concentrated in
wealthier neighborhoods, while lower-income areas, where community
resources are most needed, receive less consistent attention. By
highlighting where and when violent crimes occur and connecting those
patterns to underlying social and economic conditions, our study aims to
give policymakers more substantial evidence to distribute safety
resources more equitably.
