# My Analysis


- [START](#start)
- [Descriptive Statistics.](#descriptive-statistics)
- [](#section)
- [Predictions](#predictions)
- [Hypothesis Tests](#hypothesis-tests)
  - [**Descriptive Analysis with
    Visualizations**](#descriptive-analysis-with-visualizations)
  - [**Simple Predictive Models**](#simple-predictive-models)
  - [**Advanced Analysis**](#advanced-analysis)

## START

This is the beginning of descriptive analysis of the women dataset that
is available in R, it shall form part of a journey of a thousand miles:

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

# Descriptive Statistics.

# 

``` r
df <- data.frame(
  age = 58:72,
  height = c(115, 117, 120, 123, 126, 129, 132, 135, 
            139, 142, 146, 150, 154, 159, 164)
  )
df
```

       age height
    1   58    115
    2   59    117
    3   60    120
    4   61    123
    5   62    126
    6   63    129
    7   64    132
    8   65    135
    9   66    139
    10  67    142
    11  68    146
    12  69    150
    13  70    154
    14  71    159
    15  72    164

``` r
x <- women$height
y <- women$weight
```

``` r
plot(x,y)
```

![](Output_files/figure-commonmark/unnamed-chunk-5-1.png)

``` r
plot(x, y, type ='l')
```

![](Output_files/figure-commonmark/unnamed-chunk-6-1.png)

Barchart

``` r
barplot(y, names.arg = x)
```

![](Output_files/figure-commonmark/unnamed-chunk-7-1.png)

Density plot

``` r
plot(density(y))
```

![](Output_files/figure-commonmark/unnamed-chunk-8-1.png)

Q-Q plot

``` r
qqnorm(y); qqline(y)
```

![](Output_files/figure-commonmark/unnamed-chunk-9-1.png)

Residuals

``` r
plot(x, resid(lm(y ~ x)))
```

![](Output_files/figure-commonmark/unnamed-chunk-10-1.png)

ACF plot

``` r
acf(y)
```

![](Output_files/figure-commonmark/unnamed-chunk-11-1.png)

# Predictions

linear

``` r
predict(lm(y ~x), data.frame(x =73))
```

           1 
    164.3333 

``` r
predict(lm(y ~poly(x,2)), data.frame(x=73))
```

           1 
    168.0989 

``` r
#quad pred
```

Linear Interpolation

``` r
approx(x, y, xout = 73)$y
```

    [1] NA

Spline Interplolation

``` r
spline(x, y, xout = 73)$y
```

    [1] 167.8803

LOESS

``` r
#loess(y ~ x) %>% 
 # predict(data.frame(x=73))
```

``` r
library(randomForest)
```

    randomForest 4.7-1.2

    Type rfNews() to see new features/changes/bug fixes.

``` r
predict(randomForest(y ~x, ntree =50), data.frame(x =73))
```

           1 
    157.5343 

``` r
t.test(y, mu =140)
```


        One Sample t-test

    data:  y
    t = -0.81631, df = 14, p-value = 0.428
    alternative hypothesis: true mean is not equal to 140
    95 percent confidence interval:
     128.1504 145.3162
    sample estimates:
    mean of x 
     136.7333 

``` r
mean(y)
```

    [1] 136.7333

# Hypothesis Tests

``` r
t.test(y[1:7], y[8:15])
```


        Welch Two Sample t-test

    data:  y[1:7] and y[8:15]
    t = -5.9795, df = 11.872, p-value = 6.706e-05
    alternative hypothesis: true difference in means is not equal to 0
    95 percent confidence interval:
     -34.77840 -16.18588
    sample estimates:
    mean of x mean of y 
     123.1429  148.6250 

Test Equal Variances

``` r
var.test(y[1:7], y[8:15])
```


        F test to compare two variances

    data:  y[1:7] and y[8:15]
    F = 0.38927, num df = 6, denom df = 7, p-value = 0.2708
    alternative hypothesis: true ratio of variances is not equal to 1
    95 percent confidence interval:
     0.07605086 2.21709666
    sample estimates:
    ratio of variances 
             0.3892737 

Wilcoxon For Non-Parametric

``` r
wilcox.test(y[1:7], y[8:15])
```


        Wilcoxon rank sum exact test

    data:  y[1:7] and y[8:15]
    W = 0, p-value = 0.0003108
    alternative hypothesis: true location shift is not equal to 0

Normality Test

``` r
shapiro.test(y)
```


        Shapiro-Wilk normality test

    data:  y
    W = 0.96036, p-value = 0.6986

Distribution Test

``` r
ks.test(y,"pnorm", mean(y), sd(y))
```


        Exact one-sample Kolmogorov-Smirnov test

    data:  y
    D = 0.091099, p-value = 0.9984
    alternative hypothesis: two-sided

Chi-square Goodness of Fit

``` r
chisq.test(table(cut(y,3)))
```


        Chi-squared test for given probabilities

    data:  table(cut(y, 3))
    X-squared = 0.4, df = 2, p-value = 0.8187

``` r
library(dplyr)
```


    Attaching package: 'dplyr'

    The following object is masked from 'package:randomForest':

        combine

    The following objects are masked from 'package:stats':

        filter, lag

    The following objects are masked from 'package:base':

        intersect, setdiff, setequal, union

``` r
women %>% 
  lm(x ~ y, data =.) %>% 
  summary()
```


    Call:
    lm(formula = x ~ y, data = .)

    Residuals:
         Min       1Q   Median       3Q      Max 
    -0.83233 -0.26249  0.08314  0.34353  0.49790 

    Coefficients:
                 Estimate Std. Error t value Pr(>|t|)    
    (Intercept) 25.723456   1.043746   24.64 2.68e-12 ***
    y            0.287249   0.007588   37.85 1.09e-14 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 0.44 on 13 degrees of freedom
    Multiple R-squared:  0.991, Adjusted R-squared:  0.9903 
    F-statistic:  1433 on 1 and 13 DF,  p-value: 1.091e-14

F-Test For regression

``` r
summary(lm(x ~ y))$fstatistic
```

       value    numdf    dendf 
    1433.024    1.000   13.000 

Durbin-Watson Test

``` r
library(lmtest)
```

    Loading required package: zoo


    Attaching package: 'zoo'

    The following objects are masked from 'package:base':

        as.Date, as.Date.numeric

``` r
dwtest(lm(x ~y))
```


        Durbin-Watson test

    data:  lm(x ~ y)
    DW = 0.31156, p-value = 9.623e-08
    alternative hypothesis: true autocorrelation is greater than 0

``` r
cor(x,y)
```

    [1] 0.9954948

``` r
library(readr)
ai_job_replacement_2020_2026_v2 <- read_csv("D:/Desktop/ai_job_replacement_2020_2026_v2.csv")
```

    Rows: 15000 Columns: 14
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: ","
    chr  (3): job_role, industry, country
    dbl (11): year, automation_risk_percent, ai_replacement_score, skill_gap_ind...

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
str(ai_job_replacement_2020_2026_v2)
```

    spc_tbl_ [15,000 × 14] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
     $ job_role                   : chr [1:15000] "Data Analyst" "Accountant" "Teacher" "Customer Support Rep" ...
     $ industry                   : chr [1:15000] "Technology" "Finance" "Technology" "Technology" ...
     $ country                    : chr [1:15000] "Canada" "Brazil" "USA" "Brazil" ...
     $ year                       : num [1:15000] 2021 2020 2020 2021 2024 ...
     $ automation_risk_percent    : num [1:15000] 26.2 52.1 31.3 56.9 14.6 ...
     $ ai_replacement_score       : num [1:15000] 30.9 56.4 31.6 63.4 17.2 ...
     $ skill_gap_index            : num [1:15000] 73.2 2.06 43.19 19.97 96.56 ...
     $ salary_before_usd          : num [1:15000] 101839 146389 64948 91708 127008 ...
     $ salary_change_percent      : num [1:15000] -2.34 -4.69 -10.13 -5.44 -6.01 ...
     $ skill_demand_growth_percent: num [1:15000] 2.66 10.43 8.14 6.11 2.08 ...
     $ remote_feasibility_score   : num [1:15000] 15.2 26.4 36.3 64.7 71.6 ...
     $ ai_adoption_level          : num [1:15000] 86.6 18.3 36.6 17.1 44 ...
     $ education_requirement_level: num [1:15000] 2 5 2 5 3 3 1 3 4 5 ...
     $ reskilling_urgency_score   : num [1:15000] 33.1 22.9 28.5 30.4 36.6 ...
     - attr(*, "spec")=
      .. cols(
      ..   job_role = col_character(),
      ..   industry = col_character(),
      ..   country = col_character(),
      ..   year = col_double(),
      ..   automation_risk_percent = col_double(),
      ..   ai_replacement_score = col_double(),
      ..   skill_gap_index = col_double(),
      ..   salary_before_usd = col_double(),
      ..   salary_change_percent = col_double(),
      ..   skill_demand_growth_percent = col_double(),
      ..   remote_feasibility_score = col_double(),
      ..   ai_adoption_level = col_double(),
      ..   education_requirement_level = col_double(),
      ..   reskilling_urgency_score = col_double()
      .. )
     - attr(*, "problems")=<externalptr> 

`{View(ai_job_replacement_2020_2026_v2)}`

``` r
colSums(is.na(ai_job_replacement_2020_2026_v2))
```

                       job_role                    industry 
                              0                           0 
                        country                        year 
                              0                           0 
        automation_risk_percent        ai_replacement_score 
                              0                           0 
                skill_gap_index           salary_before_usd 
                              0                           0 
          salary_change_percent skill_demand_growth_percent 
                              0                           0 
       remote_feasibility_score           ai_adoption_level 
                              0                           0 
    education_requirement_level    reskilling_urgency_score 
                              0                           0 

``` r
names(ai_job_replacement_2020_2026_v2)
```

     [1] "job_role"                    "industry"                   
     [3] "country"                     "year"                       
     [5] "automation_risk_percent"     "ai_replacement_score"       
     [7] "skill_gap_index"             "salary_before_usd"          
     [9] "salary_change_percent"       "skill_demand_growth_percent"
    [11] "remote_feasibility_score"    "ai_adoption_level"          
    [13] "education_requirement_level" "reskilling_urgency_score"   

``` r
#install.packages(c("tidyverse", "skimr", "GGally", "corrplot"))

# Load packages
library(tidyverse)
```

    ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ✔ ggplot2   4.0.2     ✔ tibble    3.2.1
    ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ✔ purrr     1.0.4     
    ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ✖ dplyr::combine()  masks randomForest::combine()
    ✖ dplyr::filter()   masks stats::filter()
    ✖ dplyr::lag()      masks stats::lag()
    ✖ ggplot2::margin() masks randomForest::margin()
    ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(skimr)
library(GGally)
library(corrplot)
```

    corrplot 0.95 loaded

``` r
df <-ai_job_replacement_2020_2026_v2
df
```

    # A tibble: 15,000 × 14
       job_role   industry country  year automation_risk_perc…¹ ai_replacement_score
       <chr>      <chr>    <chr>   <dbl>                  <dbl>                <dbl>
     1 Data Anal… Technol… Canada   2021                  26.2                 30.9 
     2 Accountant Finance  Brazil   2020                  52.1                 56.4 
     3 Teacher    Technol… USA      2020                  31.3                 31.6 
     4 Customer … Technol… Brazil   2021                  56.9                 63.4 
     5 Teacher    Manufac… Japan    2024                  14.6                 17.2 
     6 Software … Healthc… Singap…  2022                   8.54                 8.53
     7 Accountant Manufac… Austra…  2020                  48.3                 42.2 
     8 Teacher    Finance  Austra…  2026                  13.2                 12.2 
     9 Marketing… Retail   USA      2025                  19.9                 22.3 
    10 Financial… Finance  Singap…  2020                  59.5                 64.9 
    # ℹ 14,990 more rows
    # ℹ abbreviated name: ¹​automation_risk_percent
    # ℹ 8 more variables: skill_gap_index <dbl>, salary_before_usd <dbl>,
    #   salary_change_percent <dbl>, skill_demand_growth_percent <dbl>,
    #   remote_feasibility_score <dbl>, ai_adoption_level <dbl>,
    #   education_requirement_level <dbl>, reskilling_urgency_score <dbl>

``` r
head(df)
```

    # A tibble: 6 × 14
      job_role    industry country  year automation_risk_perc…¹ ai_replacement_score
      <chr>       <chr>    <chr>   <dbl>                  <dbl>                <dbl>
    1 Data Analy… Technol… Canada   2021                  26.2                 30.9 
    2 Accountant  Finance  Brazil   2020                  52.1                 56.4 
    3 Teacher     Technol… USA      2020                  31.3                 31.6 
    4 Customer S… Technol… Brazil   2021                  56.9                 63.4 
    5 Teacher     Manufac… Japan    2024                  14.6                 17.2 
    6 Software E… Healthc… Singap…  2022                   8.54                 8.53
    # ℹ abbreviated name: ¹​automation_risk_percent
    # ℹ 8 more variables: skill_gap_index <dbl>, salary_before_usd <dbl>,
    #   salary_change_percent <dbl>, skill_demand_growth_percent <dbl>,
    #   remote_feasibility_score <dbl>, ai_adoption_level <dbl>,
    #   education_requirement_level <dbl>, reskilling_urgency_score <dbl>

``` r
dim(df)
```

    [1] 15000    14

``` r
summary(df)
```

       job_role           industry           country               year     
     Length:15000       Length:15000       Length:15000       Min.   :2020  
     Class :character   Class :character   Class :character   1st Qu.:2021  
     Mode  :character   Mode  :character   Mode  :character   Median :2023  
                                                              Mean   :2023  
                                                              3rd Qu.:2025  
                                                              Max.   :2026  
     automation_risk_percent ai_replacement_score skill_gap_index salary_before_usd
     Min.   : 5.00           Min.   :  4.01       Min.   : 0.00   Min.   : 30004   
     1st Qu.:28.79           1st Qu.: 28.36       1st Qu.:25.17   1st Qu.: 60127   
     Median :46.23           Median : 45.67       Median :49.93   Median : 89533   
     Mean   :46.18           Mean   : 46.16       Mean   :50.00   Mean   : 89771   
     3rd Qu.:63.60           3rd Qu.: 62.71       3rd Qu.:75.03   3rd Qu.:119824   
     Max.   :94.98           Max.   :113.07       Max.   :99.98   Max.   :149984   
     salary_change_percent skill_demand_growth_percent remote_feasibility_score
     Min.   :-38.3700      Min.   :-31.880             Min.   :10.01           
     1st Qu.: -6.6400      1st Qu.: -1.663             1st Qu.:32.52           
     Median :  0.1500      Median :  4.960             Median :54.77           
     Mean   :  0.1143      Mean   :  5.020             Mean   :54.90           
     3rd Qu.:  6.6900      3rd Qu.: 11.730             3rd Qu.:77.41           
     Max.   : 36.9200      Max.   : 49.790             Max.   :99.99           
     ai_adoption_level education_requirement_level reskilling_urgency_score
     Min.   : 0.01     Min.   :1.000               Min.   : 2.456          
     1st Qu.:24.71     1st Qu.:2.000               1st Qu.:26.982          
     Median :49.44     Median :3.000               Median :35.871          
     Mean   :49.80     Mean   :3.015               Mean   :35.868          
     3rd Qu.:74.80     3rd Qu.:4.000               3rd Qu.:44.699          
     Max.   :99.98     Max.   :5.000               Max.   :71.579          

``` r
# 3. Check for missing values
colSums(is.na(df))
```

                       job_role                    industry 
                              0                           0 
                        country                        year 
                              0                           0 
        automation_risk_percent        ai_replacement_score 
                              0                           0 
                skill_gap_index           salary_before_usd 
                              0                           0 
          salary_change_percent skill_demand_growth_percent 
                              0                           0 
       remote_feasibility_score           ai_adoption_level 
                              0                           0 
    education_requirement_level    reskilling_urgency_score 
                              0                           0 

``` r
# 4. Clean column names
library(tidyverse)
```

``` r
# 5. Convert categorical variables to factors
df$job_role <- as.factor(df$job_role)
df$industry <- as.factor(df$industry)
df$country <- as.factor(df$country)
df$education_requirement_level <- as.factor(df$education_requirement_level)
```

``` r
# 6. Check factor levels
levels(df$job_role)[1:20]
```

     [1] "Accountant"           "Customer Support Rep" "Data Analyst"        
     [4] "Financial Analyst"    "HR Manager"           "Marketing Specialist"
     [7] "Mechanical Engineer"  "Software Engineer"    "Teacher"             
    [10] "Truck Driver"         NA                     NA                    
    [13] NA                     NA                     NA                    
    [16] NA                     NA                     NA                    
    [19] NA                     NA                    

``` r
levels(df$industry)
```

    [1] "Education"      "Energy"         "Finance"        "Healthcare"    
    [5] "Manufacturing"  "Retail"         "Technology"     "Transportation"

``` r
levels(df$country)
```

    [1] "Australia" "Brazil"    "Canada"    "Germany"   "India"     "Japan"    
    [7] "Singapore" "UK"        "USA"      

``` r
levels(df$education_requirement_level)
```

    [1] "1" "2" "3" "4" "5"

``` r
str(df)
```

    spc_tbl_ [15,000 × 14] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
     $ job_role                   : Factor w/ 10 levels "Accountant","Customer Support Rep",..: 3 1 9 2 9 8 1 9 6 4 ...
     $ industry                   : Factor w/ 8 levels "Education","Energy",..: 7 3 7 7 5 4 5 3 6 3 ...
     $ country                    : Factor w/ 9 levels "Australia","Brazil",..: 3 2 9 2 6 7 1 1 9 7 ...
     $ year                       : num [1:15000] 2021 2020 2020 2021 2024 ...
     $ automation_risk_percent    : num [1:15000] 26.2 52.1 31.3 56.9 14.6 ...
     $ ai_replacement_score       : num [1:15000] 30.9 56.4 31.6 63.4 17.2 ...
     $ skill_gap_index            : num [1:15000] 73.2 2.06 43.19 19.97 96.56 ...
     $ salary_before_usd          : num [1:15000] 101839 146389 64948 91708 127008 ...
     $ salary_change_percent      : num [1:15000] -2.34 -4.69 -10.13 -5.44 -6.01 ...
     $ skill_demand_growth_percent: num [1:15000] 2.66 10.43 8.14 6.11 2.08 ...
     $ remote_feasibility_score   : num [1:15000] 15.2 26.4 36.3 64.7 71.6 ...
     $ ai_adoption_level          : num [1:15000] 86.6 18.3 36.6 17.1 44 ...
     $ education_requirement_level: Factor w/ 5 levels "1","2","3","4",..: 2 5 2 5 3 3 1 3 4 5 ...
     $ reskilling_urgency_score   : num [1:15000] 33.1 22.9 28.5 30.4 36.6 ...
     - attr(*, "spec")=
      .. cols(
      ..   job_role = col_character(),
      ..   industry = col_character(),
      ..   country = col_character(),
      ..   year = col_double(),
      ..   automation_risk_percent = col_double(),
      ..   ai_replacement_score = col_double(),
      ..   skill_gap_index = col_double(),
      ..   salary_before_usd = col_double(),
      ..   salary_change_percent = col_double(),
      ..   skill_demand_growth_percent = col_double(),
      ..   remote_feasibility_score = col_double(),
      ..   ai_adoption_level = col_double(),
      ..   education_requirement_level = col_double(),
      ..   reskilling_urgency_score = col_double()
      .. )
     - attr(*, "problems")=<externalptr> 

``` r
# 7. Create derived columns
df <- df %>%
  mutate(
    # Year categories
    year_category = case_when(
      year <= 2022 ~ "Past",
      year <= 2024 ~ "Present",
      TRUE ~ "Future"
    ),
    # Risk brackets
    risk_bracket = case_when(
      automation_risk_percent < 33 ~ "Low Risk",
      automation_risk_percent < 67 ~ "Medium Risk",
      TRUE ~ "High Risk"
    )
  )
```

``` r
# 8. Make them factors
df$year_category <- factor(df$year_category, levels = c("Past", "Present", "Future"))
df$risk_bracket <- factor(df$risk_bracket, levels = c("Low Risk", "Medium Risk", "High Risk"))
```

``` r
# 9. Quick summary of numerical columns
df %>%
  select(where(is.numeric)) %>%
  summary()
```

          year      automation_risk_percent ai_replacement_score skill_gap_index
     Min.   :2020   Min.   : 5.00           Min.   :  4.01       Min.   : 0.00  
     1st Qu.:2021   1st Qu.:28.79           1st Qu.: 28.36       1st Qu.:25.17  
     Median :2023   Median :46.23           Median : 45.67       Median :49.93  
     Mean   :2023   Mean   :46.18           Mean   : 46.16       Mean   :50.00  
     3rd Qu.:2025   3rd Qu.:63.60           3rd Qu.: 62.71       3rd Qu.:75.03  
     Max.   :2026   Max.   :94.98           Max.   :113.07       Max.   :99.98  
     salary_before_usd salary_change_percent skill_demand_growth_percent
     Min.   : 30004    Min.   :-38.3700      Min.   :-31.880            
     1st Qu.: 60127    1st Qu.: -6.6400      1st Qu.: -1.663            
     Median : 89533    Median :  0.1500      Median :  4.960            
     Mean   : 89771    Mean   :  0.1143      Mean   :  5.020            
     3rd Qu.:119824    3rd Qu.:  6.6900      3rd Qu.: 11.730            
     Max.   :149984    Max.   : 36.9200      Max.   : 49.790            
     remote_feasibility_score ai_adoption_level reskilling_urgency_score
     Min.   :10.01            Min.   : 0.01     Min.   : 2.456          
     1st Qu.:32.52            1st Qu.:24.71     1st Qu.:26.982          
     Median :54.77            Median :49.44     Median :35.871          
     Mean   :54.90            Mean   :49.80     Mean   :35.868          
     3rd Qu.:77.41            3rd Qu.:74.80     3rd Qu.:44.699          
     Max.   :99.99            Max.   :99.98     Max.   :71.579          

``` r
# 10. Save cleaned data
write.csv(df, "ai_job_replacement_cleaned.csv", row.names = FALSE)
```

## **Descriptive Analysis with Visualizations**

``` r
# Load visualization libraries
library(ggplot2)
library(viridis)
```

    Loading required package: viridisLite

``` r
library(gridExtra)
```


    Attaching package: 'gridExtra'

    The following object is masked from 'package:dplyr':

        combine

    The following object is masked from 'package:randomForest':

        combine

``` r
# Set theme
theme_set(theme_minimal() + 
          theme(plot.title = element_text(hjust = 0.5, face = "bold"),
                axis.text.x = element_text(angle = 45, hjust = 1)))
```

``` r
# ============================================
# OBJECTIVE 1 & 2: Job Risk and Salary Trends
# ============================================

# 1.1 Rank jobs by automation risk
job_risk <- df %>%
  group_by(job_role) %>%
  summarise(
    avg_risk = mean(automation_risk_percent),
    avg_salary_change = mean(salary_change_percent),
    count = n()
  ) %>%
  arrange(desc(avg_risk))
```

``` r
# Top 10 and bottom 10
top_10 <- head(job_risk, 10)
bottom_10 <- tail(job_risk, 10)
```

``` r
# Combine for plotting
risk_plot_data <- rbind(
  cbind(top_10, category = "Highest Risk"),
  cbind(bottom_10, category = "Lowest Risk")
)
```

``` r
# Bar plot
ggplot(risk_plot_data, aes(x = reorder(job_role, avg_risk), 
                            y = avg_risk, fill = category)) +
  geom_bar(stat = "identity") +
  coord_flip() +
  scale_fill_manual(values = c("Highest Risk" = "red", "Lowest Risk" = "green")) +
  labs(title = "Jobs with Highest and Lowest Automation Risk",
       x = "Job Role", y = "Average Automation Risk (%)")
```

![](Output_files/figure-commonmark/unnamed-chunk-53-1.png)

``` r
# 1.2 Risk vs Salary Change scatter plot
ggplot(df, aes(x = automation_risk_percent, y = salary_change_percent)) +
  geom_point(alpha = 0.3, color = "blue") +
  geom_smooth(method = "lm", color = "red") +
  labs(title = "Automation Risk vs Salary Change",
       x = "Automation Risk (%)", y = "Salary Change (%)")
```

    `geom_smooth()` using formula = 'y ~ x'

![](Output_files/figure-commonmark/unnamed-chunk-54-1.png)

``` r
# 1.3 Correlation
cor(df$automation_risk_percent, df$salary_change_percent, use = "complete.obs")
```

    [1] -0.005281448

``` r
# 1.4 Box plot by risk bracket
ggplot(df, aes(x = risk_bracket, y = salary_change_percent, fill = risk_bracket)) +
  geom_boxplot() +
  scale_fill_manual(values = c("Low Risk" = "green", "Medium Risk" = "orange", "High Risk" = "red")) +
  labs(title = "Salary Change by Risk Category", x = "Risk Category", y = "Salary Change (%)") +
  theme(legend.position = "none")
```

![](Output_files/figure-commonmark/unnamed-chunk-56-1.png)

``` r
# ============================================
# OBJECTIVE 3: Industry and Country Vulnerability
# ============================================

# 3.1 Industry summary
industry_summary <- df %>%
  group_by(industry) %>%
  summarise(
    avg_risk = mean(automation_risk_percent),
    avg_ai_score = mean(ai_replacement_score),
    avg_adoption = mean(ai_adoption_level),
    count = n()
  ) %>%
  arrange(desc(avg_risk))
```

``` r
industry_summary
```

    # A tibble: 8 × 5
      industry       avg_risk avg_ai_score avg_adoption count
      <fct>             <dbl>        <dbl>        <dbl> <int>
    1 Energy             47.0         47.2         50.0  1880
    2 Manufacturing      46.9         46.7         50.4  1822
    3 Finance            46.4         46.2         50.3  1942
    4 Retail             46.1         46.0         50.2  1865
    5 Transportation     45.9         46.0         50.7  1902
    6 Healthcare         45.8         45.8         49.2  1876
    7 Technology         45.7         45.5         48.5  1899
    8 Education          45.6         45.8         49.0  1814

``` r
# Industry bar plot
ggplot(industry_summary, aes(x = reorder(industry, avg_risk), y = avg_risk)) +
  geom_bar(stat = "identity", fill = "steelblue") +
  coord_flip() +
  labs(title = "Industry Vulnerability to AI", x = "Industry", y = "Avg Automation Risk (%)")
```

![](Output_files/figure-commonmark/unnamed-chunk-59-1.png)

``` r
# 3.2 Country summary
country_summary <- df %>%
  group_by(country) %>%
  summarise(
    avg_risk = mean(automation_risk_percent),
    avg_ai_score = mean(ai_replacement_score),
    avg_adoption = mean(ai_adoption_level),
    count = n()
  ) %>%
  arrange(desc(avg_risk))

# View
country_summary
```

    # A tibble: 9 × 5
      country   avg_risk avg_ai_score avg_adoption count
      <fct>        <dbl>        <dbl>        <dbl> <int>
    1 Australia     46.6         46.4         50.3  1642
    2 Singapore     46.5         46.6         50.0  1748
    3 UK            46.5         46.4         49.6  1716
    4 Japan         46.3         46.4         49.9  1638
    5 USA           46.1         45.9         49.4  1731
    6 Canada        46.1         46.2         49.2  1640
    7 India         46.1         46.3         50.5  1640
    8 Germany       45.8         45.5         50.0  1586
    9 Brazil        45.6         45.6         49.4  1659

``` r
# Country bar plot
ggplot(country_summary, aes(x = reorder(country, avg_risk), y = avg_risk)) +
  geom_bar(stat = "identity", fill = "darkred") +
  coord_flip() +
  labs(title = "Country Vulnerability to AI", x = "Country", y = "Avg Automation Risk (%)")
```

![](Output_files/figure-commonmark/unnamed-chunk-61-1.png)

``` r
# Country bar plot
ggplot(country_summary, aes(x = reorder(country, avg_risk), y = avg_risk)) +
  geom_bar(stat = "identity", fill = "darkred") +
  coord_flip() +
  labs(title = "Country Vulnerability to AI", x = "Country", y = "Avg Automation Risk (%)")
```

![](Output_files/figure-commonmark/unnamed-chunk-62-1.png)

``` r
# 3.3 Simple heatmap of top industries and countries
# Get top 5 industries and countries
top_industries <- head(industry_summary$industry, 5)
top_countries <- head(country_summary$country, 5)

# Filter data
heatmap_data <- df %>%
  filter(industry %in% top_industries, country %in% top_countries) %>%
  group_by(industry, country) %>%
  summarise(avg_risk = mean(automation_risk_percent), .groups = "drop")
```

``` r
# Simple heatmap
ggplot(heatmap_data, aes(x = country, y = industry, fill = avg_risk)) +
  geom_tile() +
  scale_fill_gradient(low = "white", high = "red") +
  labs(title = "Automation Risk: Top Industries vs Countries", fill = "Risk %")
```

![](Output_files/figure-commonmark/unnamed-chunk-63-1.png)

``` r
# ============================================
# OBJECTIVE 4: Reskilling Gap
# ============================================

# 4.1 Skill gap vs reskilling urgency
ggplot(df, aes(x = skill_gap_index, y = reskilling_urgency_score)) +
  geom_point(alpha = 0.3, color = "purple") +
  geom_smooth(method = "lm", color = "black") +
  labs(title = "Skill Gap vs Reskilling Urgency",
       x = "Skill Gap Index", y = "Reskilling Urgency")
```

    `geom_smooth()` using formula = 'y ~ x'

![](Output_files/figure-commonmark/unnamed-chunk-64-1.png)

``` r
# Correlation
cor(df$skill_gap_index, df$reskilling_urgency_score, use = "complete.obs")
```

    [1] 0.7015534

``` r
# 4.2 Jobs with highest reskilling urgency
top_reskilling <- df %>%
  group_by(job_role) %>%
  summarise(
    urgency = mean(reskilling_urgency_score),
    risk = mean(automation_risk_percent)
  ) %>%
  arrange(desc(urgency)) %>%
 head(15)
```

``` r
# Plot
ggplot(top_reskilling, aes(x = reorder(job_role, urgency), y = urgency, fill = risk)) +
  geom_bar(stat = "identity") +
  coord_flip() +
  scale_fill_gradient(low = "yellow", high = "red") +
  labs(title = "Top 15 Jobs Requiring Urgent Reskilling",
       x = "Job Role", y = "Reskilling Urgency", fill = "Risk %")
```

![](Output_files/figure-commonmark/unnamed-chunk-67-1.png)

``` r
# ============================================
# OBJECTIVE 5: Remote Work as Mitigating Factor
# ============================================

# 5.1 Remote feasibility vs automation risk
ggplot(df, aes(x = remote_feasibility_score, y = automation_risk_percent)) +
  geom_point(alpha = 0.3, color = "darkgreen") +
  geom_smooth(method = "lm", color = "red") +
  labs(title = "Remote Feasibility vs Automation Risk",
       x = "Remote Feasibility Score", y = "Automation Risk (%)")
```

    `geom_smooth()` using formula = 'y ~ x'

![](Output_files/figure-commonmark/unnamed-chunk-68-1.png)

``` r
# Correlation
cor(df$remote_feasibility_score, df$automation_risk_percent, use = "complete.obs")
```

    [1] 0.001511034

``` r
# 5.2 Remote feasibility by industry
ggplot(df, aes(x = reorder(industry, remote_feasibility_score, FUN = median), 
               y = remote_feasibility_score)) +
  geom_boxplot(fill = "lightblue") +
  coord_flip() +
  labs(title = "Remote Feasibility by Industry",
       x = "Industry", y = "Remote Feasibility Score")
```

![](Output_files/figure-commonmark/unnamed-chunk-70-1.png)

``` r
# ============================================
# OBJECTIVE 6: Educational Requirements
# ============================================

# 6.1 Automation risk by education level
ggplot(df, aes(x = education_requirement_level, y = automation_risk_percent)) +
  geom_boxplot(fill = "orange") +
  labs(title = "Automation Risk by Education Level",
       x = "Education Level (1=Lowest, 5=Highest)", y = "Automation Risk (%)")
```

![](Output_files/figure-commonmark/unnamed-chunk-71-1.png)

``` r
# 6.2 Salary by education level
ggplot(df, aes(x = education_requirement_level, y = salary_before_usd / 1000)) +
  geom_boxplot(fill = "steelblue") +
  labs(title = "Salary by Education Level",
       x = "Education Level (1=Lowest, 5=Highest)", y = "Salary (thousands USD)")
```

![](Output_files/figure-commonmark/unnamed-chunk-72-1.png)

``` r
# 6.3 Summary table
edu_summary <- df %>%
  group_by(education_requirement_level) %>%
  summarise(
    avg_risk = mean(automation_risk_percent),
    avg_salary = mean(salary_before_usd),
    avg_salary_change = mean(salary_change_percent),
    count = n()
  )
```

``` r
# View
edu_summary
```

    # A tibble: 5 × 5
      education_requirement_level avg_risk avg_salary avg_salary_change count
      <fct>                          <dbl>      <dbl>             <dbl> <int>
    1 1                               46.5     89603.            0.202   2893
    2 2                               45.6     90347.            0.387   3027
    3 3                               46.3     89438.           -0.186   3023
    4 4                               45.8     89711.            0.0633  3070
    5 5                               46.8     89751.            0.109   2987

## **Simple Predictive Models**

``` r
# Load modeling libraries
library(randomForest)
#library(caret)
set.seed(123)
```

``` r
# ============================================
# OBJECTIVE 7: Predicting Salary Change
# ============================================

# Prepare data
model_df <- df %>%
  select(job_role, industry, country, year,
         automation_risk_percent, ai_replacement_score,
         skill_gap_index, salary_before_usd,
         remote_feasibility_score, ai_adoption_level,
         education_requirement_level, salary_change_percent) %>%
  na.omit()
```

``` r
# Split data
set.seed(123)
train_idx <- sample(1:nrow(model_df), 0.7 * nrow(model_df))
train <- model_df[train_idx, ]
test <- model_df[-train_idx, ]
```

``` r
# Simple Random Forest model
rf_salary <- randomForest(
  salary_change_percent ~ .,
  data = train,
  ntree = 100,
  importance = TRUE
)
```

``` r
# Predictions
predictions <- predict(rf_salary, test)
```

``` r
# Calculate RMSE (Root Mean Square Error)
rmse <- sqrt(mean((predictions - test$salary_change_percent)^2))
print(paste("RMSE:", round(rmse, 3)))
```

    [1] "RMSE: 10.188"

``` r
# Calculate R-squared
ss_res <- sum((test$salary_change_percent - predictions)^2)
ss_tot <- sum((test$salary_change_percent - mean(test$salary_change_percent))^2)
r2 <- 1 - (ss_res / ss_tot)
print(paste("R-squared:", round(r2, 3)))
```

    [1] "R-squared: -0.017"

``` r
# Feature importance
importance_df <- data.frame(
  feature = rownames(importance(rf_salary)),
  importance = importance(rf_salary)[, "%IncMSE"]
) %>%
  arrange(desc(importance))
```

``` r
# Plot top 10 features
head(importance_df, 10) %>%
  ggplot(aes(x = reorder(feature, importance), y = importance)) +
  geom_bar(stat = "identity", fill = "steelblue") +
  coord_flip() +
  labs(title = "Top Factors Affecting Salary Change",
       x = "Feature", y = "Importance")
```

![](Output_files/figure-commonmark/unnamed-chunk-83-1.png)

``` r
# ============================================
# OBJECTIVE 8: Classifying Job Risk
# ============================================

# Create classification dataset
class_df <- df %>%
  select(job_role, industry, country, year,
         ai_replacement_score, skill_gap_index,
         remote_feasibility_score, ai_adoption_level,
         education_requirement_level, risk_bracket) %>%
  na.omit()
```

``` r
# Split
set.seed(123)
train_idx_c <- sample(1:nrow(class_df), 0.7 * nrow(class_df))
train_c <- class_df[train_idx_c, ]
test_c <- class_df[-train_idx_c, ]
```

``` r
# Random Forest Classifier
rf_class <- randomForest(
  risk_bracket ~ .,
  data = train_c,
  ntree = 100,
  importance = TRUE
)
```

``` r
# Predictions
pred_class <- predict(rf_class, test_c)
```

``` r
# Confusion Matrix
table(Predicted = pred_class, Actual = test_c$risk_bracket)
```

                 Actual
    Predicted     Low Risk Medium Risk High Risk
      Low Risk        1292         104         0
      Medium Risk      120        1942       215
      High Risk          0         154       673

``` r
# Accuracy
accuracy <- mean(pred_class == test_c$risk_bracket)
print(paste("Accuracy:", round(accuracy, 3)))
```

    [1] "Accuracy: 0.868"

``` r
# Feature importance for classification
class_imp <- data.frame(
  feature = rownames(importance(rf_class)),
  importance = importance(rf_class)[, "MeanDecreaseGini"]
) %>%
  arrange(desc(importance))
```

``` r
# Plot
head(class_imp, 10) %>%
  ggplot(aes(x = reorder(feature, importance), y = importance)) +
  geom_bar(stat = "identity", fill = "darkgreen") +
  coord_flip() +
  labs(title = "Top Factors for Predicting Risk Category",
       x = "Feature", y = "Importance")
```

![](Output_files/figure-commonmark/unnamed-chunk-91-1.png)

``` r
# ============================================
# OBJECTIVE 9: Predicting Reskilling Urgency
# ============================================

# Similar to Objective 7 but with different target
urgency_df <- df %>%
  select(job_role, industry, country, year,
         automation_risk_percent, ai_replacement_score,
         skill_gap_index, remote_feasibility_score,
         ai_adoption_level, education_requirement_level,
         reskilling_urgency_score) %>%
  na.omit()
```

``` r
# Split
set.seed(123)
train_idx_u <- sample(1:nrow(urgency_df), 0.7 * nrow(urgency_df))
train_u <- urgency_df[train_idx_u, ]
test_u <- urgency_df[-train_idx_u, ]
```

``` r
# Random Forest for urgency
rf_urgency <- randomForest(
  reskilling_urgency_score ~ .,
  data = train_u,
  ntree = 100,
  importance = TRUE
)
```

``` r
# Predictions
pred_urgency <- predict(rf_urgency, test_u)
```

``` r
# RMSE
rmse_u <- sqrt(mean((pred_urgency - test_u$reskilling_urgency_score)^2))
print(paste("Reskilling Urgency RMSE:", round(rmse_u, 3)))
```

    [1] "Reskilling Urgency RMSE: 2.157"

``` r
# Feature importance
urgency_imp <- data.frame(
  feature = rownames(importance(rf_urgency)),
  importance = importance(rf_urgency)[, "%IncMSE"]
) %>%
  arrange(desc(importance))
```

``` r
# Plot
head(urgency_imp, 10) %>%
  ggplot(aes(x = reorder(feature, importance), y = importance)) +
  geom_bar(stat = "identity", fill = "purple") +
  coord_flip() +
  labs(title = "Top Factors Affecting Reskilling Urgency",
       x = "Feature", y = "Importance")
```

![](Output_files/figure-commonmark/unnamed-chunk-98-1.png)

## **Advanced Analysis**

``` r
# ============================================
# OBJECTIVE 10: Temporal Analysis
# ============================================

# Yearly trends
yearly <- df %>%
  group_by(year) %>%
  summarise(
    risk = mean(automation_risk_percent),
    adoption = mean(ai_adoption_level),
    ai_score = mean(ai_replacement_score),
    salary_change = mean(salary_change_percent)
  )
```

``` r
# Plot multiple trends
par(mfrow = c(2, 2))
plot(yearly$year, yearly$risk, type = "b", col = "red", 
     xlab = "Year", ylab = "Risk %", main = "Automation Risk Trend")
plot(yearly$year, yearly$adoption, type = "b", col = "blue",
     xlab = "Year", ylab = "Adoption Level", main = "AI Adoption Trend")
plot(yearly$year, yearly$ai_score, type = "b", col = "green",
     xlab = "Year", ylab = "AI Score", main = "AI Replacement Score Trend")
plot(yearly$year, yearly$salary_change, type = "b", col = "purple",
     xlab = "Year", ylab = "Salary Change %", main = "Salary Change Trend")
```

![](Output_files/figure-commonmark/unnamed-chunk-100-1.png)

``` r
par(mfrow = c(1, 1))
```

``` r
# Trends by country (top 5 countries)
top_countries <- names(sort(table(df$country), decreasing = TRUE)[1:5])

df %>%
  filter(country %in% top_countries) %>%
  group_by(country, year) %>%
  summarise(avg_risk = mean(automation_risk_percent), .groups = "drop") %>%
  ggplot(aes(x = year, y = avg_risk, color = country, group = country)) +
  geom_line(size = 1) +
  geom_point(size = 2) +
  labs(title = "Automation Risk Trends by Country",
       x = "Year", y = "Avg Risk (%)")
```

    Warning: Using `size` aesthetic for lines was deprecated in ggplot2 3.4.0.
    ℹ Please use `linewidth` instead.

![](Output_files/figure-commonmark/unnamed-chunk-101-1.png)

``` r
# ============================================
# OBJECTIVE 11: Simple Cluster Analysis
# ============================================

# Prepare job-level data
job_data <- df %>%
  group_by(job_role) %>%
  summarise(
    risk = mean(automation_risk_percent),
    ai_score = mean(ai_replacement_score),
    skill_gap = mean(skill_gap_index),
    remote = mean(remote_feasibility_score),
    urgency = mean(reskilling_urgency_score),
    count = n()
  ) %>%
  filter(count > 10)  # Only jobs with enough data
```

``` r
# Scale the data
job_scaled <- scale(job_data[, c("risk", "ai_score", "skill_gap", "remote", "urgency")])
```

``` r
# Try 3 clusters
set.seed(123)
kmeans_result <- kmeans(job_scaled, centers = 3, nstart = 25)
```

``` r
# Add cluster to data
job_data$cluster <- as.factor(kmeans_result$cluster)
```

``` r
# View cluster characteristics
job_data %>%
  group_by(cluster) %>%
  summarise(
    avg_risk = mean(risk),
    avg_ai_score = mean(ai_score),
    avg_skill_gap = mean(skill_gap),
    avg_remote = mean(remote),
    avg_urgency = mean(urgency),
    count = n()
  )
```

    # A tibble: 3 × 7
      cluster avg_risk avg_ai_score avg_skill_gap avg_remote avg_urgency count
      <fct>      <dbl>        <dbl>         <dbl>      <dbl>       <dbl> <int>
    1 1           60.3         60.2          50.7       54.6        41.7     2
    2 2           45.1         45.1          50.0       55.1        35.5     6
    3 3           35.2         35.1          49.3       54.7        31.2     2

``` r
# Visualize clusters (using first two principal components)
pca <- prcomp(job_scaled)
plot_data <- data.frame(
  PC1 = pca$x[, 1],
  PC2 = pca$x[, 2],
  cluster = job_data$cluster,
  job = job_data$job_role
)

ggplot(plot_data, aes(x = PC1, y = PC2, color = cluster, label = job)) +
  geom_point(size = 3) +
  #geom_text_repel(size = 2, max.overlaps = 5) +
  labs(title = "Job Clusters Based on AI Risk Factors") +
  theme(legend.position = "bottom")
```

![](Output_files/figure-commonmark/unnamed-chunk-107-1.png)

``` r
# ============================================
# OBJECTIVE 12: The AI Paradox
# ============================================

# Industry-level analysis
industry_paradox <- df %>%
  group_by(industry) %>%
  summarise(
    adoption = mean(ai_adoption_level),
    risk = mean(automation_risk_percent),
    count = n()
  )
```

``` r
# Scatter plot
ggplot(industry_paradox, aes(x = adoption, y = risk, label = industry)) +
  geom_point(size = 4, aes(color = count)) +
  #geom_text_repel() +
  scale_color_gradient(low = "blue", high = "red") +
  labs(title = "AI Paradox: Adoption vs Risk by Industry",
       x = "Average AI Adoption Level", y = "Average Automation Risk (%)",
       color = "Sample Size") +
  geom_smooth(method = "lm", se = FALSE, color = "gray", linetype = "dashed")
```

    `geom_smooth()` using formula = 'y ~ x'

    Warning: The following aesthetics were dropped during statistical transformation: label.
    ℹ This can happen when ggplot fails to infer the correct grouping structure in
      the data.
    ℹ Did you forget to specify a `group` aesthetic or to convert a numerical
      variable into a factor?

![](Output_files/figure-commonmark/unnamed-chunk-109-1.png)

``` r
# Correlation
cor(industry_paradox$adoption, industry_paradox$risk)
```

    [1] 0.5570053

``` r
# ============================================
# OBJECTIVE 13: Globalization of AI Risk
# ============================================

# Select a few common job roles
common_jobs <- c("Software Engineer", "Data Analyst", "Teacher", 
                 "Financial Analyst", "Customer Support Rep")

# Filter data
job_country <- df %>%
  filter(job_role %in% common_jobs) %>%
  group_by(job_role, country) %>%
  summarise(avg_risk = mean(automation_risk_percent), .groups = "drop")
```

``` r
# Create bar plot
ggplot(job_country, aes(x = country, y = avg_risk, fill = job_role)) +
  geom_bar(stat = "identity", position = "dodge") +
  labs(title = "Automation Risk by Country for Selected Jobs",
       x = "Country", y = "Average Automation Risk (%)", fill = "Job Role") +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

![](Output_files/figure-commonmark/unnamed-chunk-112-1.png)

``` r
# Alternative: heatmap
job_country_matrix <- job_country %>%
  pivot_wider(names_from = country, values_from = avg_risk, values_fill = 0)

job_country_matrix %>%
  pivot_longer(-job_role, names_to = "country", values_to = "risk") %>%
  ggplot(aes(x = country, y = job_role, fill = risk)) +
  geom_tile() +
  scale_fill_gradient(low = "white", high = "red") +
  labs(title = "Heatmap of AI Risk: Jobs vs Countries",
       x = "Country", y = "Job Role", fill = "Risk %")
```

![](Output_files/figure-commonmark/unnamed-chunk-113-1.png)

``` r
# ============================================
# FINAL SUMMARY: Key Insights
# ============================================

# Create a simple summary of the most important findings
```

``` r
# 1. Most and least risky jobs
most_risky <- head(job_risk$job_role, 3)
least_risky <- tail(job_risk$job_role, 3)

# 2. Most vulnerable industries
top_risky_industries <- head(industry_summary$industry, 3)

# 3. Most vulnerable countries
top_risky_countries <- head(country_summary$country, 3)

# 4. Education and risk
edu_risk_correlation <- cor(as.numeric(df$education_requirement_level), 
                            df$automation_risk_percent, use = "complete.obs")
```

``` r
# Print summary
print("=== KEY INSIGHTS ===")
```

    [1] "=== KEY INSIGHTS ==="

``` r
print(paste("Most risky jobs:", paste(most_risky, collapse = ", ")))
```

    [1] "Most risky jobs: Truck Driver, Customer Support Rep, Marketing Specialist"

``` r
print(paste("Least risky jobs:", paste(least_risky, collapse = ", ")))
```

    [1] "Least risky jobs: Financial Analyst, Data Analyst, Software Engineer"

``` r
print(paste("Most vulnerable industries:", paste(top_risky_industries, collapse = ", ")))
```

    [1] "Most vulnerable industries: Energy, Manufacturing, Finance"

``` r
print(paste("Most vulnerable countries:", paste(top_risky_countries, collapse = ", ")))
```

    [1] "Most vulnerable countries: Australia, Singapore, UK"

``` r
print(paste("Education vs Risk correlation:", round(edu_risk_correlation, 3)))
```

    [1] "Education vs Risk correlation: 0.005"

``` r
print(paste("Remote work reduces risk (correlation):", 
            round(cor(df$remote_feasibility_score, df$automation_risk_percent), 3)))
```

    [1] "Remote work reduces risk (correlation): 0.002"

``` r
print(paste("Higher skill gap = higher reskilling urgency (correlation):",
            round(cor(df$skill_gap_index, df$reskilling_urgency_score), 3)))
```

    [1] "Higher skill gap = higher reskilling urgency (correlation): 0.702"
