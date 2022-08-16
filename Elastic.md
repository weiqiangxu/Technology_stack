# Elastic面试题

https://blog.csdn.net/wlei0618/article/details/124162151

https://zhuanlan.zhihu.com/p/265399976

Elastic面试题128道题

https://zhuanlan.zhihu.com/p/400387347

# 文字简述

倒排索引原理

https://baike.baidu.com/item/%E5%80%92%E6%8E%92%E7%B4%A2%E5%BC%95/11001569

http://zhhz.suda.edu.cn/aa/a9/c3892a43689/page.psp

倒排索引 = 词典 + 倒排链
词典（记录词条项和对应倒排链指针）本身通常是利用Hash表或者二叉搜索树形结构实现
倒排链（存储所有文档ID列表）

比如搜索一个关键字，找出包含关键字的文章：

正向索引：扫描索引库的文档，然后从文档之中查找关键词； 文档 - 单词
反向索引（倒排索引）：将所有关键词load出来独立存储，将这些关键词存储它们与原文档的关联关系

存储上不一致，原来的关键字是存储在文档里面，找到文档再找关键字；
而现在关键字解析后独立存储到另一个地方，这个地方叫词典，然后记录关键字与文档的关联关系

哈希 - 散列算法（变长转定长）

### Elastic常见的字段类型

keyword [integer long byte float] date date_nanos text [需要注意keyword和text的区别]

### 常见的查询选项

[query string] 
全文检索[query]
精准查询[term query]

