Loading in the packages and the original given dataset.

```{r}
library(tidyverse)
library(readxl)

RTL_voters_RI_2020 <- read_excel(".../RI Voter Data/RTL voters RI 2020.xlsx")
```

## Shortening the dataset to voters from East Greenwich, West Greenwich, and Warwick. 
These are the only cities with citizens in the RI House 20 and RI Senate 35 Voters. 
```{r}
value = which(RTL_voters_RI_2020$RegCity == 'EAST GREENWICH' | RTL_voters_RI_2020$RegCity == 'WEST GREENWICH' | 
                RTL_voters_RI_2020$RegCity == 'Warwick')
eg <- RTL_voters_RI_2020[value,]
write.csv(eg,"...\RI_House30_Voters.csv", row.names = FALSE)
```
