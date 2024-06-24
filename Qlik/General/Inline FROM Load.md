# Inline FROM Load

## Why?
If you want to load data via an Inline table and then do transformations on the data this can be done by making an Inline table and then doing a resident load from that table and making the transformations you want.

But, if you want to, you can make all steps into one by loading a table with transformations and loading the table data directly from an Inline table.

## Example of loading transformated data from an Inline table

```
Product:
Load
    Region & '|' & ProductId as %Region_Product,
    ProductId,
    Price
Inline [
    Region, ProductId, Price
    North,  A,         5
    East,   A,         5
    West,   A,         6
    North,  B,         13
    East,   B,         8
    West,   B,         9
    North,  C,         1
    East,   C,         1
    West,   C,         1
    North,  D,         21
    East,   D,         12
    West,   D,         7
];
```

## Output

![image](https://github.com/xGregoriusx/BI-in-practice/assets/139049888/3268fce9-db21-4ef7-a1d4-49f4c1979e0c)
