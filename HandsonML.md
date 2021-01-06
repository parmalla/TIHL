- [1. Chapter 2](#1-chapter-2)
  - [1.1. Prepare the data for ML algorithms](#11-prepare-the-data-for-ml-algorithms)
    - [1.1.1. Data Cleaning](#111-data-cleaning)
    - [1.1.2. Handling Text and Categorical Attributes](#112-handling-text-and-categorical-attributes)
      - [1.1.2.1. Normal Encoding](#1121-normal-encoding)
      - [1.1.2.2. OneHot Encoding](#1122-onehot-encoding)
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