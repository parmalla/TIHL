- [1. Chapter 2](#1-chapter-2)
  - [1.1. Prepare the data for ML algorithms](#11-prepare-the-data-for-ml-algorithms)
    - [1.1.1. Data Cleaning](#111-data-cleaning)
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