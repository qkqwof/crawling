## Subquery 연습문제

---

#### 연습문제 1
HackerRank 
- Top Earners

``` sql
select months*salary as earnings, count(*)
from employee
group by earnings
having earnings = (select max(months*salary) from employee)
```

#### 연습문제 2
leetcode

##### 184. Department Highest Salary

- 내가 작성한 답
``` sql
select b.name as department, a.name as employee, a.salary
from employee a
join department b
on a.departmentid = b.id
group by department,employee
having salary in (select max(salary) 
                 from employee
                 group by departmentid
                )
```

- 강의에서 작성한 답
``` sql
select d.name as department, e.name as employee, e.salary
from employee as e
    inner join
        (-- 부서에서 가장 많이 벌 때에 그 임금과 부서id
        select departmentid, max(salary) as max_salary
        from employee
        group by departmentid
        ) as dh
        on e.departmentid = dh.departmentid
        and e.salary = dh.salary
    inner join department as d 
    on d.id = e.departmentid
```