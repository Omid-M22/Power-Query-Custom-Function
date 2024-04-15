# Tips for Defining Custom Functions in Power Query

Despite the wide variety of functions available in Power Query, sometimes it is necessary to define new custom functions for specific needs, especially in complex data cleansing processes. Online training enables you to become familiar with how to create functions that precisely match your specific conditions and requirements. Defining these specialized functions helps you to clean and prepare your data in a much more accurate and flexible manner, facilitating the optimization of your workflows.


## Agenda:
### Simple Custom functions
### Optional Parameters
### Parameters Type
### Recursive Functions
### Define Custom Functions as a step of Query
### Manage Custom funinctins
### Documentation in Custom Functions
___


```powerquery-m
() => "Hello, world"
```


```powerquery-m
(income) => 0.1*income
```



```powerquery-m
(income,tax_rate) => income*tax_rate
```


```powerquery-m
(income,optional tax_rate) => if  tax_rate=null then 0.1*income else income*tax_rate
```



