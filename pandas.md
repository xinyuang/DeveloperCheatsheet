# Pandas

append list of dictionary\(row\) to data frame

```python
import pandas as pd
rows = []
cur_data = {}
for row in row_data:
    rows.append(row.copy())
df = pd.DataFrame(rows)   
df.to_csv("out.csv",index=False)
```

query data frame

```python
df1['a'].iloc[[-1]] #get the last value
ttl_df = pd.DataFrame() #init empty dataframe
ttl_df = pd.concat([ttl_df,next_df], ignore_index=True) #concat df
```

