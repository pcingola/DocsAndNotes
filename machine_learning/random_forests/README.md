
# Random Forests

The followin are base on [Introduction to Machine Learning for Coders](http://course18.fast.ai/ml)

Ref: [Fast.ai: ML 1, Introduction to random forests](http://course18.fast.ai/lessonsml1/lesson1.html)
- See Jupiter notebook `ml/randomForest/Bulldozers.ipynb`
- Random forest are easy, generic and require no prior assumptions
- To use a random forest you need to convert categorical data to numeric.

Ref: [Fast.ai: ML 2, Random forest deep dive](http://course18.fast.ai/lessonsml1/lesson2.html)
- Create a shallow RandomForest model having a single tree: it's a crappy model, but you can draw the tree to gain insight into the data.
- Building a tree:
  - How to build a RandomForest: Just build a bunch of Trees
  - How to build a Tree: For each variable, for each possible splits (middle point betwee in your dataset), pick the 'best' split
  - How to compare 'splits': For each split compare the r^2 of the parent node vs the weighted average of the r^2 of the child nodes
  - In practice: Gini gain is often used
- OOB: Out of bag predictions. Use the samples that were not used by bootstrapping to test the Tree, average for all trees that were not using the sample in order to get the tree ensamble average (note that this leaves some trees out, so the average is worst than a test set)
- Tunning the RandomFores model:
	- `n_estimators`: Suggested values are `{10, 50, 100}`. Increasing the number of trees in the RandomForest reached a plateau at some point. Usualy this is after 100 trees for small datasets
	- `min_samples_leaf`: Suggested values are `{1, 3, 10, 100}`. By default RandomForest stops building the tree when every sample is a leaf node. Use `min_samples_leaf > 1` to stop earlier (you want generalization, going all the way down will overfit the tree).
	- `max_features`: Suggested values are `{0.5, 'srqt', 'log2'}`. To create uncorrelated trees, you can randomly limit the number of parameters (i.e. columns in your data frame):e.g. `max_features=0.5` other good options are `'sqrt', 'log2'`
	- `n_jobs=-1`: Parallelize into all the CPUs


Ref: [Fast.ai: ML 3, Performance, Validation and model interpretation](http://course18.fast.ai/lessonsml1/lesson3.html)
- Profiling on Jupiter notebooks: `%time function()`, `%prun function()` runs into the profiler
- Calculate the variance of the estimators (i.e. all trees) to have an estimate of the "confidence" the model has in the predicted value.
	- Calculate the stdev of a single sample to understand how certain the model is about a prediction
	- Calculate the stdev of a group of samples (e.g. all samples having a one categorical value). This is used to find "groups" with low confidence (e.g. if a group has small number of samples)
- Feature importance: 
	- Using linear regression for "feature importance" is usually not accurrate because of the number of assumptions you are implicitly making in the lnear model (e.g. no feature interactions). Random forests make fewer assumptions, thus usually more accurate.
	- How to calculate: Create a predictor, take the variable to analyze and randomly shuffle the values in the trainig set, calculate how much the prediction is degraded (compare to the original unshuffled dataset). The higher the differece, the more important the variable is.
	- You can use "feature importance" to remove unnecesary variables (when plotting the feature importance curve, remove variables after the curve flattens)
	- You can also use to find "data leakage" (which might indicate problems with the raining dataset)
	- Removing unnecesary variables, you might reduce feature co-linearity.

Ref: [Fast.ai: ML 4, Feature importance and Tree interpreter](http://course18.fast.ai/lessonsml1/lesson4.html)
- Depth of the tree is `log2(number_of_samples)`
- Number of leaves in the tree is `number_of_samples` for a fully trained tree
- OOB (Out Of Bag): Error calculated from "Out of bag" samples, allows you to not have  atest set.
- Random forest usually don't overfit
- Perform hierarchical clustering: Remove variables that are too similar (try one by one, checking that the model doesn't drop when removing the variables)
- Partial dependence: All things being equal, what's the (partial) dependency between two variables (according to our model)
	- Partial dependence plots: Perform a plot of "all things being equal, except the choosen variable". For a choosen variable perform a prediction by replacing all values in the dataset by a fixed value, once the predictions are performed, we get an average of "all things where equal, except the choosen variable", then we change the value and plot the averages vs the replaced value. Good method for plotting a correlation whithout the colinear effects.
	- Use Python library `pdp`
	- `pdp.interact`: To anlyze how variable interact (using Partial Dependence Plots)
- Tree interpreter: Analyze the contribution of each variable change
- Show a graphical model of a single tree

Ref: [Fast.ai: ML 5, Extrapolation and RF from scratch](http://course18.fast.ai/lessonsml1/lesson5.html)
- OOB can replace a validation set (with the score a bit lower than in real validation since the number of trees used is lower)
- Build 5 basic models (one better than the other), test them on the test and validation set. The scores should be in the same direcction, otherwise the test/validation models are biased.
- Cross validation is usefull if you have few data points, but mostly useless when you have a lot of data
- Tree interpreter + Waterflow plots are used to show how a Random Forest gets a result (justify a result and analyze the data)
- Predict the test dataset: Create a dataset having the training set and the test set, add a columnt "is_test" and try to predict that column. If you can do a decent prediction, this means that the train and test datasets are like "time sequences" and have some sort of cutting point.
	- What's the difference between validation set and training set? This indicates predictors with strong temporal componene (if the train/validation were split at a point in time). These features ar irrelevant for predition since they will not apply when the model is in production.
	- You run a feature importance analysis to find out what the "time" dependent variables are, and drop the variables from the input.
	
