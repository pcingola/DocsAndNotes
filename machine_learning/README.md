
# ML & AI: Notes & Learning points

Some learning points about ML & AI.
These are in no particular order


# General ML

- Course of dimentionality is not a real concern in practice for many ML problems
- The "No free lunch" theorem doesn't usually apply in practice because practical ML problems are not totally random data (i.e. the data lies within a sub-manifold)
- `r^2 = 1-SS_reg / SS_tot`, the second term is the ratio between `SS_reg` (sum of squared errors of our model) and `SS_tot` (sum of squared error of a very naive estimator, that is the just the mean). This is a number below 1.0, it can be negative infinity (not between 0 and 1 as many people think)
- Build 5 basic models (one better than the other), test them on the test and validation set. The scores should be in the same direcction, otherwise the test/validation models are biased.
- For exploring / finding models you probably don't need millions of datapoints. Subsample to a point when you can get the models to train within a few minutes/seconds so you can iterate faster.
- In ML we care about generalization (if the model doesn't generalize, we are doing a bad job at predicting).

# Data pre-processing

- Try sorting accordingly if the categorical data has an ordering, e.g. `['low' 'medium' 'high']` it sometimes provides a small advantage (by default most packages just sort alphabetically)
- Expand 'data/timestamp' into many columns (`year, month, day, day_of_weeK, is_holliday`, etc.)
- Saving the data in `feather` format is udualy fast (the dataFrame is just dumped from memory to disk, so it's fast)
- Dividing 80% training, 10% testing and 10% validation works usualy OK, but if you have millions of samples, you might only need 1% for test and validation (or even 0.1%)
- Add a column for missing data: e.g. if your column is called `value`
    - Create a new column `value_na` which is `{0, 1}` depending on whether the value is missing or not
    - Fill missing values in `value` by setting them to the mean / median / default
- Do basic statistics: mean, stdev, max, min, number of missing values, distribution, etc.
- If you are interested on money amounts, it might be better to use the `log`
- Perform a "one hot encoding" for categorical input variables, most machine learning models need this, An exception can be categories with high cardinality (i.e. too many options).

# Exploratory data analaysis

- Calculate ranking variables having missing values
- Create a shallow RandomForest model having a single tree: it's a crappy model, but you can draw the tree to gain insight into the data.
- For large datasets, there is no point on always using all the samples to train the model during the explaratory analysis: Just sub-sample your dataset
- Perform a "feature impoirtance analysis" (see "Randome forest" for details).
- Perform hierarchical clustering (use spearman's rank-order correlation `pearmanr`)

# Ensemble models
- Bagging: Create many models that are somewhat predictive, but have un-correlated errors. When you average all the models, you have `mean(prediction_i + error_i) = mean(prediction_i) + mean(error_i)`. The second term tends to zero (errors are uncorrelated with mean 0), so we have a much better predition.
- Bag if little bootstraps: It is more important to be uncorrelated than predictive (e.g. Extremelly randomized trees)
- Calculate the variance of the estimators to have an estimate of the "confidence" the ensembl model has in the predicted value. Plot the dendogram to show variables that are somehow similar / colinear


