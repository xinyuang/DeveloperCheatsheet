append list of dictionary(row) to data frame
```python
import pandas as pd
rows = []
cur_data = {}
for row in row_data:
    rows.append(row.copy())
df = pd.DataFrame(rows)   
df.to_csv("out.csv")
```