# R notes

### Pre requisites
```R
library(nycflights13)
library(tidyverse)
```

### NA
NA represents an unknown value so missing values are "_contagious_": almost any operation involving an unknown value will also be unknown.

```R
NA ^ 0 # 1
NA | T # TRUE
F & NA # FALSE
T & NA # NA
NA * 0 # NA
```
### Points to remember:
- R either prints out the results, or saves them to a variable. If you want to do both, you can wrap the assignment in parentheses
- **Tibbles** are data frames, but slightly tweaked to work better in the `tidyverse`
- dplyr functions never modify their inputs
- R either prints out the results, or saves them to a variable. If you want to do both, you can wrap the assignment in parentheses. i.e. `(d <- c(1:10))`

### Data types
 - `int` stands for integers.
 - `dbl` stands for doubles, or real numbers.
 - `chr` stands for character vectors, or strings.
 - `dttm` stands for date-times (a date + a time).
 - `lgl` stands for logical, vectors that contain only TRUE or FALSE.
 - `fctr` stands for factors, which R uses to represent categorical variables with fixed possible values.
 - `date` stands for dates. 

### Data manipulation functions
- #### `filter()` // WHERE
  - Syntax : `filter(df, condition_1, condition_2, ...)`

  - Examples : 
    ```R
    filter(flights, month == 1, day == 1)
    perfect_flights <- filter(flights, dep_delay == 0, arr_delay == 0)
    filter(flights, origin %in% c("JFK", "EWR"))
    filter(flights, origin == "JFK" | origin == "EWR"))
    ```

  - **Chaining conditions**: ![Chaining](http://r4ds.had.co.nz/diagrams/transform-logical.png)
- #### `arrange()` // ORDER BY
  - If you provide more than one column name, each additional column will be used to break ties in the values of preceding columns
  - Missing values are always sorted at the end
  - Syntax : `arrange(df, field_1, field_2, ...)`
  - Examples : 
    ```R
    arrange(flights, year, month, day)
    ```
  - **desc()**:
    ```R
    arrange(flights, desc(arr_delay), dep_delay)
    ```
- #### `select()` // SELECT
  - for selecting columns from a tibble
  - Syntax: `select(df, col1, col2, col5, ...)`
  - Example:
    ```R
    select(df, col1:col4) # from col1 to col4 (inclusive)
    select(df, -(col1:col4)) # all columns except from 1 to 4
    ```
  - **Helper Functions**:
    - They by default ignores case. to make them case sensitive: `ignore.case = FALSE`
    - `starts_with("abc")`: matches names that begin with “abc”.
    - `ends_with("xyz")`: matches names that end with “xyz”.
    - `contains("ijk")`: matches names that contain “ijk”.
    - `matches("(.)\\1")`: selects variables that match a regular expression. This one matches any variables that contain repeated characters.
    - `num_range("x", 1:3)`  matches  `x1`,  `x2`  and  `x3`.
    - `one_of(c("col1", "col2", ...))`: Returns all the columns that are present
  - **Re organizing columns using `everything()`**:
    `select(df, col3, col4, everything()) # new order: col3, col4, col1, col2, ...`

- #### `mutate()` // UPDATE
  - always adds new columns at the end of your dataset
  - Syntax: `mutate(df, new_col = f(old_col), ...)`
  - Example: `a quick brown fox jyumps over the lazy dof`
- #### `summarise()` // GROUP BY
- #### `group_by()` // ???
- #### `near()`
  ```R
  > near(2, 1.9)
  [1] FALSE
  > near(2, 1.999999999999999)
  [1] TRUE
  ```
- #### `between()`
  ```R
  > between(-4, -5, -1)
  [1] TRUE
  ```
- #### `is.na()`
  ```R
  filter(flights, is.na(dep_time)) # Extract
  filter(flights, !is.na(dep_time)) # Filter
  ```
- #### `rename()` // ALTER

  - Update Column names
  - `rename(df, old_name=new_name)`

**Q** Push all the rows having NA in a specific column to top  
**A** `flights %>% arrange(!is.na(dep_time))`

**Q** Arrange tibble by desc value of a column using `min_rank()`  
**A** `flights %>% arrange(desc(min_rank(arr_delay)))`

**Q** In `rank(a, ties.method="XXX")`, What are different ties.method params and their behavior?  
**A** Ties.method specifies the method rank uses to break ties. Suppose you have a vector `c(1,2,3,3,4,5)`. It's obvious that 1 is first, and 2 is second. However, it's not clear what ranks should be assigned to the first and second 3s. Ties.method determines how this is done. There are a few options:
  - `average` assigns each tied element the "average" rank. The ranks would therefore be 1, 2, 3.5, 3.5, 5, 6
  - `first` lets the "earlier" entry "win", so the ranks are in numerical order `(1,2,3,4,5,6)`
  - `min` assigns every tied element to the lowest rank, so you get `(1,2,3,3,5,6)`
  - `max` does the opposite: tied elements get the highest rank `(1,2,4,4,5,6)`
  - `random` breaks ties randomly, so you'd get either `(1,2,3,4,5,6)` or `(1,2,4,3,5,6)`.
  - __Reference__: [stackoverflow](https://stats.stackexchange.com/a/34010)