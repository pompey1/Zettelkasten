202407152047
Status: #idea
Tags: [[C++]]
# atomic 原子操作
std::atomic< typename > 提供了很多原子参数的接口，这些接口都需要指定内存顺序
1. memory_order_relaxed: 只能确保操作是原子性的，不能保证内存顺序
2. memory_order_release: 用于写操作，在写之前加入一个屏障
3. memory_order_acquire: 用于读操作，在读之前加入一个屏障
4. memory_order_acq_rel: memory_order_acquire + memory_order_release
5. memory_order_seq_cst: 相当于直接禁用 cache，只从内存中保存读取数据，迫不得已才用

---
# References
[CPU流水线技术全面解读-CSDN博客](https://blog.csdn.net/weixin_43719763/article/details/135769082)
[大白话C++之：一文搞懂C++多线程内存模型(Memory Order)_c++ memory order-CSDN博客](https://blog.csdn.net/sinat_38293503/article/details/134612152)