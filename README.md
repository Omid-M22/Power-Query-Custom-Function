# Tips for Defining Custom Functions in Power Query

Despite the wide variety of functions available in Power Query, sometimes it is necessary to define new custom functions for specific needs, especially in complex data cleansing processes. Such as lack of List.Larg, List.Small, Vlookup (Approximate Match), List.Corolation.

Example:
[Crolation of Respondants](https://www.linkedin.com/posts/omid-motamedisedeh-74aba166_excelchallenge-powerquerychllenge-excel-activity-7182482203040256003-YKtZ?utm_source=share&utm_medium=member_desktop)
[Vlookup](https://www.linkedin.com/posts/crispo-mwangi-6ab49453_excel-excelchallenge-crispexcel-activity-7180081447607672832-lTqu?utm_source=share&utm_medium=member_desktop)


## Agenda:
### Basic Custom functions
### Parameters & Output Type
### Optional Parameters
### Advanced Custom Functions
### Define Custom Functions as a step of Query
### Recursive Functions
### Manage Custom funinctins by Expression.Evaluate
### Documentation in Custom Functions
___

### Basic Custom functions
Custome Function without any input parameter:
```powerquery-m
() => "Hello, world"
```

Custom function with an input parameter: 
```powerquery-m
(income) => 0.1*income
```
Custom function with two input parameters: 
```powerquery-m
(income,tax_rate) => income*tax_rate
```

using space in thename of parameters
```powerquery-m
(income,#"tax rate") => income*#"tax rate"
```

## Parameters & output Type

Implicit parameter
```powerquery-m
(a) =>Text.Start(a,3)
```

Explicit parameter
```powerquery-m
(a as text) =>Text.Start(a,3)
```
Explicit return & parameter
```powerquery-m
(a as text) as text =>Text.Start(a,3)
```



## Optional Parameters
```powerquery-m
(income,optional tax_rate) => if  tax_rate=null then 0.1*income else income*tax_rate
```




## Advanced Custom Functions

Vlookup


```powerquery-m
= Table.FromRows({{"S-081","David R",12500},{"S-210","John K",120000},{"S-006","Sara B",44500},{"S-012","Robin M",35100},{"S-510","BO X",27500},{"S-423","Xhang X",18000}},
    {"Staff ID", "Name", "Income"})
```

    

Staff Info
| Staff ID | Name | Income |
|:-- | :-- | :-- |
| S-081 | David R| 12500|
| S-210 | John K| 120000 |
| S-006 | Sara B | 44500 |
| S-012 | Robin M | 35100 |
| S-510 | BO X | 27500 |
| S-423 | Xhang X| 18000 |


Tax Rates
| From | To | Tax Rate |
|:-- | :-- | :-- |
| 0 | 30000 | 0 |
| 30000 | 85000 | 10% |
| 85000 | 10000000 | 20% |

```powerquery-m
= Table.FromRows({{0,30000,0},{30000,85000,0.1},{85000,10000000,0.2}},
    {"From", "To", "Tax Rate"})
```


Minerals Tax


### Define Custom Functions as a step of Query


### Recursive Functions



### Manage Custom funinctins by Expression.Evaluate


```powerquery-m
=Expression.Evaluate("5+6")
```

```powerquery-m
 =Expression.Evaluate("List.Sum({1..6})")
```

```powerquery-m
 =Expression.Evaluate("List.Sum({1..6})",#shared)
```


[More Description](https://learn.microsoft.com/en-us/powerquery-m/expression-evaluate)





