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

 

Staff Info
| Staff ID | Name | Income |
|:-- | :-- | :-- |
| S-081 | David R| 12500|
| S-210 | John K| 120000 |
| S-006 | Sara B | 44500 |
| S-012 | Robin M | 35100 |
| S-510 | BO X | 27500 |
| S-423 | Xhang X| 18000 |

```powerquery-m
= Table.FromRows({{"S-081","David R",12500},{"S-210","John K",120000},{"S-006","Sara B",44500},{"S-012","Robin M",35100},{"S-510","BO X",27500},{"S-423","Xhang X",18000}},
    {"Staff ID", "Name", "Income"})
```


Tax Rates
| From | To | Tax Rate |
|:-- | :-- | :-- |
| 0 | 30000 | 0 |
| 30000 | 85000 | 10% |
| 85000 | 100000 | 20% |
| 100000 | .... | 20% |


```powerquery-m
= Table.FromRows({{0,30000,0},{30000,85000,0.1},{85000,100000,0.2},{100000,10000000,0.3}},
    {"From", "To", "Tax Rate"})
```


Minerals Tax



### Define Custom Functions as a step of Query

```powerquery-m
let
    Source = Table.FromRows({{"S-081","David R",12500},{"S-210","John K",120000},{"S-006","Sara B",44500},{"S-012","Robin M",35100},{"S-510","BO X",27500},{"S-423","Xhang X",18000}},
    {"Staff ID", "Name", "Income"}),
    Tax=(income) => 0.1*income,
    Result=Table.AddColumn(Source,"Tax",each Tax(_[Income]))

in
    Result
```

[More Info](https://www.linkedin.com/posts/omid-motamedisedeh-74aba166_excelchallenge-powerquerychllenge-excel-activity-7178873434918019072-8EHc?utm_source=share&utm_medium=member_desktop)
    


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

Save the below code in a text file namely Tax and save it in C:\Functions\
```powerquery-m
(income,optional tax_rate) => if  tax_rate=null then 0.1*income else income*tax_rate
```

Use the below code to extract this file and convert it to a function.
```powerquery-m
let
    Source = Csv.Document(File.Contents("C:\Functions\TAX.txt"),[Delimiter="#(tab)"]),
    Text = Source{0}[Column1],
    Result =Expression.Evaluate(Text,#shared)
in
    Result
```    


[More Description](https://learn.microsoft.com/en-us/powerquery-m/expression-evaluate)




## Documentation in Custom Functions


```powerquery-m
let
  Tax = (a) => a * 0.1, 
  z   = [Documentation.Name = "This function can be used to calculate the tax value"]
in
  Value.ReplaceType(Tax, Value.ReplaceMetadata(Value.Type(Tax), z))
```


```powerquery-m
=  Value.Type(List.Sum )
```


```powerquery-m
= Value.Metadata( Value.Type(List.Sum) )
```



| Value | Detail | 
| :--- | :--- |
| Documentation.Name | Text to display across the top of the function invocation dialog, like "List.Sum" |
| Documentation.Description |  General info about what function do like "Returns the sum of the items in the list."|
| Documentation.LongDescription |  Description of what function do like "Returns the sum of the non-null values in the list, <code>list</code>. Returns null if there are no non-null values in the list."|
|Documentation.Category	| cattegory of function like "List.Addition" |
| Documentation.Examples |Example of function applications in the format of list of records with the filds of [Description, Code, Result] |




(Formating the code)[https://www.powerqueryformatter.com/]
