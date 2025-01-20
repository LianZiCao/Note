lower_bound返回指向首个不小于给定键的元素的迭代器

upper_bound返回指向首个大于给定键的元素的迭代器


[std::set<Key,Compare,Allocator>::equal_range](https://zh.cppreference.com/w/cpp/container/set/equal_range)
一个指向首个不小于 key 的元素，另一个指向首个大于 key 的元素。首个迭代器可以换用 lower_bound() 获得，而第二迭代器可换用 upper_bound() 获得。

[std::prev](https://zh.cppreference.com/w/cpp/iterator/prev)返回迭代器 it 的第 n 个前驱（或当 n 是负数时为第 n 个后继）。