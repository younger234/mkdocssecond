# SQL语言
详细一些的SQL教程在CSDN上，[click here](https://blog.csdn.net/m0_50546016/article/details/120070003?ops_request_misc=%257B%2522request%255Fid%2522%253A%252203b0f8b494da45639e76737e4ab34e4d%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=03b0f8b494da45639e76737e4ab34e4d&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-120070003-null-null.142^v101^pc_search_result_base9&utm_term=SQL&spm=1018.2226.3001.4187)
或者上菜鸟教程网站学习SQL语言，[click here](https://www.runoob.com/sql/sql-tutorial.html)


!!! tip 
    CSDN讲的详细，内容多，菜鸟教程更精简，方便




---
### 学生和俱乐部信息的关系模式：
- **student( sid, name, age, gender, department)**
- **club(cid, name, supervisor)**
- **member( sid, cid, date)**
### SQL查询：
#### (1) 查找计算机科学系参加舞蹈俱乐部的学生姓名
```sql
-- 使用自然连接
SELECT student.name
FROM student 
NATURAL JOIN member 
NATURAL JOIN club USING(cid)
WHERE department='CS' AND club.name='Dancing';
-- 另一种解决方案
SELECT student.name
FROM student, member, club
WHERE student.sid=member.sid AND 
      member.cid=club.cid AND
      department='CS' AND 
      club.name='Dancing';
```
#### (2) 查找参加由JL SUN监督的所有俱乐部的学生姓名
```sql
-- 使用自然连接和分组
SELECT student.name
FROM student 
NATURAL JOIN member 
NATURAL JOIN club USING(cid)
WHERE supervisor='JL Sun'
GROUP BY student.sid, student.name
HAVING COUNT(*) = (SELECT COUNT(*)
                   FROM club
                   WHERE supervisor='JL Sun');
-- 另一种解决方案
SELECT S.name
FROM student S
WHERE NOT EXISTS
  (    (SELECT cid
        FROM club
        WHERE supervisor='JL Sun')         
      EXCEPT
      (SELECT cid
       FROM member 
       NATURAL JOIN club
       WHERE club.supervisor='JL Sun' AND member.sid=S.sid)
  );
```
#### (3) 查找只包含女生的俱乐部
```sql
-- 使用NOT EXISTS
SELECT cid
FROM club
WHERE NOT EXISTS(
  (SELECT *
   FROM student 
   NATURAL JOIN member
   WHERE student.gender='M' AND 
         member.cid=club.cid)
);
-- 另一种解决方案
SELECT cid 
FROM student 
NATURAL JOIN member
WHERE student.gender='F'
GROUP BY cid
HAVING COUNT(*)= (SELECT COUNT(*)
                  FROM member
                  WHERE member.cid=cid);
```
#### (4) 对于每个系，找出加入任何俱乐部的学生百分比
```sql
SELECT department, 
       COUNT(DISTINCT member.sid) / COUNT(DISTINCT student.sid) * 100 AS percentage
FROM student 
NATURAL LEFT OUTER JOIN member
GROUP BY department;
```
#### (5) 找出年龄差异最大的两名学生
```sql
-- 使用子查询
SELECT s1.sid, s2.sid
FROM student AS s1, student AS s2
WHERE s1.age - s2.age >= ALL
      (SELECT s3.age - s4.age
       FROM student AS s3, student AS s4);
-- 另一种解决方案
SELECT s1.sid, s2.sid
FROM student AS s1, student AS s2
ORDER BY s1.age - s2.age DESC
LIMIT 1;
```
---
以上是Markdown格式的SQL查询内容，每个查询后面都有对应的SQL代码块。


