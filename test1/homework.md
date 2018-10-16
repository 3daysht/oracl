
# 教程语句1
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
