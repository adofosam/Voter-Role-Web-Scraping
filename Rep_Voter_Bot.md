## Loading in Libraries
``` Python
from selenium import webdriver
from selenium.webdriver.common.by import By
import pandas as pd
from pandas import DataFrame
 
```
## Loading in the data that I gathered from R
``` Python
#votes_data = pd.read_csv("RI_House30_Voters.csv") # Uncomment this line to run this for the House Voters.
votes_data = pd.read_csv("RI_Senate35_Voters.csv")
state_rep = []
state_senate = []
```

## Looping through the dataset and parsing the information for each voter.
``` Python
for i in range((len(votes_data))):
#for i in range(30):
    
    address_str = str(votes_data['RegistrationAddr1'][i])
    city_str = str(votes_data['RegCity'][i])
    zc_str = str('0' + str(votes_data['RegZip5'][i])) 
    
    browser = webdriver.Chrome()
    browser.get(('https://vote.sos.ri.gov/Home/PollingPlaces'))
        
    address = browser.find_element_by_name('_pollingPlaces.StreetAddress')
    address.send_keys(address_str)
    
    city = browser.find_element_by_name('_pollingPlaces.City')
    city.send_keys(city_str)
    
    zc = browser.find_element_by_name('_pollingPlaces.Zip')
    zc.send_keys(zc_str)
    
    nextButton = browser.find_element_by_id('Continue')
    nextButton.click()
    
    browser.implicitly_wait(2)
    
    state_size = len(state_rep)
    
    elements = browser.find_elements(By.TAG_NAME, 'li')
    
    for e in elements:
        
        if(e.text.startswith("STATE REP")):
            
            state_rep.append(e.text[-2] + e.text[-1])
            
        if(e.text.startswith("STATE SENATE")):
            
            state_senate.append(e.text[-2] + e.text[-1])
            
            
        
    if len(state_rep) == state_size:
        state_rep.append("NA")
        state_senate.append("NA")
        
    browser.quit()

print(state_rep)
print(state_senate)
states = {'RegAdr' : votes_data['RegistrationAddr1'],
          'state_rep' : state_rep,
          'state_senate': state_senate}

df = DataFrame(states, columns= ['RegAdr','state_rep', 'state_senate'])
```

## Printing to dataframe
#df.to_csv("house30_rep.csv") # Uncomment this line to run this for the House Voters.        
df.to_csv('Senate35_rep.csv')

## Comments on this code
This particular process takes a non-insignificant amount of time. To speed up this process, you should run this in your GPU because the process is parallel.
You could also run and open multiple browsers at once without significantly slowing your laptop.
