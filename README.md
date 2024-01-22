# μSQL

μSQL aims to be the result of splicing the SQL and `<insert generic unicode array-language here>` genomes and mixing them together.

Pros:
- Write functional code in yet another environment.
- Saves memory space by replacing long keywords and indentations with cute pictograms.
- Forces 80%+ brain usage to write any non-trivial query.
- Gain approval from APL bros in XMPP.
Cons:
- `null`

Eventually I want μSQL to consist of an executable binary that transpiles `.musql` files into `.sql`, as well as a Rust library to provide the conversion within Rust projects.

Besides operator remapping I will iteratively add any modifications to the language that I please in order for it to align with my own internal viewpoints and biases. So far we have:
- Selection arguments must be predefined columns
    - Any operation on columns must be applied before the select statement `→{}`
    - New columns are defined using the `table⊕{column}` syntax
    - New columns may be given names using the `{col_name: ...}` syntax or be referred to by creation order using the `$n` identifier

Here are experimental examples of the syntax:

```bash
$ query="employee↾{salary>50000}⊕{ranking:☇⇌{↧salary}}→{employee_id,last_name,salary,ranking}"
$ musql -i $query
SELECT 
    employee_id,
    last_name,
    first_name,
    salary,
    RANK() OVER (ORDER BY salary DESC) as ranking
FROM employee
WHERE salary > 50000
ORDER BY ranking
```

```bash
$ query="{product⊗box_sizes}⊕{grain.price_per_pound*box_size.box_weight}
            →{grain.product_name,box_size.description,§1}"
$ musql -i $query
SELECT
  grain.product_name,
  box_size.description,
  grain.price_per_pound * box_size.box_weight
FROM product
CROSS JOIN box_sizes
```

```bash
$ query="employee↾{⋄salary}→{*}"
$ musql -i $query
SELECT *
FROM employee 
WHERE salary > ( SELECT AVG(salary) FROM employee )
```

The main emphasis for now is on transpiling `SELECT` statements, but eventually I might also include modification statements and whatnot.

## Documentation

WIP ;)
