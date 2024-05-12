# Permutation

In the recent challenge, the Assignment problem was discussed, where we aim to assign n tasks to n individuals in a way that minimizes the cost. Writing the code for this using the Hungarian method in Excel was a significant challenge, which is why I attempted to examine all possible scenarios in this allocation and select the one with the lowest cost among them. To do this, I needed to calculate n permutations.

[Assignment Challenge](https://www.linkedin.com/posts/omid-motamedisedeh-74aba166_excelchallenge-powerquerychllenge-excel-activity-7194803393637335040-lpzE?utm_source=share&utm_medium=member_desktop)

Since such a function does not exist in Power Query, I created a custom function for this purpose. It takes one input and generates the different permutation scenarios using the magical function List.Accumulate.



```powerquery-m
(n)=>

let
    T = Table.FromColumns(List.Repeat({{{1..n}}},n)),
    N = Table.ColumnNames(T),
    Comb = List.Accumulate(N,T, (x,y)=>Table.ExpandListColumn(x, y)),
    Filtered = Table.SelectRows(Table.CombineColumns(Comb,N,each _,"M"),each List.IsDistinct([M]))
in
    Filtered
```
