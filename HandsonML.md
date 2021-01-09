- [1. Chapter 2](#1-chapter-2)
  - [1.1. Prepare the data for ML algorithms](#11-prepare-the-data-for-ml-algorithms)
    - [1.1.1. Data Cleaning](#111-data-cleaning)
    - [1.1.2. Handling Text and Categorical Attributes](#112-handling-text-and-categorical-attributes)
      - [1.1.2.1. Normal Encoding](#1121-normal-encoding)
      - [1.1.2.2. OneHot Encoding](#1122-onehot-encoding)
    - [1.1.3. Custom Transformers](#113-custom-transformers)
    - [1.1.4. Transformation Pipelines](#114-transformation-pipelines)
  - [1.2. Select and train a model](#12-select-and-train-a-model)
    - [1.2.1. Training and Evaluating on the Training Set](#121-training-and-evaluating-on-the-training-set)
    - [1.2.2. Better Evaluation Using Cross Validation](#122-better-evaluation-using-cross-validation)
  - [1.3. Fine Tune Your Model](#13-fine-tune-your-model)
    - [1.3.1. Grid Search](#131-grid-search)
    - [Randomized Search](#randomized-search)
# 1. Chapter 2
## 1.1. Prepare the data for ML algorithms
### 1.1.1. Data Cleaning
```Python
from sklearn.impute import SimpleImputer
imputer = SimpleImputer(strategy="median")
#Remove the text attribute because median can only be calculated on numerical attributes:
housing_num = housing.select_dtypes(include=[np.number])
imputer.fit(housing_num)
imputer.statistics_
#Check that this is the same as manually computing the median of each attribute:
housing_num.median().values
#Transform the training set:
X = imputer.transform(housing_num)
housing_tr = pd.DataFrame(X, columns=housing_num.columns,
                        index=housing_num.index)
```
[Link](https://colab.research.google.com/github/ageron/handson-ml2/blob/master/02_end_to_end_machine_learning_project.ipynb#scrollTo=tm1NMvG_cWbR)
### 1.1.2. Handling Text and Categorical Attributes
#### 1.1.2.1. Normal Encoding
```Python
from sklearn.preprocessing import OrdinalEncoder
ordinal_encoder = OrdinalEncoder()
housing_cat_encoded = ordinal_encoder.fit_transform(housing_cat)
housing_cat_encoded[:10]
ordinal_encoder.categories_
```
[Link](https://colab.research.google.com/github/ageron/handson-ml2/blob/master/02_end_to_end_machine_learning_project.ipynb#scrollTo=NVXPJM6ccWbU)
#### 1.1.2.2. OneHot Encoding
```Python
from sklearn.preprocessing import OneHotEncoder
cat_encoder = OneHotEncoder()
housing_cat_1hot = cat_encoder.fit_transform(housing_cat)
housing_cat_1hot
cat_encoder.categories_
```
[Link](https://colab.research.google.com/github/ageron/handson-ml2/blob/master/02_end_to_end_machine_learning_project.ipynb#scrollTo=Ye1DztXKcWbV)
### 1.1.3. Custom Transformers
```Python
from sklearn.base import BaseEstimator, TransformerMixin

col_names = "total_rooms", "total_bedrooms", "population", "households"
rooms_ix, bedrooms_ix, population_ix, households_ix = [
    housing.columns.get_loc(c) for c in col_names] # get the column indices

class CombinedAttributesAdder(BaseEstimator, TransformerMixin):
    def __init__(self, add_bedrooms_per_room=True): # no *args or **kargs
        self.add_bedrooms_per_room = add_bedrooms_per_room
    def fit(self, X, y=None):
        return self  # nothing else to do
    def transform(self, X):
        rooms_per_household = X[:, rooms_ix] / X[:, households_ix]
        population_per_household = X[:, population_ix] / X[:, households_ix]
        if self.add_bedrooms_per_room:
            bedrooms_per_room = X[:, bedrooms_ix] / X[:, rooms_ix]
            return np.c_[X, rooms_per_household, population_per_household,
                         bedrooms_per_room]
        else:
            return np.c_[X, rooms_per_household, population_per_household]

attr_adder = CombinedAttributesAdder(add_bedrooms_per_room=False)
housing_extra_attribs = attr_adder.transform(housing.values)
```
[Link](https://colab.research.google.com/github/ageron/handson-ml2/blob/master/02_end_to_end_machine_learning_project.ipynb#scrollTo=2Lc1Nm-VcWbW)
### 1.1.4. Transformation Pipelines
```Python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler

num_pipeline = Pipeline([
        ('imputer', SimpleImputer(strategy="median")),
        ('attribs_adder', CombinedAttributesAdder()),
        ('std_scaler', StandardScaler()),
    ])

housing_num_tr = num_pipeline.fit_transform(housing_num)
```
[Link](https://colab.research.google.com/github/ageron/handson-ml2/blob/master/02_end_to_end_machine_learning_project.ipynb#scrollTo=CdBnQPO8fMlY)
## 1.2. Select and train a model
### 1.2.1. Training and Evaluating on the Training Set
```Python
from sklearn.linear_model import LinearRegression

lin_reg = LinearRegression()
lin_reg.fit(housing_prepared, housing_labels)

# let's try the full preprocessing pipeline on a few training instances
some_data = housing.iloc[:5]
some_labels = housing_labels.iloc[:5]
some_data_prepared = full_pipeline.transform(some_data)

print("Predictions:", lin_reg.predict(some_data_prepared))

from sklearn.metrics import mean_squared_error

housing_predictions = lin_reg.predict(housing_prepared)
lin_mse = mean_squared_error(housing_labels, housing_predictions)
lin_rmse = np.sqrt(lin_mse)
lin_rmse

from sklearn.metrics import mean_absolute_error

lin_mae = mean_absolute_error(housing_labels, housing_predictions)
lin_mae
# new model
from sklearn.tree import DecisionTreeRegressor

tree_reg = DecisionTreeRegressor(random_state=42)
tree_reg.fit(housing_prepared, housing_labels)

housing_predictions = tree_reg.predict(housing_prepared)
tree_mse = mean_squared_error(housing_labels, housing_predictions)
tree_rmse = np.sqrt(tree_mse)
tree_rmse
```
[Link](https://colab.research.google.com/github/ageron/handson-ml2/blob/master/02_end_to_end_machine_learning_project.ipynb#scrollTo=JPpErlr_FDDD)
### 1.2.2. Better Evaluation Using Cross Validation
```Python
from sklearn.model_selection import cross_val_score

scores = cross_val_score(tree_reg, housing_prepared, housing_labels,
                         scoring="neg_mean_squared_error", cv=10)
tree_rmse_scores = np.sqrt(-scores)
```
## 1.3. Fine Tune Your Model
### 1.3.1. Grid Search
```Python
from sklearn.model_selection import GridSearchCV

param_grid = [
    # try 12 (3×4) combinations of hyperparameters
    {'n_estimators': [3, 10, 30], 'max_features': [2, 4, 6, 8]},
    # then try 6 (2×3) combinations with bootstrap set as False
    {'bootstrap': [False], 'n_estimators': [3, 10], 'max_features': [2, 3, 4]},
  ]

forest_reg = RandomForestRegressor(random_state=42)
# train across 5 folds, that's a total of (12+6)*5=90 rounds of training 
grid_search = GridSearchCV(forest_reg, param_grid, cv=5,
                           scoring='neg_mean_squared_error',
                           return_train_score=True)
grid_search.fit(housing_prepared, housing_labels)

# The best hyperparameter combination found:
grid_search.best_params_

# Score of each hyperparameter combination tested during the grid search:
cvres = grid_search.cv_results_
for mean_score, params in zip(cvres["mean_test_score"], cvres["params"]):
    print(np.sqrt(-mean_score), params)
```
### Randomized Search