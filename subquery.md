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

#### 연습문제 3

HackerRank
    - challenges

``` sql
select a.hacker_id, a.name, count(*) as challenges_created
             from hackers a
             inner join challenges b
             on a.hacker_id = b.hacker_id
             group by a.hacker_id, a.name
             having challenges_created = (select max(challenges_created)
                                          from (select hacker_id, count(*)
                                                from challenges
                                                group by hacker_id) sub)
                                                or
                    challenges_created in (select challenges_created
                                           from(select hacker_id, count(*) as challenges_created
                                                from challenges
                                                group by hacker_id) sub
                    group by challenges_created
                    having count(*) = 1)
                    order by challenges_created desc, hacker_id
```

- 내가 쓴 풀이
``` sql
with counter as (
    select b.hacker_id, b.name, count(*) as challenges_created
    from challenges a
        inner join hackers b
        on a.hacker_id = b.hacker_id
    group by b.hacker_id, b.name
)

select counter.hacker_id, counter.name, counter.challenges_created
from counter
where challenges_created = (select max(challenges_created) from counter)
or challenges_created in (select challenges_created
                          from counter
                          group by challenges_created
                          having count(*) = 1)
group by counter.challenges_created desc, counter.hacker_id
```