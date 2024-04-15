# Tips for Defining Custom Functions in Power Query

Despite the wide variety of functions available in Power Query, sometimes it is necessary to define new custom functions for specific needs, especially in complex data cleansing processes. Such as request of calculation crolation between variables in the cleaning process of below challenge.

[Crolation of Respondants](https://www.linkedin.com/posts/omid-motamedisedeh-74aba166_excelchallenge-powerquerychllenge-excel-activity-7182482203040256003-YKtZ?utm_source=share&utm_medium=member_desktop)

## Agenda:
### Custom functions
### Optional Parameters
### Parameters Type
### Recursive Functions
### Define Custom Functions as a step of Query
### Manage Custom funinctins
### Documentation in Custom Functions
___

### Custom functions
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




## Optional Parameters
```powerquery-m
(income,optional tax_rate) => if  tax_rate=null then 0.1*income else income*tax_rate
```

Minerals Tax


| From | To | Tax Rate |
|:-- | :-- | :-- |
| 0 | 30000 | 0 |
| 30000 | 85000 | 10% |
| 85000 | 10000000 | 20% |





