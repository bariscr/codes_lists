# Codes

-   [SQL](#sql)
    -   [Connection](#connection)
    -   [insert into](#insert-into)
-   [Datatable (DT)](#datatable-dt)
    -   [Columndefs](#columndefs)
-   [Excel](#excel)
    -   [Show all excel sheets as
        datatable](#show-all-excel-sheets-as-datatable)
    -   [Download all excel sheets to the
        environment](#download-all-excel-sheets-to-the-environment)
-   [Plotly](#plotly)
    -   [hline](#hline)
-   [File operations - use very
    carefully](#file-operations---use-very-carefully)
-   [Yedekleme](#yedekleme)

``` r
file.copy("codes.html", "www/codes.html", overwrite = TRUE)
```

# SQL

## Connection

``` r
con2 <- dbConnect(MySQL(),
  user = Sys.getenv("xxx"),
  password = Sys.getenv("xxx")
)

dbGetQuery(con2, "set names utf8")
```

## insert into

``` r
a <- read_excel("/path/excel_file.xlsx",
           sheet = "done"
           ) |> 
  filter(!is.na(event)) |> 
  mutate(date_done = as.character(as.Date(date)),
         event = str_replace_all(event, "'", " "),
         )

a$id = 1:nrow(a)

for (i in 1:nrow(a)) {
  
dbGetQuery(con2, glue::glue('insert into daily.table_x (id, event, date_done)
                            values (
                                      ',  a$id[i], ',
                                     \'', a$event[i], '\',
                                     \'', a$date_done[i], '\');'))
}
```

# Datatable (DT)

``` r
datatable(data, 
          rownames = FALSE,
          selection = 'single',
          escape = F,
          options = list(scrollX = TRUE,
                         iDisplayLength = 50,
                         initComplete = JS(
                           "function(settings, json) {",
                           "$('.dataTables_wrapper .dataTables_scroll .dataTables_scrollBody table.dataTable').css('font-size', '20px');",
                           "}"
                         )
          ))
```

## Columndefs

``` r
columnDefs = list(
                 list(targets = "_all", width = "150px") 
```

# Excel

## Show all excel sheets as datatable

``` r
library(purrr)

path <- "data/info.xlsx"

map(
  .x = excel_sheets(path),
  .f = function(x) {
    read_excel(path, sheet = x) |> DT::datatable()
  }
)
```

## Download all excel sheets to the environment

``` r
walk(
  .x = excel_sheets(path),
  .f = function(x) {
    assign(x, read_excel(path, sheet = x), envir = .GlobalEnv)
  }
)
```

# Plotly

## hline

``` r
hline <- function(y = 0, color = "blue") {
  list(
    type = "line",
    x0 = 0,
    x1 = 1,
    xref = "paper",
    y0 = y,
    y1 = y,
    line = list(color = color)
  )
}
```

# File operations - use very carefully

``` r
dir.create("/xxxxxxxxxx")
a
fs::dir_delete("/Users/xxxxx/xxxxxxxx/xxxxxxxx")
```

# Yedekleme

``` r
# path i belirle
path <- "/Users/xxxxx/xxxx/dataxxxxxx"
# önce içerisinde directory oluştur
library(lubridate)
library(stringr)
a <- Sys.time()
a1 <- paste0(date(a),  "_", 
             formatC(ceiling(hour(a)), width = 2, flag = "0"),
             formatC(ceiling(minute(a)), width = 2, flag = "0"),
             formatC(ceiling(second(a)), width = 2, flag = "0"))

ek <- str_replace_all(a1, "-", "")

path_new <- paste0(path, "yedek_coord2_", ek, "/") # path i güncelle
#dir.create(path_new)

fs::dir_copy("/Users/xxxx/Documents/projectsxxxxxx/xxxx" , path_new)
```
