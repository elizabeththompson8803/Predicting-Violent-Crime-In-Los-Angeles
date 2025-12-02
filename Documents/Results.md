# Results and Implications

## Analysis and Statistical Inference

Violent crimes make up around 45-50% of total incidents each year. As
shown in [1](#fig:violent_crimes){reference-type="ref+label"
reference="fig:violent_crimes"}, both the monthly trend and the yearly
proportions move very little over time, indicating that temporal
features such as month or year likely add limited predictive value.
Across months, the proportion of violent incidents rarely shifts more
than a few percentage points, suggesting that seasonal patterns are
extremely weak. The sharp drop in 2024-2025 aligns with incomplete
reporting rather than a real shift in crime patterns, so those months
must be handled cautiously to avoid bias. Overall, the stability of
violent crime proportions implies that features such as location and
demographic may be more informative for predicting crime severity.

![Monthly trend and yearly proportion of violent
crimes.](Results Figures/violent_crimes_trends.png){#fig:violent_crimes
width="80%"}

Spatial and demographic patterns reveal clearer differences than
temporal features. [2](#fig:combined_spatial){reference-type="ref+label"
reference="fig:combined_spatial"} shows two complementary views: the
heatmap on the left displays monthly crime frequency by year,
highlighting consistent seasonal fluctuations and spikes that may
correspond to reporting patterns or broader crime trends. The bar chart
on the right illustrates total reported crimes across LAPD divisions,
showing that a small set of divisions (Central, Southwest, and 77th
Street) consistently report far more incidents than others. Together,
these figures indicate that spatial and temporal aggregation patterns,
along with division-specific effects, carry strong signals for modeling
violent crime, while gaps in reporting and persistent location-based
disparities remain important considerations.

![Monthly crime frequency by year (left) and total reported crimes by
LAPD division
(right).](Results Figures/figure2_combined (1).png){#fig:combined_spatial
width="80%"}

## Logistic Regression

Understanding the factors that distinguish violent from non-violent
crime is a central question in criminology and an essential step in
public safety analysis. In our analysis, logistic regression was used as
a foundational modeling approach to assess how well demographic,
contextual, and spatial-temporal characteristics predict violent
incidents in Los Angeles. We used logistic regression as the baseline
because it has long been used in violence prediction and risk assessment
research [@liu2011]. Consistent with prior work,
[@liu2011; @oh2022; @quick2018] our baseline model tests whether victim
characteristics, such as descent, sex, and age, are meaningful
predictors of violence. Research has repeatedly shown that demographic
and situational factors relate to differences in victimization risk and
crime severity across communities [@corcoran2019]. These structural and
demographic influences often shape where violence occurs and which
individuals are most vulnerable. Making them important features to
include when modeling.\
However, criminological theories and spatial research also emphasize
that crime is not evenly distributed. Violence consistently clusters in
particular neighborhoods and varies alongside structural characteristics
[@quick2018]. Due to these patterns, demographic variables alone may not
fully explain why violent incidents occur in certain places and not
others. Environmental criminology further suggests that the features of
the surrounding environment, as well as the routine activity patterns,
influence the likelihood that offenders and targets converge [@lee2024].
That means that where and when crime occurs can be as important as who
is involved. For this reason, our second logistic regression model
incorporates spatial and temporal features, including coordinates,
police reporting areas, and detailed timing variables. Prior research
shows that adding structural and environmental predictors often improves
crime forecasting compared to models that rely solely on demographic
information [@oh2022]. Because violent crime displays strong spatial
clustering and seasonal variation, incorporating location and time
allows the model to capture contextual dynamics that demographic
variables may miss.\
By comparing the baseline and expanded logistic regression models, our
analysis evaluates how much predictive value is gained by incorporating
location-based information. This comparison also establishes a necessary
reference point for the later nonlinear modeling approach, where we
examine whether algorithms such as Random Forests further improve
predictive accuracy. Together, these models provide a comprehensive
assessment of how demographic, situational, and spatial--temporal
factors contribute to distinguishing violent from non-violent crime in
Los Angeles.

![Top 10 Feature Importances for Logistic Regression
Model](Results Figures/logreg_feature_importance){#fig:logreg_feature_importance
width="80%"}

The feature importance results in
Figure [3](#fig:logreg_feature_importance){reference-type="ref"
reference="fig:logreg_feature_importance"} further illustrate why the
spatial--temporal logistic regression model performs better than the
baseline model. The strongest predictors of violence include victim sex,
victim descent, and spatial coordinates, highlighting that both
demographic and geographic context meaningfully shape the probability
that a reported incident is violent. For example,
Victim_Sex_Non-binary/Unknown'' has the highest absolute coefficient,
indicating that cases with ambiguous or missing sex designations are
substantially less likely to be violent relative to the model's
reference category. Similarly, several victim descent
categories,including Hispanic, Black, and Other Asian, are among the
most influential predictors, showing that patterns of victimization risk
differ across communities in ways consistent with prior criminological
research. Importantly, spatial coordinates (latitude and longitude) also
emerge as highly influential, confirming that where an incident occurs
carries as much predictive weight as who is involved. The high
importance of these spatial variables aligns with well-documented
clustering of violent crime in particular corridors of Los Angeles.
Finally, the inclusion of temporal structure, such as
Occurred_Year_2024,'' suggests that recent shifts in crime patterns also
help the model differentiate between violent and non-violent cases.
Together, these feature importances explain why the spatial--temporal
model achieves better balance in the confusion matrices: by
incorporating geographic and contextual information, the enhanced model
captures structural patterns that the demographic-only baseline model
cannot.

<figure id="fig:logreg_confmats">
<figure id="fig:logreg_cm_baseline">
<img src="Results Figures/Confusion_Matrix_LOGREG_baseline.png" />
<figcaption>Baseline Logistic Regression</figcaption>
</figure>
<figure id="fig:logreg_cm_st">
<img src="Results Figures/Confusion_Matrix_LOGREG_spatiotemp.png" />
<figcaption>Logistic Regression with Spatial &amp; Temporal
Features</figcaption>
</figure>
<figcaption>Confusion matrices for logistic regression models. (a)
Baseline model. (b) Spatial and temporal enhanced model.</figcaption>
</figure>

The confusion matrices in
[4](#fig:logreg_cm_baseline){reference-type="ref"
reference="fig:logreg_cm_baseline"} and
[5](#fig:logreg_cm_st){reference-type="ref"
reference="fig:logreg_cm_st"} demonstrate how incorporating spatial and
temporal features affects the logistic regression model's ability to
distinguish violent from non-violent crimes. In the baseline model, the
classifier correctly identifies 105,344 violent crimes (true positives)
but misclassifies 76,809 non-violent incidents as violent, reflecting
its strong recall for violence but a substantial rate of false
positives. When spatial and temporal predictors are added, the model
exhibits a similar pattern: it maintains strong detection of violent
crimes (105,241 true positives) while slightly reducing false positives
among non-violent crimes (75,102). Although the absolute changes are
modest, the spatial--temporal model shows a consistent improvement in
recall for non-violent cases and yields a more balanced trade-off
between the two classes. These results indicate that contextual factors,
such as location within Los Angeles, date, and hour, carry meaningful
predictive value. Incorporating these structural dynamics helps the
model better differentiate where and when violent incidents are more
likely to occur, ultimately improving predictive accuracy and setting a
stronger foundation for more advanced machine learning models later in
the analysis.

## Random Forest Classifier

To predict whether a crime is violent or nonviolent, we used a
comprehensive dataset from the [City of Los Angeles Open Data
portal](https://catalog.data.gov/dataset/crime-data-from-2020-to-present),
which contains detailed records of crimes reported to the LAPD.
[@LACrime2020] The dataset includes over 950,000 incidents and covers
features such as victim demographics (age, sex, and descent), crime type
and description, weapon used, location (address, district, and
geocoordinates), and temporal information (date, day of the week, and
hour of occurrence). After cleaning, preprocessing, and handling missing
values, we retained features most relevant for modeling violent crime
while minimizing data sparsity.

Our goal is not to inform policing actions, but to understand patterns
that differentiate violent from nonviolent crimes. By building
predictive models, we can identify the characteristics and conditions
that are most strongly associated with violent incidents. This analysis
provides insight into factors driving violent behavior, guiding further
research and data-driven policy discussions without promoting predictive
policing.

Urban crime is deeply influenced by social, economic and demographic
factors, as demonstrated in previous studies of Los Angeles and other
major cities [@lee2020]. Research consistently shows that neighborhoods
with higher poverty rates, housing instability, and limited access to
social services tend to experience elevated violent crime rates, while
demographic characteristics, including racial composition, shape both
where crime occurs and how law enforcement interacts with residents
[@rosenfeld2023]. These patterns indicate that crime is not evenly
distributed and that predictive models benefit from including features
capturing both spatial context and population characteristics.

To evaluate the impact of spatiotemporal information on classification
performance, we implemented a two-model strategy using Random Forest
classifiers. The baseline model includes only demographic and
crime-specific features, such as victim age, sex, descent, and the type
of crime. This model provides a benchmark for understanding the
predictive value of purely intrinsic crime and victim characteristics.

The full model extends the baseline by incorporating temporal (hour, day
of the week, month) and spatial (latitude, longitude, district, area)
features. This allows us to test whether information about when and
where a crime occurs improves predictive accuracy and feature
interpretability. Comparing these two models highlights the relative
importance of spatiotemporal context and helps determine which features
are most influential in predicting violent crime.

This design also allows us to quantify model performance gains from
including spatiotemporal information, using metrics such as ROC AUC,
precision-recall AUC, and overall accuracy. Understanding these
differences is critical for interpreting feature importance and
assessing which characteristics of the dataset contribute the most
strongly to classification of violent crimes. Our approach mimics
methods in spatial criminology and data-driven simulation models
[@Roses2021], capturing both temporal trends and spatial dependencies
while also considering demographic context. By integrating these
features, our models aim not only to classify crime severity but also to
reveal which factors most strongly influence violent incidents,
providing actionable insights for policy and resource allocation.

Table [1](#tab:rf_performance){reference-type="ref"
reference="tab:rf_performance"} presents the performance metrics of the
two Random Forest models: the baseline model without spatiotemporal
features and the full model including these additional features. Each
model was trained in a 70-30% train-test split and evaluated using
standard classification metrics, including accuracy, ROC AUC, and
precision-recall AUC. The table highlights the differences in predictive
power between the models, showing the extent to which incorporating
temporal and spatial context improves the ability to distinguish violent
from nonviolent incidents. Although both models capture general
patterns, the inclusion of spatiotemporal features in the full model
provides a more nuanced representation of the data, improving overall
classification performance and identifying which demographic or
spatiotemporal factors most influence violent crime.\

::: {#tab:rf_performance}
  ---------- ------------------- ------------
               No_Spatiotemporal   Full_Model
  Metric                         
  ROC AUC                  0.747        0.757
  PR AUC                   0.661        0.673
  Accuracy                 0.656        0.676
  ---------- ------------------- ------------

  : Performance metrics of the Random Forest models: baseline without
  spatiotemporal features (No_Spatiotemporal) versus full model
  including spatiotemporal features (Full_Model).
:::

The Random Forest model that used only demographic and crime-specific
features achieved a ROC AUC of 0.747, a PR AUC of 0.661, and an overall
accuracy of 0.656. The full model, which includes temporal and spatial
features, achieved a ROC AUC of 0.757, a PR AUC of 0.673, and an overall
accuracy of 0.676 (Table [1](#tab:rf_performance){reference-type="ref"
reference="tab:rf_performance"}). The inclusion of spatiotemporal
information improved all three metrics, with the largest gains observed
in PR AUC and overall accuracy, indicating that time and location
provide additional predictive value. However, the similarity in ROC AUC
suggests that intrinsic victim and crime characteristics remain the
primary drivers of model performance. These results provide context for
the subsequent feature importance analysis, highlighting which features
consistently influence the model's predictions.\
To interpret the Random Forest models, we examined permutation
importances and feature distributions to identify which predictors most
strongly influence the violent--nonviolent classification. Since the two
models performed similarly, feature analysis helps determine whether
demographic, crime-specific, or spatiotemporal variables meaningfully
contribute to the model's decisions, even if they do not substantially
change overall accuracy.

![Top 10 permutation importances for the full Random Forest
model.](Results Figures/top10_feature_importance.png){#fig:top10_perm
width="80%"}

The permutation importance results in Figure
[7](#fig:top10_perm){reference-type="ref" reference="fig:top10_perm"},
show that the model's predictive power is driven primarily by four
demographic features: Victim Sex: Non-binary/Unknown, Victim Descent:
Unknown, Victim Descent: Hispanic, and Victim Age. These features
consistently produce the largest drops in model performance when
permuted, indicating that they carry the strongest signal for
distinguishing violent from nonviolent incidents. The presence of two
"unknown" categories at the top suggests that missing or ambiguous
demographic information itself is highly informative. This likely
reflects reporting inconsistencies or systematic differences in how
certain incidents are documented. The high importance of Hispanic victim
descent aligns with the demographic composition of Los Angeles and
suggests that incidents involving Hispanic victims may display distinct
patterns relevant to violence classification. Victim age appearing as an
important factor reflects well-documented patterns in criminology that
the likelihood of violence differs across age groups. These results
suggest that demographic structure influences how the model separates
violent from nonviolent incidents.\
The confusion matrix from the Random Forest model (Figure
[8](#fig:conf_matrix_full){reference-type="ref"
reference="fig:conf_matrix_full"}) indicates that the classifier
performs asymmetrically across violent and nonviolent incidents. While
it correctly identifies the majority of nonviolent cases, a notable
portion of violent incidents are misclassified as nonviolent. This
pattern reflects a conservative bias toward the negative class, a common
outcome in imbalanced datasets where violent crimes are less frequent.
The distribution of errors suggests that, although demographic and
contextual features contribute to distinguishing crime severity, they do
not fully capture the variability of violent events. This limited
sensitivity to less frequent violent cases should be considered when
interpreting the subsequent feature importance analyses.

![Confusion matrix of the Random Forest full model, showing
classification performance for violent and nonviolent
incidents.](Results Figures/conf_matrix_full.png){#fig:conf_matrix_full
width="70%"}

The Random Forest model identifies victim demographic features as the
most predictive for distinguishing violent from nonviolent incidents.
Victim sex, particularly non-binary or unknown categories, and victim
descent, especially unknown and Hispanic, consistently produce the
largest performance drops when permuted, indicating they carry the
strongest signal. Victim age also ranks highly, reflecting
well-established criminological patterns in which the risk of
involvement in violence varies across the life course. The prominence of
"unknown" categories suggests that missing or ambiguous data itself
provides informative patterns, likely reflecting reporting
inconsistencies or systematic differences in incident documentation.
These results demonstrate that both observed demographic characteristics
and data completeness influence how the model classifies crime severity.

Patterns across features reveal that demographic and crime-specific
information drives most of the model's predictive power, while temporal
and spatial features provide modest improvements. The model's
misclassification of a substantial portion of violent incidents as
nonviolent highlights the impact of imbalanced classes and the relative
rarity of violent events in the dataset. These limitations constrain
sensitivity to lower-frequency incidents and should be considered when
interpreting feature importance. Connections to criminological theory
support the observed patterns: age-related differences in violent
behavior, neighborhood demographic composition, and systemic reporting
variations provide context for why victim characteristics, rather than
temporal or spatial cues, dominate the predictive signal. These insights
clarify which factors most strongly shape the classification of violent
crime and indicate avenues for further research into social and
structural determinants of violence.

## Model Comparison

![ROC Curve Comparison Between Logistic Regression and Random Forest
Models](Results Figures/ROC COMPARISON.png){#fig:roc_compare
width="70%"}

The ROC comparison in Figure [9](#fig:roc_compare){reference-type="ref"
reference="fig:roc_compare"} highlights the performance differences
between the logistic regression and Random Forest models. While both
classifiers perform meaningfully above the 0.50 baseline of random
chance, the Random Forest consistently achieves a higher true positive
rate across almost all false positive rates, resulting in a larger AUC
(0.753 vs. 0.723). This indicates that the Random Forest is better at
separating violent from non-violent crimes regardless of the chosen
decision threshold, reflecting its ability to capture nonlinear
interactions and complex feature relationships that logistic regression
cannot. The improvement, though moderate, suggests that violent crime
prediction benefits from modeling flexibility, especially in contexts
where demographic, spatial, and temporal factors interact in
non-additive ways. However, the gap between the curves also underscores
a broader limitation: even the stronger model does not approach high
discriminatory power, implying that many determinants of violent crime
are unobserved, unmeasured, or inherently unpredictable. These results
reinforce the need for caution when interpreting model outputs and
suggest that algorithmic predictions should complement, not replace,
contextual criminological understanding.

# Challenges

Our analysis faced several methodological challenges that mirror
well-documented issues in spatial criminology and crime prediction
research. A major difficulty stemmed from working with a dataset of
nearly one million records, which imposed substantial computational
demands throughout the modeling pipeline. Long runtimes during
preprocessing, cross-validation, and particularly permutation importance
restricted how thoroughly we could tune or compare models. In addition,
although the dataset is large, violent crime represents a relatively
small proportion of total incidents. This imbalance made it difficult
for models to correctly identify violent cases and contributed to
consistently elevated false-negative rates. Prior research has noted
that imbalanced outcomes create a "glass-ceiling" effect in which even
sophisticated algorithms struggle to substantially improve
classification accuracy for minority classes [@liu2011; @oh2022].

Beyond computational constraints and class imbalance, accurately
modeling spatial crime dynamics presented its own set of challenges.
Crime patterns in cities are shaped by multiple overlapping processes,
broad structural trends, neighborhood-specific characteristics, routine
activities, and localized environmental conditions. Our models, which
operate on tabular data at the incident level, can only approximate
these complex relationships. Existing studies highlight that crime
clusters often arise from both city-wide patterns and highly localized
dynamics that are difficult to capture using standard machine-learning
approaches [@Balocchi2019; @quick2018]. Moreover, as Hipp and Williams
(2020) [@Hipp2020] observe, theoretical development in spatial
criminology has not fully kept pace with the increasing granularity of
modern crime data. As a result, models may detect spatial patterns but
offer limited insight into the underlying processes that produce them.

Finally, the use of administrative police data introduces important
ethical and interpretive limitations. Crime datasets reflect not only
actual incidents but also patterns in reporting behavior, documentation
quality, and law-enforcement practices. Variation in how incidents are
recorded, substantial numbers of "unknown'' demographic entries, and
uneven policing across neighborhoods can introduce structural bias that
shapes model predictions. These limitations are especially salient in
communities that experience historical underreporting or
over-surveillance, often low-income or minority neighborhoods. As such,
predictive models must be used cautiously. Without careful
interpretation, they risk reinforcing existing inequalities rather than
offering objective insights. Our findings underscore the importance of
using these models to understand structural patterns, not to inform
individual-level predictions or enforcement decisions.
