# 实验1：SQL语句的执行计划分析与优化指导

## 实验分析：
 - 分析SQL执行计划，执行SQL语句的优化指导。
 - 理解分析SQL语句的执行计划的重要作用。
 
## 实验过程：

查询1：

```SQL
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
from hr.departments d，hr.employees e
where d.department_id = e.department_id
and d.department_name in ('IT'，'Sales')
GROUP BY department_name;
```

查询2：
```SQL
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
FROM hr.departments d，hr.employees e
WHERE d.department_id = e.department_id
GROUP BY department_name
HAVING d.department_name in ('IT'，'Sales');
```

## 总结：
查询二的语句最优
having只能用在group by之后，对分组后的结果进行筛选(即使用having的前提条件是分组)
第二个有优化指导所以第二个的效率高
改进能在之前添加索引

```sql
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
FROM hr.departments d，hr.employees e
WHERE d.department_id = e.department_id
GROUP BY department_name
HAVING d.department_name in ('IT'，'Sales');
```
