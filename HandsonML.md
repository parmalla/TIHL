- [1. Chapter 2](#1-chapter-2)
  - [1.1. Prepare the data for ML algorithms](#11-prepare-the-data-for-ml-algorithms)
    - [1.1.1. Data Cleaning](#111-data-cleaning)
    - [1.1.2. Handling Text and Categorical Attributes](#112-handling-text-and-categorical-attributes)
      - [1.1.2.1. Normal Encoding](#1121-normal-encoding)
      - [1.1.2.2. OneHot Encoding](#1122-onehot-encoding)
    - [1.1.3. Custom Transformers](#113-custom-transformers)
    - [1.1.4. Transformation Pipelines](#114-transformation-pipelines)
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