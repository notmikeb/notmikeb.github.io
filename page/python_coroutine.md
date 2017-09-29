---
layout: page
title: "python coroutine"
description: ""
---
{% include JB/setup %}

## python co-routine async and await tutorial
### A chart and example
Sample
[https://docs.python.org/3/library/asyncio-task.html](https://docs.python.org/3/library/asyncio-task.html)

![](https://docs.python.org/3/_images/tulip_coro.png)

```
import asyncio

async def compute(x, y):
    print("Compute %s + %s ..." % (x, y))
    await asyncio.sleep(1.0 * x * y)
    return x + y

async def print_sum(x, y):
    result = await compute(x, y)
    print("%s + %s = %s" % (x, y, result))

loop = asyncio.get_event_loop()
loop.run_until_complete( asyncio.gather(print_sum(1, 2), print_sum(2, 3), print_sum(2, 2)) )
loop.close()
```

output
```
J:\temp>python co2.py
Compute 3 + 2 ...
Compute 2 + 2 ...
Compute 1 + 2 ...
1 + 2 = 3
2 + 2 = 4
3 + 2 = 5
```

### Python async/await Tutorial
[http://stackabuse.com/python-async-await-tutorial/)](http://stackabuse.com/python-async-await-tutorial/)