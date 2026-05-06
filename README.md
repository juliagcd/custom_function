# custom_function

This repository contains a custom R function I created for exploratory data analysis.

The function, `grouped_summary()`, summarizes a numeric variable across groups and reports observations, missing values, missing-value percentages, mean, median, standard deviation, minimum, maximum, and a simple completeness check.

## Custom Function: Utility

The function is useful when working with datasets that include repeated groups, such as months, countries, years, treatment groups, or categories. Instead of manually writing separate summary code for each variable, the function creates a consistent summary table that helps evaluate both data quality and variable patterns.

In this demo, I apply the function to R's built-in `airquality` dataset and summarize `Ozone`, `Solar.R`, and `Temp` by month.

## Replication Code

```r
library(tidyverse)

grouped_summary <- function(x, group, var_name) {
  tibble(
    Month = group,
    Value = x
  ) |>
    group_by(Month) |>
    summarise(
      Variable = var_name,
      Observations = n(),
      Missing = sum(is.na(Value)),
      Missing_Pct = round(sum(is.na(Value)) / n() * 100, 1),
      Mean = round(mean(Value, na.rm = TRUE), 1),
      Median = round(median(Value, na.rm = TRUE), 1),
      SD = round(sd(Value, na.rm = TRUE), 1),
      Min = min(Value, na.rm = TRUE),
      Max = max(Value, na.rm = TRUE),
      Check = ifelse(sum(is.na(Value)) > 0, "Missing values", "Complete"),
      .groups = "drop"
    )
}

air_quality <- bind_rows(
  grouped_summary(airquality$Ozone, airquality$Month, "Ozone"),
  grouped_summary(airquality$Solar.R, airquality$Month, "Solar.R"),
  grouped_summary(airquality$Temp, airquality$Month, "Temp")
)

air_quality
```

## Files

- `function_demo.qmd`: Quarto source file with explanation, code, and output.
- `function_demo.pdf`: Rendered output showing the function and demonstration.