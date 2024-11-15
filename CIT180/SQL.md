Data in a SQL table is stored in a row that contains exactly one #tuple . A table is made up of one or more columns that independently describe the name, data type and attribute of the tuples.

The structure of the table, also known as #relation_schema, is defined by the attributes, or columns of the table.

The type of information that can be stored in a table is defined by the data types of the attributes at table creation time.

In lament terms, this means that SQL uses the following terms respectively:
- #table = #relation
- #row = #tuple
- #column = #attribute

### Oracle Data Types

In an Oracle database, the following data types are valid
- #char __char__(_n_) - Fixed length character string
	- Max size is 255 bytes (2000 in newer versions)
	- String of type char is always padded on right with blanks to a full length of _n_.
- #varchar2 __varchar2__(_n_) - Variable length character string
	- Max size is 2000 (4000 in newer versions)
- #number __number__(_0_, _d_) - Self-explanatory
	- _o_ = overall numbers of digits
	- _d_ = number of digits to the right of the decimal point
		- `number(5, 2)` cannot contain anything larger than `999.99`
- #date __date__ - Default format is DD-MM-YYYY
- #long __long__ - Character data up to a length of _2GB_
- #boolean __boolean__ - do not exist but can be simulated using `char` or `number` with a value of 1.


### SQL Queries

A basic SQL query looks like the following:
```SQL
select [distinct] <column(s)>
from <table>
[ where <condition> ]
[ order by <column(s) [asc|desc]> ]
```

#select allows you to select a column from a table. This operation is also known as _projection_.

 You can either `select` a column:
  ```sql
  select LOC, DEPTNO from DEPT;
```

 Or you can `select` all tuples with columns from the table:
 ```sql
 select ∗ from EMP;
```

`select` can also contain arithmetic expressions:
```sql
select ENAME, DEPTNO, SAL ∗ 1.55 from EMP;
```

##### Operators and Functions Provided by Oracle
- for numbers: abs, cos, sin, exp, log, power, mod, sqrt, +, −, ∗, /, . . .
- for strings: chr, concat(string1, string2), lower, upper, replace(string, search string, replacement string), translate, substr(string, m, n), length, to date, . . .
- for the date data type: add month, month between, next day, to char, . . 