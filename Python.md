process time
```python
import time
from time import mktime
# convert time string to time obj
absTime = time.strptime(start_time,'%Y-%m-%d-%H-%M-%S')  
# convert time obj to unix time stamp i.e.1584752096.0
start_time_stamp = mktime(absTime)              
```
        