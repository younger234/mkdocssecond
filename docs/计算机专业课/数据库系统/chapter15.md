# chapter15: Query Processing

!!! abstract
    ![alt text](image-284.png)


## 1 Basic Steps in Query Processing
![alt text](image-285.png)

经过语法分析、语义检查翻译成关系表达式，经过查询优化转化成执行计划（目标代码），由求值引擎得到输出。


??? example
    ![alt text](image-286.png)
    ![alt text](image-287.png)


![alt text](image-288.png)

## 2 Measures Of Query Cost
![alt text](image-289.png)


## 3 Selection Operation

### 3.1 file scan
![alt text](image-290.png)

### 3.2 index scan
![alt text](image-291.png)

![alt text](image-292.png)

![alt text](image-293.png)

![alt text](image-294.png)

![alt text](image-295.png)

![alt text](image-296.png)

![alt text](image-297.png)

![alt text](image-298.png)

### 3.3 Bitmap Index Scan
![alt text](image-299.png)

## 4 Sorting
- We may build an index on the relation, and then use the index to read the relation in sorted order.  May lead to one disk block access for each tuple.
- For relations that fit in memory, techniques like quicksort can be used.  
- For relations that don’t fit in memory,**external sort-merge** is a good choice.

??? example
    ![alt text](image-300.png)
    ![alt text](image-301.png)


### 4.1 Procedure
![alt text](image-302.png)
![alt text](image-303.png)


### 4.2 Cost Analysis
![alt text](image-304.png)
![alt text](image-305.png)


### 4.3 New Version
![alt text](image-306.png)
![alt text](image-307.png)


## 5 Join Operation


### 5.1 Nested-Loop Join
![alt text](image-308.png)
![alt text](image-309.png)

### 5.2 Block Nested-Loop Join
![alt text](image-310.png)
![alt text](image-311.png)
![alt text](image-312.png)


### 5.3 Indexed Nested-Loop Join
![alt text](image-313.png)


!!! example
    ![alt text](image-314.png)


### 5.4 Merge-Join
![alt text](image-315.png)
![alt text](image-316.png)
![alt text](image-317.png)
![alt text](image-318.png)


### 5.5 Hash-Join
![alt text](image-319.png)
![alt text](image-320.png)
![alt text](image-321.png)
![alt text](image-322.png)


??? Cost Of Hash-Join
    ![alt text](image-327.png)


??? Example
    ![alt text](image-328.png)


#### 5.5.1 Recursive partitioning
![alt text](image-323.png)
![alt text](image-324.png)


#### 5.5.2 Cost Of Hash-Join
![alt text](image-325.png)
![alt text](image-326.png)