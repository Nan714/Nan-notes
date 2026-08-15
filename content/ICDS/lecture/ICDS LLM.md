解决办法有两个最经典：

  

- **平滑（smoothing）**：给没见过的组合分一点点极小概率，不让它变成 0
- **回退（backoff）**：如果 3-gram 没见过，就降级用 2-gram；2-gram 也没有，就用 1-gram


**act**: using tools