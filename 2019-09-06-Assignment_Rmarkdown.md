---
title: "Sales Demography over five years"
author: "Alex"
date: "06/09/2019"
output:
  pdf_document: default
  html_document: default
---
# R-Markdown document
## By Alex Ogbole
### Assignment on Advanced Crime Statistics
This is a chunck of block ploting sales in  city over five year period 
```{r message=FALSE}
library(ggplot2)
g <- ggplot(txhousing, aes(x = date, y = sales, group = city)) +
  geom_line(alpha = 0.4)
g
```
