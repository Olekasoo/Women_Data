# My Analysis


- [Quarto](#quarto)
- [Running Code](#running-code)

## Quarto

Quarto enables you to weave together content and executable code into a
finished document.

## Running Code

When you click the **Render** button a document will be generated that
includes both content and the output of embedded code. You can embed
code like this:

``` r
women
```

       height weight
    1      58    115
    2      59    117
    3      60    120
    4      61    123
    5      62    126
    6      63    129
    7      64    132
    8      65    135
    9      66    139
    10     67    142
    11     68    146
    12     69    150
    13     70    154
    14     71    159
    15     72    164

You can add options to executable code like this

``` r
str(women)
```

    'data.frame':   15 obs. of  2 variables:
     $ height: num  58 59 60 61 62 63 64 65 66 67 ...
     $ weight: num  115 117 120 123 126 129 132 135 139 142 ...

some Analysis of Women data.

``` r
cor(women$height,women$weight)
```

    [1] 0.9954948

``` r
plot(hist(women$height))
```

![](Output_files/figure-commonmark/unnamed-chunk-4-1.png)
