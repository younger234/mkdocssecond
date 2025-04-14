# Chapter7: Relational Database Design

## 1 Relational Database Design

### 1.1 Introduction

??? example
    ![alt text](image-211.png)
    ![alt text](image-212.png)


Pitfalls of the “bad” relations
- Information repetition (信息重复)
- Insertion anomalies (插入异常)
- Update difficulty (更新困难)

- 数据之间存在着隐含的函数约束关系，知道了 id 就可以决定其他元素。 
- e.g. id -> name, salary, dept_name;
- dept_name ->  building, budget

- 产生冗余的原因是 dept_name 决定了部分属性，但他却不是这个表的 `primary key`.
`好的关系`：只有 `candidate key` 能决定其他属性。
拆表后要有重叠的属性，否则无法拼接回去。这里的**公共属性必须是分拆出一个关系模式的 primary key**, 这是无损（没有信息损失）连接。


!!! example  "lossy decomposition"
    ![alt text](image-213.png)
    employee(ID,name,street,city,salary)->employee1(ID,name) and employee2(name,street,city,salary)


???  example "examples of lossless-join decomposition"
    ![alt text](image-214.png)

#### 1.1.1 Lossless-join Decomposition
![alt text](image-215.png)


### 1.2 Goal-Devise a theory for the following
![alt text](image-216.png)


## 2 Functional Dependencies
![alt text](image-217.png)
![alt text](image-218.png)
![alt text](image-219.png)

### 2.1 Closure(闭包)

#### 2.1.1 Closure of a Set of Functional Dependencies
![alt text](image-220.png)
![alt text](image-221.png)

??? "additional rules"
    ![alt text](image-222.png)
    ![alt text](image-223.png)


??? example
    ![alt text](image-224.png)


#### 2.1.2 Closure of Attribute Sets
![alt text](image-225.png)

??? example
    ![alt text](image-226.png)

#### 2.1.3 Uses of Attribute Closure
![alt text](image-227.png)

??? example "use of attribute closure"
    ![alt text](image-228.png)


### 2.2 Canonical Cover(正则覆盖)
a **canonical cover** of F is a “minimal” set of functional dependencies equivalent to F, having no redundant dependencies or redundant parts of dependencies.F的正则覆盖是等价于F的函数依赖的“最小”集，没有冗余依赖或冗余部分的依赖。

??? example 
    ![alt text](image-229.png)

![alt text](image-230.png)


??? "Computing a Canonical Cover"
    ![alt text](image-231.png)
    ![alt text](image-232.png)


??? "Exercise 2"
    ![alt text](image-233.png)


### 2.3 Boyce-Codd Normal FormAZSX
![alt text](image-234.png)

#### 2.3.1 Decomposing a Schema into BCNF
![alt text](image-235.png)
![alt text](image-236.png)

??? "Exercise 3"
    ![alt text](image-237.png)

#### 2.3.2 Dependency Preservation
![alt text](image-238.png)

??? "example1"
    ![alt text](image-239.png)


??? "example2"
    ![alt text](image-240.png)
    ![alt text](image-241.png)


???+ "BCNF and Dependency Preservation"
    ![alt text](image-242.png)

### 2.4 Third Normal Form

??? "motivation"
    ![alt text](image-243.png)
    - 在某些情况下，BCNF不保留依赖关系，有效检查更新上的FD违反是很重要的 
    - 解决方案：定义一个较弱的范式，称为第三范式（3NF） 允许一些冗余 但是可以在不计算连接的情况下检查单个关系的功能依赖。总是有一个无损连接、保持依赖关系的3NF分解。


![alt text](image-244.png)
任何一个非平凡函数依赖，如果左边不是一个 `super key`, 那么右边必须包含在一个 `candidate key` 里面。

??? example
    ![alt text](image-245.png)


!!! tip 
    ![alt text](image-246.png)

**Goals of Normalization**
![alt text](image-247.png)
设R是一个函数依赖集F的关系格式。判断关系方案R是否处于“良好”形式。在关系方案R不是“好”形式的情况下，将其分解为关系方案{R1， R2，…， Rn}使得每个关系方案都是良好的形式（即，BCNF或3NF） 分解是一个无损连接分解 最好是保持依赖关系的分解。

??? example "E-R Modeling and Normal Forms"
    ![alt text](image-248.png)
    ![alt text](image-249.png)
    这里的无损分解，先指定一个路径，考虑每两个关系直接是否无损(公共属性是否为其中一个关系的 key)

## 3 Multivalued Dependencies


