## AWS Machine Learning  Pointers

------
- [AWS Machine Learning  Pointers](#aws-machine-learning--pointers)
  * [Software Engineering Practises](#software-engineering-practises)
    
    + [Code Refactoring](#code-refactoring)
  * [Efficient Code](#efficient-code)
  * [Convert string to int in numpy](#convert-string-to-int-in-numpy)
  * [Optimising](#optimising)
  * [Unit Testing Tools](#unit-testing-tools)
  * [Test Driven Development and Data Science](#test-driven-development-and-data-science)
  * [Code Review](#code-review)
    + [Is the code clean and modular?](#is-the-code-clean-and-modular-)
    + [Is the code efficient?](#is-the-code-efficient-)
    + [Is documentation effective?](#is-documentation-effective-)
    + [Is the code well tested?](#is-the-code-well-tested-)
    + [Is the logging effective?](#is-the-logging-effective-)
  * [Tips for Conducting a Code Review](#tips-for-conducting-a-code-review)
    + [Tip: Use a code linter](#tip--use-a-code-linter)
    + [Tip: Explain issues and make suggestions](#tip--explain-issues-and-make-suggestions)
    + [Tip: Keep your comments objective](#tip--keep-your-comments-objective)
    + [Tip: Provide code examples](#tip--provide-code-examples)
    
      

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



### Efficient Code

![Optimising](/Images/Optimising.jpg)



### Convert string to int in numpy

```python
with open('gift_costs.txt') as f:
    gift_costs = f.read().split('\n')
    
gift_costs = np.array(gift_costs).astype(int)  # convert string to int
```



### Optimising

Reducing time through numpy condition selection.

```python
start = time.time()

total_price = 0
for cost in gift_costs:
    if cost < 25:
        total_price += cost * 1.08  # add cost after tax

print(total_price)
print('Duration: {} seconds'.format(time.time() - start))
```

```python
32765421.24
Duration: 5.947547674179077 seconds
```

```python
start = time.time()

cost = np.sum(gift_costs[gift_costs < 25])
total_price = cost * 1.08 # TODO: compute the total price

print(total_price)
print('Duration: {} seconds'.format(time.time() - start))
```

```python
32765421.24
Duration: 0.08053874969482422 seconds
```



### Unit Testing Tools

To install `pytest`, run `pip install -U pytest` in your terminal. You can see more information on getting started [here](https://docs.pytest.org/en/latest/getting-started.html).

- Create a test file starting with `test_`
- Define unit test functions that start with `test_` inside the test file
- Enter `pytest` into your terminal in the directory of your test file and it will detect these tests for you!

`test_` is the default - if you wish to change this, you can learn how to in this [`pytest` configuration](https://docs.pytest.org/en/latest/customize.html)

In the test output, periods represent successful unit tests and F's represent failed unit tests. Since all you see is what test functions failed, it's wise to have only one `assert` statement per test. Otherwise, you wouldn't know exactly how many tests failed, and which tests failed.

Your tests won't be stopped by failed `assert` statements, but it will stop if you have syntax errors.



### Test Driven Development and Data Science

- **TEST DRIVEN DEVELOPMENT:** writing tests before you write the code that’s being tested. Your test would fail at first, and you’ll know you’ve finished implementing a task when this test passes.
- Tests can check for all the different scenarios and edge cases you can think of, before even starting to write your function. This way, when you do start implementing your function, you can run this test to get immediate feedback on whether it works or not in all the ways you can think of, as you tweak your function.
- When refactoring or adding to your code, tests help you rest assured that the rest of your code didn't break while you were making those changes. Tests also helps ensure that your function behavior is repeatable, regardless of external parameters, such as hardware and time.

Test driven development for data science is relatively new and has a lot of experimentation and breakthroughs appearing, which you can learn more about in the resources below.

- [Data Science TDD](https://www.linkedin.com/pulse/data-science-test-driven-development-sam-savage/)
- [TDD for Data Science](http://engineering.pivotal.io/post/test-driven-development-for-data-science/)
- [TDD is Essential for Good Data Science Here's Why](https://medium.com/@karijdempsey/test-driven-development-is-essential-for-good-data-science-heres-why-db7975a03a44)
- [Testing Your Code](http://docs.python-guide.org/en/latest/writing/tests/) (general python TDD)



### Code Review

Code reviews benefit everyone in a team to promote best programming practices and prepare code for production. Let's go over what to look for in a code review and some tips on how to conduct one.

- [Code Review](https://github.com/lyst/MakingLyst/tree/master/code-reviews)
- [Code Review Best Practices](https://www.kevinlondon.com/2015/05/05/code-review-best-practices.html)

Questions to Ask Yourself When Conducting a Code Review

First, let's look over some of the questions we may ask ourselves while reviewing code. These are simply from the concepts we've covered in these last two lessons!

> #### Is the code clean and modular?
>
> - Can I understand the code easily?
> - Does it use meaningful names and whitespace?
> - Is there duplicated code?
> - Can you provide another layer of abstraction?
> - Is each function and module necessary?
> - Is each function or module too long?

> #### Is the code efficient?
>
> - Are there loops or other steps we can vectorize?
> - Can we use better data structures to optimize any steps?
> - Can we shorten the number of calculations needed for any steps?
> - Can we use generators or multiprocessing to optimize any steps?

> #### Is documentation effective?
>
> - Are in-line comments concise and meaningful?
> - Is there complex code that's missing documentation?
> - Do function use effective docstrings?
> - Is the necessary project documentation provided?

> #### Is the code well tested?
>
> - Does the code high test coverage?
> - Do tests check for interesting cases?
> - Are the tests readable?
> - Can the tests be made more efficient?

> #### Is the logging effective?
>
> - Are log messages clear, concise, and professional?
> - Do they include all relevant and useful information?
> - Do they use the appropriate logging level?

### Tips for Conducting a Code Review

Now that we know what we are looking for, let's go over some tips on how to actually write your code review. When your coworker finishes up some code that they want to merge to the team's code base, they might send it to you for review. You provide feedback and suggestions, and then they may make changes and send it back to you. When you are happy with the code, you approve and it gets merged to the team's code base.

As you may have noticed, with code reviews you are now dealing with people, not just computers. So it's important to be thoughtful of their ideas and efforts. You are in a team and there will be differences in preferences. The goal of code review isn't to make all code follow your personal preferences, but a standard of quality for the whole team.

> #### Tip: Use a code linter

This isn't really a tip for code review, but can save you lots of time from code review! Using a Python code linter like [pylint](https://www.pylint.org/) can automatically check for coding standards and PEP 8 guidelines for you! It's also a good idea to agree on a style guide as a team to handle disagreements on code style, whether that's an existing style guide or one you create together incrementally as a team.

> #### Tip: Explain issues and make suggestions

Rather than commanding people to change their code a specific way because it's better, it will go a long way to explain to them the consequences of the current code and *suggest* changes to improve it. They will be much more receptive to your feedback if they understand your thought process and are accepting recommendations, rather than following commands. They also may have done it a certain way intentionally, and framing it as a suggestion promotes a constructive discussion, rather than opposition.

```txt
BAD: Make model evaluation code its own module - too repetitive.

BETTER: Make the model evaluation code its own module. This will simplify models.py to be less repetitive and focus primarily on building models.

GOOD: How about we consider making the model evaluation code its own module? This would simplify models.py to only include code for building models. Organizing these evaluations methods into separate functions would also allow us to reuse them with different models without repeating code.
```

> #### Tip: Keep your comments objective

Try to avoid using the words "I" and "you" in your comments. You want to avoid comments that sound personal to bring the attention of the review to the code and not to themselves.

```txt
BAD: I wouldn't groupby genre twice like you did here... Just compute it once and use that for your aggregations.

BAD: You create this groupby dataframe twice here. Just compute it once, save it as groupby_genre and then use that to get your average prices and views.

GOOD: Can we group by genre at the beginning of the function and then save that as a groupby object? We could then reference that object to get the average prices and views without computing groupby twice.
```

> #### Tip: Provide code examples

When providing a code review, you can save the author time and make it easy for them to act on your feedback by writing out your code suggestions. This shows you are willing to spend some extra time to review their code and help them out. It can also just be much quicker for you to demonstrate concepts through code rather than explanations.

Let's say you were reviewing code that included the following lines:

```python
first_names = []
last_names = []

for name in enumerate(df.name):
    first, last = name.split(' ')
    first_names.append(first)
    last_names.append(last)

df['first_name'] = first_names
df['last_names'] = last_names
BAD: You can do this all in one step by using the pandas str.split method.
GOOD: We can actually simplify this step to the line below using the pandas str.split method. Found this on this stack overflow post: https://stackoverflow.com/questions/14745022/how-to-split-a-column-into-two-columns
df['first_name'], df['last_name'] = df['name'].str.split(' ', 1).str
```