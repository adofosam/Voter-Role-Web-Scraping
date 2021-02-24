## Loading in the packages and data from the previous Python output

```{r}
library(tidyverse)
library(readxl)
RTL_voters_RI_2020 <- read_excel(".../RI Voter Data/RTL voters RI 2020.xlsx")
```
## Shortening the dataset to voters from East Greenwich, West Greenwich, and Warwick
```{r}
value = which(RTL_voters_RI_2020$RegCity == 'EAST GREENWICH' | RTL_voters_RI_2020$RegCity == 'WEST GREENWICH' | 
                RTL_voters_RI_2020$RegCity == 'Warwick')
eg <- RTL_voters_RI_2020[value,]
#write.csv(eg,"C:\\Users\\Samuel\\OneDrive - nd.edu\\RI_House30_Voters.csv", row.names = FALSE)
```

## Loading in the data from the Bot program.
```{r}

house_rep_data <- read.csv('house30_rep.csv', header = TRUE)
house_rep_data.1 <- house_rep_data %>% select(RegAdr, state_rep, state_senate)
house30_data <- cbind(eg, house_rep_data.1)


district_30_r <- which(house30_data$state_rep == 30)
district_na_r <- which(is.na(house30_data$state_rep))

voter30_data <- house30_data[district_30_r,]
NA_voter <- house30_data[district_na_r,]

```

## Listing the voters in house district 30 who are Republican, Unsure, and Democrat
```{r}

house30_r <- which(house30_data$state_rep == 30 & house30_data$OfficialParty == "R")
house30_u <- which(house30_data$state_rep == 30 & house30_data$OfficialParty == "U")
house30_d <- which(house30_data$state_rep == 30 & house30_data$OfficialParty == "D")

r_voter <- house30_data[house30_r,]
u_voter <- house30_data[house30_u,]
d_voter <- house30_data[house30_d,]

# Selecting Republican Voters
r_voters <- r_voter %>% select(PrecinctName...3, FirstName, LastName, Sex, Age, OfficialParty, MailingAddr1, MailingAddr2, RegCity, 'Cell Phone', Landline)

# Selecting Undecided Voters
u_voters <- u_voter %>% select(PrecinctName...3, FirstName, LastName, Sex, Age, OfficialParty, MailingAddr1, MailingAddr2, RegCity, 'Cell Phone', Landline)

# Selecting Democratic Voters
d_voters <- d_voter %>% select(PrecinctName...3, FirstName, LastName, Sex, Age, OfficialParty, MailingAddr1, MailingAddr2, RegCity, 'Cell Phone', Landline)

# Selecting Voters who may not be vegistered voters
na_voters <- NA_voter %>% select(PrecinctName...3, FirstName, LastName, Sex, Age, OfficialParty, MailingAddr1, MailingAddr2, RegCity, 'Cell Phone', Landline)

```

## Saving Files
```{r}
# Republican
write.csv(r_voters,"C:\\Users\\Samuel\\OneDrive - nd.edu\\RI_House30_Rep_Voters.csv", row.names = FALSE)

# Dem
write.csv(d_voters,"C:\\Users\\Samuel\\OneDrive - nd.edu\\RI_House30_Dem_Voters.csv", row.names = FALSE)

# Unsure
write.csv(u_voters,"C:\\Users\\Samuel\\OneDrive - nd.edu\\RI_House30_Und_Voters.csv", row.names = FALSE)

# Unregistered
write.csv(na_voters,"C:\\Users\\Samuel\\OneDrive - nd.edu\\RI_House30_NA_Voters.csv", row.names = FALSE)


```
