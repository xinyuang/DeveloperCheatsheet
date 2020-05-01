process time
```python
import time
from time import mktime
        absTime = time.strptime(start_time,'%Y-%m-%d-%H-%M-%S')  # convert time string to time obj
        start_time_stamp = mktime(absTime)              # convert time obj to unix time stamp i.e.1584752096.0
```
        