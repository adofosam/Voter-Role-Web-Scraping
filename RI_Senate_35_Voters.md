## Loading in the packages and data
```{r}
library(tidyverse)
library(readxl)

RTL_voters_RI_2020 <- read_excel("C:/Users/Samuel/OneDrive - nd.edu/RI Voter Data/RTL voters RI 2020.xlsx")
```

## Shortening the dataset to voters from East Greenwich, West Greenwich, and Warwick
```{r}
value = which(RTL_voters_RI_2020$RegCity == 'EAST GREENWICH' | RTL_voters_RI_2020$RegCity == 'EXETER' | 
                RTL_voters_RI_2020$RegCity == 'NORTH KINGSTOWN' | RTL_voters_RI_2020$RegCity == 'NARRAGANSETT' | 
                RTL_voters_RI_2020$RegCity == 'Galilee')
val <- RTL_voters_RI_2020[value,]
#write.csv(val,"C:\\Users\\Samuel\\OneDrive - nd.edu\\RI_Senate35_Voters.csv", row.names = FALSE)
```

## Loading in the data from the Bot program.
```{r}

senate_rep_data <- read.csv('Senate35_rep.csv', header = TRUE)
senate_rep_data.1 <- senate_rep_data %>% select(RegAdr, state_rep, state_senate)
senate35_data <- cbind(val, senate_rep_data.1)

#missing <- which(is.na(house30_data$state_rep))
#length(missing)

#house30_data$state_rep[missing] <- ''

district_35_r <- which(senate35_data$state_rep == 35)
district_na_r <- which(is.na(senate35_data$state_rep))

voter35_data <- senate35_data[district_35_r,]
NA_voter <- senate35_data[district_na_r,]

```

## Listing the voters in house district 30 who are Republican, Unsure, and Democrat
```{r}

senate35_r <- which(senate35_data$state_rep == 30 & senate35_data$OfficialParty == "R")
senate35_u <- which(senate35_data$state_rep == 30 & senate35_data$OfficialParty == "U")
senate35_d <- which(senate35_data$state_rep == 30 & senate35_data$OfficialParty == "D")

r_voter <- senate35_data[senate35_r,]
u_voter <- senate35_data[senate35_u,]
d_voter <- senate35_data[senate35_d,]

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
write.csv(r_voters,"C:\\Users\\Samuel\\OneDrive - nd.edu\\RI_Senate35_Rep_Voters.csv", row.names = FALSE)

# Dem
write.csv(d_voters,"C:\\Users\\Samuel\\OneDrive - nd.edu\\RI_Senate35_Dem_Voters.csv", row.names = FALSE)

# Unsure
write.csv(u_voters,"C:\\Users\\Samuel\\OneDrive - nd.edu\\RI_Senate35_Und_Voters.csv", row.names = FALSE)

# Unregistered
write.csv(na_voters,"C:\\Users\\Samuel\\OneDrive - nd.edu\\RI_Senate35_NA_Voters.csv", row.names = FALSE)
```
