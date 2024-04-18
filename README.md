# Tips for Defining Custom Functions in Power Query

Despite the wide variety of functions available in Power Query, sometimes it is necessary to define new custom functions for specific needs, especially in complex data cleansing processes. Such as lack of List.Large, List.Small, Vlookup (Approximate Match), List.Corolation.

Example:

[Crolation of Respondants](https://www.linkedin.com/posts/omid-motamedisedeh-74aba166_excelchallenge-powerquerychllenge-excel-activity-7182482203040256003-YKtZ?utm_source=share&utm_medium=member_desktop)

[Vlookup](https://www.linkedin.com/posts/crispo-mwangi-6ab49453_excel-excelchallenge-crispexcel-activity-7180081447607672832-lTqu?utm_source=share&utm_medium=member_desktop)

[Convert Number To Text](https://www.linkedin.com/posts/omid-motamedisedeh-74aba166_excelchallenge-powerquerychllenge-excel-activity-7174524765414588416-YurR?utm_source=share&utm_medium=member_desktop)

You need to define new function in some functions like List.Generate

[List.Generate](https://learn.microsoft.com/en-us/powerquery-m/list-generate)


## Agenda:

### Basic Custom functions
### Parameters & Output Type
### Optional Parameters
### VLOOKUP
### Define Custom Functions as a step of Query
### Recursive Functions
### Manage Custom functions by Expression.Evaluate
### Documentation in Custom Functions
### Advanced Custom Functions
___

### Basic Custom functions
Custom Function without any input parameter:
```powerquery-m
() => "Hello, world"
```

Custom function with an input parameter: 
```powerquery-m
(income) => 0.1*income
```
or using each experision as below:
```powerquery-m
each 0.1*_
```

Custom function with two (more than one) input parameters: 
```powerquery-m
(income,tax_rate) => income*tax_rate
```

Using space in the name of parameters:
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
(income as number, optional tax_rate as number) as number =>
  if tax_rate = null then 0.1 * income else income * tax_rate
```




## Vlookup

 

Staff_Info
| Staff ID | Name | Income |
|:-- | :-- | :-- |
| S-081 | David R| 12500|
| S-210 | John K| 120000 |
| S-006 | Sara B | 44500 |
| S-012 | Robin M | 35100 |
| S-510 | BO X | 27500 |
| S-423 | Xhang X| 18000 |

```powerquery-m
=Table.FromRows(
  {
    {"S-081", "David R", 12500}, 
    {"S-210", "John K", 120000}, 
    {"S-006", "Sara B", 44500}, 
    {"S-012", "Robin M", 35100}, 
    {"S-510", "BO X", 27500}, 
    {"S-423", "Xhang X", 18000}
  }, 
  {"Staff ID", "Name", "Income"}
)
```


Tax_Rates
| From | To | Tax Rate |
|:-- | :-- | :-- |
| 0 | 30000 | 0 |
| 30000 | 85000 | 10% |
| 85000 | 100000 | 20% |
| 100000 | .... | 20% |


```powerquery-m
=Table.FromRows(
  {{0, 30000, 0}, {30000, 85000, 0.1}, {85000, 100000, 0.2}, {100000, 10000000, 0.3}}, 
  {"From", "To", "Tax Rate"}
)
```

To calculate the tax rate, based on the tabl, below function can be defined.
```powerquery-m
(income,Tax)=> List.Last(Table.SelectRows(Tax, each [From] <= income)[Tax Rate])
```


Minerals Tax



### Define Custom Functions as a step of Query

```powerquery-m
let
  Source = Table.FromRows(
    {
      {"S-081", "David R", 12500}, 
      {"S-210", "John K", 120000}, 
      {"S-006", "Sara B", 44500}, 
      {"S-012", "Robin M", 35100}, 
      {"S-510", "BO X", 27500}, 
      {"S-423", "Xhang X", 18000}
    }, 
    {"Staff ID", "Name", "Income"}
  ), 
  Tax = (income) => 0.1 * income, 
  Result = Table.AddColumn(Source, "Tax", each Tax(_[Income]))
in
  Result
```

Deinfe multi line functins inside the steps of query
```powerquery-m
let
  Source = Table.FromRows(
    {
      {"S-081", "David R", 12500}, 
      {"S-210", "John K", 120000}, 
      {"S-006", "Sara B", 44500}, 
      {"S-012", "Robin M", 35100}, 
      {"S-510", "BO X", 27500}, 
      {"S-423", "Xhang X", 18000}
    }, 
    {"Staff ID", "Name", "Income"}
  ), 
  TaxFX = (income, Tax) =>
    let
      a = Table.SelectRows(Tax, each [From] <= income), 
      b = a[Tax Rate], 
      c = List.Last(b)
    in
      c, 
  Result = Table.AddColumn(Source, "Tax", each TaxFX([Income], Tax_Rates) * [Income])
in
  Result
```

### Recursive Functions

Find the next prime number

first check a value is prime by the below IsPrime Function
```powerquery-m
(a) =>
  let
    List   = {1 .. Number.IntegerDivide(a, 2)}, 
    Mode   = List.Transform(List, each Number.Mod(a, _)), 
    Select = List.Select(Mode, each _ = 0), 
    Count  = List.Count(Select) = 1
  in
    Count
```

then use the below recursive function to find the next prime number by the below function namely NextPrimeNumber
```powerquery-m
(a)=> if IsPrime(a) then a else NextPrimeNumber(a+1)
```

Both functions can be writien in a query as below:
```powerquery-m
(x) =>
  let
    IsPrime = (a) =>
      let
        List   = {1 .. Number.IntegerDivide(a, 2)}, 
        Mode   = List.Transform(List, each Number.Mod(a, _)), 
        Select = List.Select(Mode, each _ = 0), 
        Count  = List.Count(Select) = 1
      in
        Count, 
    b = if IsPrime(x) then x else NextPrimeNumber(x + 1)
  in
    b
```


[More Info](https://www.linkedin.com/posts/omid-motamedisedeh-74aba166_excelchallenge-powerquerychllenge-excel-activity-7178873434918019072-8EHc?utm_source=share&utm_medium=member_desktop)
    






### Manage Custom functions by Expression.Evaluate


```powerquery-m
=Expression.Evaluate("5+6")
```

Below code leads to error
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
  Source = Csv.Document(File.Contents("C:\Functions\TAX.txt"), [Delimiter = "#(tab)"]), 
  Text   = Source{0}[Column1], 
  Result = Expression.Evaluate(Text, #shared)
in
  Result
```    


[More Description](https://learn.microsoft.com/en-us/powerquery-m/expression-evaluate)




## Documentation in Custom Functions


```powerquery-m
let
  Tax = (a) => a * 0.1, 
  z   = [Documentation.Description = "This function can be used to calculate the tax value"]
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



```powerquery-m
let
  Tax = (a) => a * 0.1, 
  z = [
    Documentation.Name = "Tax", 
    Documentation.Description = "This function can be used to calculate the tax value", 
    Documentation.LongDescription = "In this function a fixed tax rate of 10% is used", 
    Documentation.Examples = {
      [Description = "Tax value for a person with income =500$", Code = "Tax(500)", Result = "50"], 
      [Description = "Tax value for a person with income =250$", Code = "Tax(250)", Result = "25"]
    }
  ]
in
  Value.ReplaceType(Tax, Value.ReplaceMetadata(Value.Type(Tax), z))
```
  

[Formating the code](https://www.powerqueryformatter.com/)




#Advance Custom Function

Custom function for converting number to text (Persian)
```powerquery-m
(B) =>
  let
    S1 = {
      {"یک","دو","سه","چهار","پنج","شش","هفت","هشت","نه","ده","یازده","دوازده","سیزده","چهارده","پانزده","شانزده","هفتده","هجده","نانزده"}, 
      {"بیست", "سی", "چهل", "پنجاه", "شصت", "هفتاد", "هشتاد", "نود"}, 
      {"صد", "دویست", "سیصد", "چهارصد", "پانصد", "شش صد", "هفت صد", "هشت صد", "نه صد"}
    }, 
    S2 = {"هزار", "میلیون", "میلیارد"}, 
    Z = List.Transform(List.Reverse(Text.ToList(Text.From(B))), Number.From), 
    Text1 = List.Transform(
      List.Positions(Z), 
      each (
        try
          
            if Number.Mod(_ + 1, 3) = 0 then
              S1{2}{Z{_} - 1}
            else if Number.Mod(_ + 1, 3) = 1 then
              (
                if List.Count(Z) > _ + 1 then
                  (if Z{_ + 1} = 1 then S1{0}{Z{_} + 10 - 1} else S1{0}{Z{_} - 1})
                else
                  S1{0}{Z{_} - 1}
              )
            else
              (if Z{_} = 1 then "" else S1{1}{Z{_} - 2})
        otherwise
          ""
      )
    ), 
    Text2 = List.Combine(
      List.Transform({{""}, {" هزار"}, {" میلیون"}, {" میلیارد"}}, each List.Repeat(_, 3))
    ), 
    Text3 = List.Transform(List.Positions(Text1), each if Text1{_} = "" then "" else Text2{_}), 
    Text = List.Transform(
      List.Positions(Text3), 
      each 
        if List.Count(List.Select(List.FirstN(Text3, _ + 1), (x) => x = Text3{_})) > 1 then
          Text1{_}
        else
          Text1{_} & Text3{_}
    ), 
    Combine = Text.Clean(Text.Combine(List.Reverse(List.Select(Text, each _ <> "")), " و "))
  in
    Combine
```


