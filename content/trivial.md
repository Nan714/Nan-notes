
### random

```python
import random
random.random()
#> 0.0 <= x < 1.0
random.randint(1, 10)#含两端 含10
random.randrange(1, 10, 2)
#类似range(1, 10, 2) 不含10
nums = [1, 2, 3]
random.choice(nums)
#随机选一个
random.sample(nums, 2)
#随机选多个 不重复
nums = [1, 2, 3]
random.shuffle(nums)#打乱顺序 原地！不能x = random.shuffle(nums)

print(nums)
random.choices(["A", "B"], weights=[0.8, 0.2], k=10) #可重复抽样，带权重
random.seed(42)#控制随机 方便调试
```
