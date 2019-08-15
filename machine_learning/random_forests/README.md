
# Random Forests

Ref: [Fast.ai: Introduction to random forests](http://course18.fast.ai/lessonsml1/lesson1.html)
- See Jupiter notebook `ml/randomForest/Bulldozers.ipynb`
- Random forest are easy, generic and require no prior assumptions
- To use a random forest you need to convert categorical data to numeric.

Ref: [Fast.ai: Random forest deep dive](http://course18.fast.ai/lessonsml1/lesson2.html)
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


Ref: [Fast.ai: Performance, Validation and model interpretation](http://course18.fast.ai/lessonsml1/lesson3.html)
- Profiling on Jupiter notebooks: `%time function()`, `%prun function()` runs into the profiler
- Calculate the variance of the estimators (i.e. all trees) to have an estimate of the "confidence" the model has in the predicted value.
- Feature importance: 
	- How to calculate: Create a predictor, take the variable to analyze and randomly shuffle the values in the trainig set, calculate how much the prediction is degraded (compare to the original unshuffled dataset). The higher the differece, the more important the variable is.
	- You can use "feature importance" to remove unnecesary variables (when plotting the feature importance curve, remove variables after the curve flattens)
	- You can also use to find "data leakage" (which might indicate problems with the raining dataset)
	- Removing unnecesary variables, you might reduce feature co-linearity.
	
	

Ref: [Fast.ai: Feature importance and Tree interpreter](http://course18.fast.ai/lessonsml1/lesson4.html)
- Depth of the tree is `log2(number_of_samples)`
- Number of leaves in the tree is `number_of_samples` for a fully trained tree
- OOB (Out Of Bag): Error calculated from "Out of bag" samples, allows you to not have  atest set.
- 


- Predict the test dataset: Create a dataset having the training set and the test set, add a columnt "is_test" and try to predict that column. If you can do a decent prediction, this means that the train and test datasets are like "time sequences" and have some sort of cutting point.
	- You can run a feature importance analysis to find out what the "time" dependent variables are, and drop the variables from the input.
	
