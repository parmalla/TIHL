## AWS Machine Learning  Pointers

------

### Software Engineering Practises

#### Code Refactoring

Bad

```python
labels = list(df.columns)

for i in range(len(labels)):
    labels[i] = labels[i].replace(' ','_')

df.columns = labels

df.head()
```

Refactored

```python
df.columns = [label.replace(' ', '_') for label in df.columns]
df.head()
```

Bad

```python
median_alcohol = df.alcohol.median()
for i, alcohol in enumerate(df.alcohol):
    if alcohol >= median_alcohol:
        df.loc[i, 'alcohol'] = 'high'
    else:
        df.loc[i, 'alcohol'] = 'low'
df.groupby('alcohol').quality.mean()

median_pH = df.pH.median()
for i, pH in enumerate(df.pH):
    if pH >= median_pH:
        df.loc[i, 'pH'] = 'high'
    else:
        df.loc[i, 'pH'] = 'low'
df.groupby('pH').quality.mean()

median_sugar = df.residual_sugar.median()
for i, sugar in enumerate(df.residual_sugar):
    if sugar >= median_sugar:
        df.loc[i, 'residual_sugar'] = 'high'
    else:
        df.loc[i, 'residual_sugar'] = 'low'
df.groupby('residual_sugar').quality.mean()

median_citric_acid = df.citric_acid.median()
for i, citric_acid in enumerate(df.citric_acid):
    if citric_acid >= median_citric_acid:
        df.loc[i, 'citric_acid'] = 'high'
    else:
        df.loc[i, 'citric_acid'] = 'low'
df.groupby('citric_acid').quality.mean()
```

Refactored

```python
def numeric_to_buckets(df, column_name):
    median = df[column_name].median()
    for i, val in enumerate(df[column_name]):
        if val >= median:
            df.loc[i, column_name] = 'high'
        else:
            df.loc[i, column_name] = 'low' 
 
for feature in df.columns[:-1]:
    numeric_to_buckets(df, feature)
    print(df.groupby(feature).quality.mean(), '\n')
```

------

### Efficient Code

![Optimising](Images\Optimising.jpg)