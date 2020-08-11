# C++常用操作

相关博客：[LeetCode 牛客网 C++ 算法 刷题必备基础知识](https://blog.csdn.net/huwei039/article/details/104269486)

## 容器vector的用法
```
size //当前使用数据的大小
empty //判断vector是否为空
vector<int> a(n,0)  // <int> 为数据类型，n为长度，0为初始化的值
push_back //从尾部加入一个新数据

对vector进行for循环： for(auto v : V)
```

## 常用基础操作

* 判断奇偶数：`number & 1` true=奇数，false=偶数

* 比较两个数的大小并返回较大的那个： `c = a>b?a:b` 或者 `c = max(a,b)` max函数需要algorithm头文件，后者更快
