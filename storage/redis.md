# Redis

## 如何做到O\(1\) 的查找性能

查看了源码 确认他用的是 hash算法 可以保证 接近O\(1\) 的时间复杂度，所以 100条与 100W 查询性能不会有太大区别，他用的是 MurmurHash2 算法。

