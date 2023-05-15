# Scopespace

> ### A new design pattern for working with data in a notebook environment.

    pip install scopespace


## Quickstart: Learn by example

`ScopeSpace` is a context manager whose `with` block has its own local scope.
```python
x = 5
with ScopeSpace() as foo:
    x = x + 1
    
print(x)  # 5
print(foo.x) # 6
```
Notice we did **not** overwrite `x` globally. Instead, we have `foo`, a namespace with it's _own_ version of `x`:

Anything we declare within a `ScopeSpace` is isolated within our chosen namespace.

```python
with ScopeSpace() as bar:
    stuff = 10
    
print(stuff)  # NameError: name 'stuff' is not defined
print(bar.stuff)  # 10
```

# What's the point?

### A common challenge for data people
In the world of dataframes and notebook environments, *naming* things can be tough. You're
often juggling multiple references to the same underlying data. Describing one of those with a variable name is
difficult. And you're often forced to *version* your data by changing its name across cells, to make
each cell idempotent. This confuses the reader.

There's a conflict of interest here:
- While the following may be true ...
  1. You want to use descriptive names.
  2. You want to incorporate versioning/renaming so cells can be run repeatedly.
- There are a few big problems with this:
  1. Renaming your data 10 times can make your code seem far more complex than it really is.
  2. In some libraries, like Pandas, you're **strongly** incentivized, naturally, to use *short* names, since names
     are often used repeatedly in the same command, to access columns.

#### A Typical Example

You've just created a new notebook, and have quickly jotted down some code to perform a few manipulations on a Pandas
dataframe, displaying the data at each stage.

<img width="450" alt="image" src="https://github.com/ryayoung/scopespace/assets/90723578/cc6860a7-093c-4169-b3da-6ff38e948eeb">

2 problems arise from this code:
1. The **second** and **third** cells will **error** if we try to run either of them twice in a row.
2. The name, `df` is not very descriptive.

So we switch to more descriptive names, and version them across cells.

<img width="700" alt="image" src="https://github.com/ryayoung/scopespace/assets/90723578/5b1e85f1-85da-4a5f-981c-7b693e7b10e3">

Now we've created a few more problems:
1. Our code is verbose and redundant. The **second line** in the **third cell** now takes **92** characters
   (the *name* of the dataframe occupies **57** of them), just to express `c = (c + b).astype(str)`.
2. It's *not* clear to the reader that **we are only working with one dataset**.
3. We've created room for mistakes.

A common solution is to use **functions** with descriptive names and a `df` parameter. This may present its own issues though:
1. Introduces several unnecessary steps: 1. Declare function signature. 2. Return something. 3. Call it, and pass it arguments.
2. We never intended to use it anywhere except _immediately_ after declaration. Thus, its purpose is unclear to the reader.

# ScopeSpace: An alternative

<img width="425" alt="image" src="https://github.com/ryayoung/scopespace/assets/90723578/2dcef8c5-5f71-4e8b-bd83-ff2034a3878c">

> _"Namespaces are one honking great idea -- let's do more of those!"_ - Tim Peters, 'The Zen of Python'

Here, we took the idea of a *notebook cell* and gave it a logical structure - an improvement in both form, and function.

By isolating logical tasks to their own scopes:
1. Each cell is **expressive** of its own purpose; the namespace labels are explicitly declared *first*
2. We made our naming style **consistent**, with an **obvious, repeatable pattern**
3. The code is more **concise**, but *without* sacrificing details
4. We **eliminated redundancy**: Descriptive names (the namespace labels) no longer clutter our logic when working with the data.

By grouping similar tasks together under a common namespace:
1. We get the logical grouping benefit that **functions** provide, but *without* needing to define or call one.
2. We get the attribute organization benefit that **class instances** provide, but *without* having to define our own class.






