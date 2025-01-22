# insights-on-Airbnb-Tokyo-
python analysis using calendar and listing dataset of Tokyo 
## For the following analysis I have download the data from https://data.insideairbnb.com/the-netherlands/south-holland/the-hague/2024-09-21/data/calendar.csv.gz
``` diff
import pandas as pd
data=pd.read_csv("calendar.csv")
```
## 1 want to know the number of available and unavailable rooms
``` diff
data.available.value_counts()
```
<img width="360" alt="image" src="https://github.com/user-attachments/assets/abf39c86-5b3d-46da-a0c1-295c911aa600" />

f(false) means not available, t(true) means available 

## 2 purpose: calculates the percentage of available (t) and unavailable (f) dates.
Explanation value_counts(normalize=True) give proportion. Multiplaying by 100 converts the proportion into percentage


``` diff
availability_percentage = calendar['available'].value_counts(normalize=True) * 100
availability_percentage
```
