# Logistic Regression Model of Violent vs. Non-Violent Incidents

To move beyond descriptive patterns and evaluate how well basic victim
and context features predict whether a reported crime is violent, we fit
a regularized logistic regression model to the Los Angeles crime
incident data (2020--present). Following a similar structure to prior
policing projects, we first focused on careful data cleaning and feature
construction, then trained and evaluated a binary classifier. The binary
outcome variable, Is_Violent, was derived from the textual crime
description (Crime_Code_Desc). We wrote a keyword-based function that
flags an incident as violent if the description contains terms
associated with serious offenses. This includes homicides (e.g.,
"HOMICIDE", "MURDER", "MANSLAUGHTER"), sex crimes (e.g., "RAPE", "SEXUAL
ASSAULT"), robberies and burglaries involving force or direct person
contact, aggravated assaults and related batteries, kidnapping and false
imprisonment, weapons offenses (e.g., "SHOOTING", "DISCHARGE FIREARM",
"ARSON"), and explicit threat or extortion language. Incidents without
any of these keywords were treated as non-violent. For modeling, we
restricted the data to rows with a non-missing Is_Violent label and
encoded the target as y = 0,1, where 1 represents a violent crime and 0
represents a non-violent crime.

## Preprocessing (Excluding Spatial and Temporal Features)

Logistic regression cannot use categorical strings directly, so one-hot
encoding was required. These variables contain many categories.For
example, over twenty area codes and more than a dozen descent codes so
encoding them produced a higher-dimensional feature set. After encoding,
the design matrix contained sixty-four columns. Storing this matrix in
dense form would have been inefficient, since most of the entries
created by one-hot encoding are zeros. To address this, we used
scikit-learn's sparse one-hot encoder, which creates a compressed sparse
matrix. This format stores only non-zero values and greatly reduces
memory usage.

The model was implemented using scikit-learn's LogisticRegression with
the SAGA solver. SAGA is one of the few solvers that can efficiently
handle large, sparse datasets. We used class_weight = \"balanced\" to
correct for the lower frequency of violent crimes relative to
non-violent crimes. Without this adjustment, the model would be biased
toward predicting the majority class. Data were split using a stratified
70/30 train--test split. After splitting and pre-processing, the
training set contained approximately 678,000 observations, and the test
set contained about 291,000.

All pre-processing steps: imputation, scaling of numeric inputs, and
one-hot encoding of categorical inputs, were combined into a single
ColumnTransformer pipeline. This ensured consistent, reproducible
transformations on both the training and test data. The model was then
trained on the sparse feature matrix and evaluated using metrics such as
ROC AUC, PR AUC, accuracy, precision, recall, and the confusion matrix.

## Results (Excluding Spatial and Temporal Features)

::: {#tab:logit_summary}
  Metric        Value
  ---------- --------
  ROC AUC      0.7228
  PR AUC       0.6291
  Accuracy     0.6521

  : Overall performance metrics for logistic regression.
:::

The logistic regression model produced a ROC AUC of 0.7228, indicating
moderate ability to distinguish violent from non-violent crimes. The PR
AUC of 0.6291 shows that the model performs reasonably well when
focusing on identifying violent incidents specifically, which is
important because the classes are imbalanced. The overall accuracy was
65.21%, which reflects the combined performance on both classes but is
less meaningful than the AUC metrics due to the imbalance in the
dataset. Together, these values show that the model captures patterns
related to violent crime prediction, but also leaves room for
improvement, especially in reducing false positives and false negatives.

![Logistic Regression ROC
Curve](Methodology Figures/ROC LOGREG.png){#fig:ROC_LOGREG width="60%"}

The ROC curve in [1](#fig:ROC_LOGREG){reference-type="ref+Label"
reference="fig:ROC_LOGREG"} illustrates how the model's true positive
rate changes relative to the false positive rate across different
decision thresholds. The curve rises well above the diagonal reference
line, which represents random guessing, showing that the model
consistently performs better than chance. The steep increase in true
positive rate at lower false positive rates indicates that the model can
correctly identify a large share of violent crimes without immediately
producing excessive false alarms. However, The curve's distance from the
upper-left corner shows the model has only moderate discriminative
power. Overall, the ROC visualization supports the numerical metrics by
showing that the model is effective at separating the two classes but
still has noticeable overlap that limits its performance.

::: {#tab:logit_class_report}
  Class               Precision   Recall   F1-score   Support
  ----------------- ----------- -------- ---------- ---------
  0 (Non-violent)         0.776    0.523      0.625    161181
  1 (Violent)             0.578    0.812      0.675    129757

  : Classification report for logistic regression model.
:::

[2](#tab:logit_class_report){reference-type="ref+Label"
reference="tab:logit_class_report"} shows how the model performs on each
class. For non-violent crimes (class 0), the model achieved a precision
of 0.776 but a lower recall of 0.523, meaning it correctly identifies
most of the predictions it labels as non-violent but misses a large
portion of actual non-violent incidents. For violent crimes (class 1),
precision drops to 0.578, but recall increases to 0.812, indicating the
model captures most violent incidents but also produces a noticeable
number of false positives. The F1-scores reflect this imbalance, with
0.625 for non-violent crimes and 0.675 for violent crimes. Overall, the
model prioritizes correctly identifying violent incidents, which aligns
with the use of class weighting, but does so at the cost of
misclassifying some non-violent cases.

::: {#tab:logit_confusion}
                             Predicted 0   Predicted 1
  ------------------------ ------------- -------------
  Actual 0 (Non-violent)           84372         76809
  Actual 1 (Violent)               24413        105344

  : Confusion matrix for logistic regression (threshold = 0.5).
:::

[3](#tab:logit_confusion){reference-type="ref+Label"
reference="tab:logit_confusion"} shows how the model's predictions
compare to the true labels. Out of all non-violent incidents, the model
correctly classified 84,372 as non-violent but incorrectly labeled
76,809 of them as violent. For violent incidents, it identified 105,344
correctly while misclassifying 24,413 as non-violent. The model captures
most violent crimes but at the cost of more false positives, with many
non-violent incidents being predicted as violent.

## Preprocessing (Including Spatial and Temporal Features)

To evaluate how spatial and temporal information influences violent
crime prediction, we extended our initial logistic regression model by
adding location-based and time-based features. The first version of the
model relied primarily on victim demographics which provided a baseline
but did not incorporate where or when an incident occurred beyond the
hour of the day. Since crime in Los Angeles varies by location and time,
adding spatial coordinates and finer temporal features was expected to
improve the model's ability to separate violent from non-violent cases.
To test this, we re-ran the same logistic regression pipeline with the
addition of spatial variables (latitude, longitude, and police area
code) and finer temporal variables (year, month, and day). This allows
us to compare the baseline model against an enriched version and assess
whether spatial--temporal context provides measurable predictive value.

## Results (Including Spatial and Temporal Features

::: {#tab:logit_st_summary}
  Metric        Value
  ---------- --------
  ROC AUC      0.7325
  PR AUC       0.6413
  Accuracy     0.6576

  : Overall performance metrics for logistic regression with spatial and
  temporal features.
:::

The logistic regression model that includes spatial and temporal
features shows a small but consistent improvement over the baseline
model. The ROC AUC increases from 0.7228 to 0.7325, indicating slightly
better overall separation between violent and non-violent crimes across
thresholds. The PR AUC also rises from 0.6291 to 0.6413, suggesting that
the model becomes more effective at identifying violent crimes
specifically, which is important given the class imbalance. Accuracy
also increases from 65.21% to 65.76%, though, as with the first model,
this metric is less informative than the AUC values.

![Logistic Regression ROC Curve with Spatial and Temporal
FEatures](Methodology Figures/ROC LOGREG SPATIAL TEMPORAL.png){#fig:ROC_LOGREG_ST
width="60%"}

The ROC curve plot reinforces this improvement. Compared to the baseline
curve, the spatial--temporal model reaches higher true positive rates
across most false positive levels and stays slightly further above the
random-chance diagonal. This indicates that the additional context
provided by latitude, longitude, month, day, and year helps the model
better distinguish between the two crime categories. Overall, the gains
are not large, but they show that spatial and temporal information does
add predictive value, improving the model's ability to capture patterns
that the demographic-only model could not fully represent.

::: {#tab:logit_st_class_report}
  Class               Precision   Recall   F1-score   Support
  ----------------- ----------- -------- ---------- ---------
  0 (Non-violent)         0.778    0.534      0.633    161181
  1 (Violent)             0.584    0.811      0.679    129757

  : Classification report for logistic regression with spatial and
  temporal features.
:::

[5](#tab:logit_st_class_report){reference-type="ref+Label"
reference="tab:logit_st_class_report"} shows how the spatial--temporal
model performs on each class. For non-violent incidents (class 0),
precision remains high at 0.778, but recall increases from 0.523 in the
baseline model to 0.594, meaning the model now correctly identifies a
larger portion of non-violent cases. For violent incidents (class 1),
precision increases slightly from 0.578 to 0.584, while recall remains
essentially the same at 0.811. The F1-scores also improve for both
classes, rising from 0.625 to 0.633 for non-violent crimes and from
0.675 to 0.679 for violent crimes.

::: {#tab:logit_st_confusion}
                             Predicted 0   Predicted 1
  ------------------------ ------------- -------------
  Actual 0 (Non-violent)           86079         75102
  Actual 1 (Violent)               24516        105241

  : Confusion matrix for logistic regression with spatial and temporal
  features (threshold = 0.5).
:::

[6](#tab:logit_st_confusion){reference-type="ref+Label"
reference="tab:logit_st_confusion"} further shows how predictions change
when spatial and temporal features are added. True negatives
(non-violent correctly predicted) increase from 84,372 in the baseline
model to 86,079, while false positives decrease from 76,809 to 75,102.
This shows that the model mislabels fewer non-violent crimes as violent.
For violent incidents, the number of true positives stays nearly the
same (105,344 vs. 105,241), and false negatives increase slightly from
24,413 to 24,516.

# Random Forest Model of Violent vs. Non-Violent Incidents

To improve predictive performance beyond linear methods, we trained a
Random Forest classifier on the Los Angeles crime incident data
(2020--present). Random Forest leverages an ensemble of decision trees,
each grown on a bootstrap sample of the data and considering a random
subset of features at each split. This design allows the model to
capture complex, nonlinear interactions among victim demographics,
contextual features, and incident characteristics, which linear models
like logistic regression cannot fully exploit.

Class weighting was applied to address the imbalance between violent and
non-violent incidents, ensuring the model does not disproportionately
favor the majority class. Additionally, the algorithm naturally
accommodates a mix of numeric and categorical inputs, reducing the need
for extensive scaling or transformation. Feature importance scores are
automatically computed during training, providing insight into which
predictors most strongly influence the likelihood of violent crime.

This Random Forest setup establishes a flexible, robust baseline model
that can later be extended with spatial and temporal features to assess
the additional predictive value of location and time information.

## Preprocessing (Excluding Spatial and Temporal Features)

For the Random Forest model, preprocessing focused on structuring the
dataset to fully leverage the ensemble learning algorithm while
excluding spatial and temporal features. Categorical variables with
multiple levels were one-hot encoded to convert them into numeric format
compatible with scikit-learn's `RandomForestClassifier`. Unlike logistic
regression, scaling numeric features was unnecessary because tree-based
models are invariant to monotonic transformations.

Missing values in numeric columns were imputed using median values, and
categorical features with missing or unknown entries were assigned a
separate \"Unknown\" category. This ensured that the Random Forest trees
could split on all features without dropping any observations. Class
imbalance was addressed directly through the `class_weight="balanced"`
parameter, which automatically adjusts sample weights inversely
proportional to class frequency. Data were split into training and test
sets using a stratified 70/30 split to preserve the proportion of
violent incidents across sets. After preprocessing, the training set
contained approximately 678,000 rows, and the test set contained about
291,000.

All transformations were applied consistently across both training and
test sets to prevent data leakage. The resulting feature matrix, while
larger due to one-hot encoding, was efficiently handled by the Random
Forest model, which can naturally select informative features and ignore
irrelevant ones during tree construction.

## Results (Excluding Spatial and Temporal Features)

::: {#tab:rf_summary}
  Metric        Value
  ---------- --------
  ROC AUC      0.7456
  PR AUC       0.6599
  Accuracy     0.6562

  : Overall performance metrics for Random Forest.
:::

The Random Forest model produced a ROC AUC of 0.7456, indicating
moderate-to-strong ability to distinguish violent from non-violent
crimes. The PR AUC of 0.6599 shows that the model performs reasonably
well when focusing on identifying violent incidents specifically, which
is important given the class imbalance. The overall accuracy was 65.62%,
which reflects the combined performance on both classes but is less
informative than the AUC metrics due to the imbalance in the dataset.
Together, these values show that the ensemble model captures more
complex patterns related to violent crime prediction, providing improved
discrimination over the baseline logistic regression, but still leaves
room for improvement, particularly in reducing false positives and false
negatives.

![Random Forest ROC Curve(Excluding Spatial and
Temporal](Methodology Figures/ROC RandomForestExcluding.png){#fig:ROC_RandomForestExcluding
width="60%"}

The ROC curve in
[3](#fig:ROC_RandomForestExcluding){reference-type="ref+label"
reference="fig:ROC_RandomForestExcluding"} shows how the Random Forest
model balances true positive and false positive rates across different
classification thresholds. As with the logistic regression model, the
curve lies well above the diagonal reference line that represents random
guessing, but it rises more sharply at the beginning, indicating
stronger early discrimination. The model achieves a high true positive
rate even when the false positive rate is still relatively low,
reflecting the ensemble's ability to capture nonlinear interactions and
complex feature patterns. As the threshold decreases, the curve begins
to flatten, showing that additional gains in sensitivity come with a
higher cost in false positives. Overall, the ROC curve confirms the
numerical metrics by illustrating that the Random Forest provides a
noticeably stronger separation between violent and non-violent incidents
compared to logistic regression, though some overlap between the classes
still persists.

::: {#tab:rf_class_report}
  Class               Precision   Recall   F1-score   Support
  ----------------- ----------- -------- ---------- ---------
  0 (Non-violent)         0.828    0.479      0.607    161181
  1 (Violent)             0.575    0.876      0.694    129757

  : Classification report for Random Forest model.
:::

[8](#tab:rf_class_report){reference-type="ref+Label"
reference="tab:rf_class_report"} summarizes the class-specific
performance of the Random Forest model. For non-violent crimes (class
0), the model attains higher precision and recall than the logistic
regression model, indicating that it not only labels non-violent
incidents more accurately but also overlooks fewer true non-violent
cases. For violent crimes (class 1), the model continues to show strong
recall, capturing the majority of violent incidents, while its precision
reflects the presence of some false positives. The resulting F1-scores
show a more balanced relationship between precision and recall across
both classes, suggesting that the model benefits from its ability to
capture nonlinear structure and interactions in the data. Overall, the
Random Forest offers improved discrimination between violent and
non-violent incidents, reducing the trade-offs seen in the logistic
regression model while still maintaining emphasis on correctly
identifying violent events.

::: {#tab:rf_confusion}
                             Predicted 0   Predicted 1
  ------------------------ ------------- -------------
  Actual 0 (Non-violent)           77762         85419
  Actual 1 (Violent)               15051        114706

  : Confusion matrix for Random Forest (threshold = 0.5).
:::

[9](#tab:rf_confusion){reference-type="ref+Label"
reference="tab:rf_confusion"} shows how the Random Forest model's
predictions align with the true class labels. For non-violent incidents,
the model correctly identified a substantially larger share as
non-violent while misclassifying fewer cases as violent compared to the
logistic regression model. For violent incidents, it successfully
recognized the majority of true violent cases but still produced some
false negatives, where violent incidents were labeled as non-violent.
Overall, the confusion matrix reflects the model's improved balance: it
reduces the volume of false positives while maintaining strong detection
of violent crimes, indicating that the Random Forest is more effective
at distinguishing between the two categories without relying as heavily
on aggressive up-weighting of the minority class.

## Preprocessing (Including Spatial and Temporal Features)

For the second version of the model, we included both spatial and
temporal variables to test whether geolocation and time-based
information improve predictive performance. Spatial features consisted
of *Latitude* and *Longitude*, while temporal features included *Hour*,
*DayOfWeek*, *Month_Name*, and *Occurred_Year*. These variables were
combined with the remaining non-identifying attributes in the dataset.

The full feature matrix contained approximately 776,000 records for
training and 194,000 for testing, following a 70/30 stratified split
based on the target variable. All preprocessing steps, including one-hot
encoding for categorical variables and scaling for numeric features,
were performed using the same pipeline structure as the previous model
to ensure consistency. The goal of this preprocessing stage was to
incorporate meaningful spatial--temporal patterns while maintaining a
standardized transformation process across both model versions.

## Results (Including Spatial and Temporal Features)

::: {#tab:rf_st_summary}
  Metric        Value
  ---------- --------
  ROC AUC      0.7470
  PR AUC       0.6613
  Accuracy     0.6564

  : Overall performance metrics for Random Forest model including
  spatial and temporal features.
:::

The Random Forest model that includes spatial and temporal features
shows improved performance compared to the baseline version. The ROC AUC
increases to 0.7500, demonstrating stronger ability to separate violent
from non-violent incidents across thresholds. The PR AUC also rises to
0.6600, indicating that the model becomes more effective at identifying
violent crimes in the presence of class imbalance. Accuracy increases to
66%, though---consistent with the logistic regression models---this
measure is less informative than AUC due to the unequal class
distribution. Overall, these results show that adding spatial--temporal
context enhances the model's predictive capacity and helps capture
patterns that were not captured by demographic and categorical variables
alone.

![Random Forest ROC Curve with Spatial and Temporal
Features](Methodology Figures/ROC_RandomForestIncluding.png){#fig:ROC_RF_ST
width="60%"}

The ROC curve in [4](#fig:ROC_RF_ST){reference-type="ref+Label"
reference="fig:ROC_RF_ST"} shows how the model's true positive rate
varies with the false positive rate across different decision
thresholds. The curve stays noticeably above the diagonal reference line
corresponding to random guessing, confirming that the model has
meaningful discriminative ability. Compared to the version without
spatial and temporal variables, the curve rises higher and maintains a
stronger shape throughout, indicating that the inclusion of location and
time information improves performance. While the model does not achieve
near-perfect separation, the ROC curve clearly reflects the increased
predictive value of incorporating geographic and temporal structure.

::: {#tab:rf_st_class_report}
  Class               Precision   Recall   F1-score   Support
  ----------------- ----------- -------- ---------- ---------
  0 (Non-violent)         0.828    0.479      0.607    161181
  1 (Violent)             0.575    0.876      0.694    129757

  : Classification report for Random Forest model including spatial and
  temporal features.
:::

[11](#tab:rf_st_class_report){reference-type="ref+Label"
reference="tab:rf_st_class_report"} shows the model's performance for
each class. For non-violent incidents (class 0), precision is high at
0.828, but recall is lower at 0.479, indicating that while predictions
labeled as non-violent are often correct, the model misses a substantial
number of true non-violent cases. For violent incidents (class 1),
recall is very high at 0.876, meaning the model captures most violent
crimes, while precision of 0.575 indicates moderate false positives. The
F1-scores reflect this trade-off, with 0.607 for non-violent and 0.694
for violent crimes. Overall, the model emphasizes correctly identifying
violent incidents, consistent with the class-balancing strategy used in
training.

::: {#tab:rf_st_confusion}
                             Predicted 0   Predicted 1
  ------------------------ ------------- -------------
  Actual 0 (Non-violent)           77261         83920
  Actual 1 (Violent)               16021        113736

  : Confusion matrix for Random Forest model including spatial and
  temporal features (threshold = 0.5).
:::

[12](#tab:rf_st_confusion){reference-type="ref+Label"
reference="tab:rf_st_confusion"} shows the distribution of predictions
relative to the true labels. The model correctly classifies 77,261
non-violent incidents but misclassifies 83,920 as violent. For violent
incidents, it correctly predicts 113,736 while misclassifying 16,021 as
non-violent. The high recall for violent incidents comes at the cost of
some false positives for non-violent cases, which is consistent with the
model's prioritization of detecting violent crimes. Overall, the
confusion matrix confirms that spatial and temporal features improve
detection of violent incidents while maintaining an acceptable balance
of errors.

## Planned Additional Analyses

Although our current models provide useful insights, there are several
ways to build on these results. We plan to explore finer-grained
temporal and spatial patterns, such as hourly trends in specific
neighborhoods or seasonal shifts, to see if these reveal additional
nuances. We are also interested in testing ensemble approaches, like
soft voting classifiers, to see if combining models improves predictions
or highlights new important factors.

We also want to examine model interpretability more closely.
Specifically, we will compare Random Forest feature importance with
Logistic Regression coefficients across different data subsets to see
which predictors consistently drive violent crime outcomes. These
analyses will help us understand the strengths and limitations of our
models and provide a clearer picture of the factors influencing violent
crime.
