# chapter14: Index

## 1 Basic Concepts

Index mechanisms used to speed up access to desired data.
索引用来加速查找。


**search Key**: attribute to set of attributes used to look up records in a file.
属性为用于查找文件中记录的属性集。

An **index file** consists of records (called index entries) of the form.
索引文件通常是有顺序的

### 1.1 Index Evaluation Metrics
- Access types supported efficiently
    - **Point query**: records with a specified value in the attribute.
    `点查询`
    - **Range query**: records with an attribute value falling in a specified range of values.
    `范围查询`
- Access time
- Insertion time
- Deletion time
- Space overhead

### 1.2 Ordered Indices
![alt text](image-250.png)

Primary index 是很宝贵的，只能有一个，其他都是辅助索引。

??? example
    ![alt text](image-251.png)
    ![alt text](image-252.png)


- **Dense index(稠密索引)** — Index record appears for every search-key value in the file.
所有 search-key 都要出现在索引文件里。
- **Sparse Index(稀疏索引)**: contains index records for only some search-key values.

??? example
    ![alt text](image-253.png)
    ![alt text](image-254.png)


!!! tip good tradeoff
    ![alt text](image-255.png)


### 1.3 Multilevel Index(多级索引)

![alt text](image-256.png)


## 2 B+-Tree Index
![alt text](image-257.png)
![alt text](image-258.png)
![alt text](image-259.png)
![alt text](image-260.png)


??? example
    ![alt text](image-261.png)


### 2.1 Observations about B+-trees
![alt text](image-262.png)


!!! note Query on B+-Tree
    ![alt text](image-263.png)
    ![alt text](image-264.png)


### 2.2 Insert of B+-Tree
!!! note Insert on B+-Trees
    ![alt text](image-265.png)
    ![alt text](image-266.png)


??? example of insert
    ![alt text](image-267.png)
    ![alt text](image-268.png)


### 2.3 Delete of B+-Tree
!!! note Delete on B+-Tree
    ![alt text](image-269.png)
    ![alt text](image-270.png)


??? example of delete
    ![alt text](image-271.png)
    ![alt text](image-272.png)
    ![alt text](image-273.png)


!!! warning B+-Tree: height and size estimation
    ![alt text](image-274.png)

### 2.4 other issues in indexing
- **Record relocation and secondary indices**
    - If a record moves, all secondary indices that store record pointers have to be updated如果一条记录移动，存储记录指针的所有二级索引都必须更新
    - Node splits in B+-tree file organizations become very expensive B+树文件组织中的节点拆分变得非常昂贵
    - Solution: use primary-index search key instead of record pointer in secondary index解决方案：在二级索引中使用主索引搜索键代替记录指针
- **Variable length strings as keys**
    - Variable fanout
- **Prefix compression**
    - Key values at internal nodes can be prefixes of full key内部节点的键值可以是全键的前缀
    - Keep enough characters to distinguish entries in the subtrees separated by the key value保留足够的字符来区分由键值分隔的子树中的条目


### 2.5 Multiple-Key Access
![alt text](image-275.png)


## 3 Bulk Loading and Bottom-up Build
![alt text](image-276.png)


??? example
    ![alt text](image-277.png)
    如果要排序的内容较大，无法放下内存，可以使用外部排序。
    fanout 可以计算出来。
    可以用 level-order 写到磁盘里，便于顺序访问所有索引，此时块是连续的。（便于顺序访问所有数据项）
    这里的代价就是建好后，一次 seek 后全部写出去 (9 blocks)


??? example2
    ![alt text](image-278.png)
    把刚刚那棵 B+ 树叶子节点（即遍历所有数据）需要 1seek+6blocks. 随后和上面的数据合并后，写回磁盘时需要 1seek+13blocks.


Merge two existing two B+-trees , to create a new B+-tree using the Bottom-UP Build algorithm, as in LSM-tree Index
假设有两棵这样生成的 B+ 树，将他们合并在一起。首先把叶子节点拿出来排序。


## 4 Indexing in Main Memory
![alt text](image-279.png)


## 5 Indexing on flash
![alt text](image-280.png)


### 5.1 LogStructured Merge(LSM) Tree
![alt text](image-281.png)

![alt text](image-282.png)

![alt text](image-283.png)
