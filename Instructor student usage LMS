with person_data as (
  select
    case 
      when lpc.course_role = 'I' then 'Instructor'
      when lpc.course_role = 'S' then 'Student'
      else 'Other'
    end as role,
    concat(lp.first_name, ' ', lp.last_name) as name,
    lp.email,
    lpc.course_id,
    count(distinct lca.id) as accesses,
    sum(lca.duration_sum)/60 as minutes,
    sum(lca.interaction_cnt) as interactions
  from cdm_lms.person lp
  inner join cdm_lms.person_course lpc
    on lpc.person_id = lp.id
  inner join cdm_lms.course_activity lca
    on lca.person_course_id = lpc.id
  where lca.first_accessed_time >= '2023-06-01 00:00:00'
    and lca.first_accessed_time < '2025-01-01 00:00:00' -- Ensures data only from last year -ish
    and lpc.course_role in ('I', 'S') -- Include only instructors and students
  group by
    lpc.course_role,
    lp.first_name,
    lp.last_name,
    lp.email,
    lpc.course_id
  order by role, name, lpc.course_id
)

select
  role,
  name,
  email,
  count(distinct course_id) as "Course Count",
  round(avg(accesses), 0) as "Avg. Accesses",
  round(sum(minutes), 0) as "Total Minutes", 
  round(avg(minutes), 0) as "Avg. Minutes",
  round(avg(interactions), 0) as "Avg. Interactions"
from person_data
group by 
  role,
  name,
  email
order by
  role,
  round(sum(minutes), 0) desc; 
