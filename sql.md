### practice1

```sql
SELECT * FROM Employee where salary >6000 AND NAME LIKE '%n%'；
```

### practice2

```sql
SELECT
    Company.companyName
    , Employee.name
FROM
    employee_db.Company
    , employee_db.Employee
WHERE Employee.salary = (SELECT MAX(salary) FROM Employee WHERE Employee.companyId=Company.id) 
AND Employee.companyId=Company.id;
```

**扩展：求所有公司工资最高的人的姓名和公司名字**

```sql
SELECT
    Company.companyName
    , Employee.name
FROM
    employee_db.Company
    , employee_db.Employee
WHERE Employee.salary = (SELECT MAX(salary) FROM Employee) AND Employee.companyId=Company.id;
```

### practice3

参考：<https://stackoverflow.com/questions/46486047/finding-the-highest-average-salary-in-sql>

```sql
SELECT Company.companyName, AVG(Employee.salary)
FROM Company , Employee
WHERE Employee.companyId=Company.id
GROUP BY Company.companyName ORDER BY AVG(Employee.salary) DESC LIMIT 0,1;
```

上面只是其中一种解决办法！

还可以首先获得最大的平均工资然后.....

```sql
# 获得最大的平均工资
SELECT MAX(e.avgsal) FROM (SELECT companyId,AVG(salary) AS avgsal FROM Employee GROUP BY companyId) e
```

### pravtice4

```sql
SELECT Employee.id,Employee.name,Employee.gender,Employee.companyId,Employee.salary,Company.companyName,AVG(Employee.salary) 
FROM Employee,Company 
WHERE Employee.companyId=Company.id AND Employee.salary > (SELECT AVG(salary) FROM Employee WHERE Employee.companyId=Company.id)
GROUP BY Company.companyName;
 
```

上面这种写法通过 SQLyog 执行不会出现错误，但是通过命令行执行会出现错误，正确的写法应该是：

```sql
.......
GROUP BY Employee.id;
```

### 扩展

**求每个公司的平均工资**

```sql
SELECT  Employee.id,Employee.name,Employee.gender,Employee.companyId,Employee.salary,Company.companyName,AVG(Employee.salary) 
FROM Employee,Company 
WHERE Employee.companyId=Company.id
GROUP BY Company.id
```

