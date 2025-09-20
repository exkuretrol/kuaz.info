---
title: "在 R 中建立一個空的資料框"
date: 2023-12-18T09:16:26+08:00
tags: ["R"]
draft: false
---

建立一個空的 `data.frame`。

```r
summary_duplicated_dataframe <- data.frame(
    list(
        filename = character(),
        normal_record = integer(),
        duplicated_record = integer()
    )
)
```

另一個範例，建立一個空的 `tibble`。

```r
generate_empty_dataframe <- \() {
    df <- c(
        "filename",
        "year",
        "month",
        "duplicated_rows",
        "duplicated_rows_num",
        "num_of_rows"
    ) %>%
        paste(collapse = ",") %>%
        I() %>%
        readr::read_csv(
            col_types = "ciicii"
        )
    return(df)
}
```