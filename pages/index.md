# HR Dashboard 

```sql MIN_salary
select min(salary) as min_salary from hr_dashboard.hr_data1 where Country = '${inputs.Country.value}'
```
```sql MAX_salary
select max(salary) as max_salary from hr_dashboard.hr_data1 where Country = '${inputs.Country.value}'
```
```sql AVG_SALARY
select Avg(salary) as avg_salary from hr_dashboard.hr_data1 where Country = '${inputs.Country.value}'
```
```sql country_filter
select Country from hr_dashboard.hr_data1
group by 1
```
```sql head_count 
select count(*) as head_count,Gender,Department from hr_dashboard.hr_data1 where Country = '${inputs.Country.value}'
group by 2,3
``` 
```sql ages
select Age from hr_dashboard.hr_data1 where Country = '${inputs.Country.value}'
group by 1
````
```sql age_bins 
   SELECT 
    count(*) as Head_Count,
    CASE 
        WHEN age >= 5 AND age < 10 THEN '5-9'
        WHEN age >= 10 AND age < 15 THEN '10-14'
        WHEN age >= 15 AND age < 20 THEN '15-19'
        WHEN age >= 20 AND age < 30 THEN '20-29'
        WHEN age >= 30 AND age < 35 THEN '30-34'
        WHEN age >= 35 AND age < 40 THEN '35-39'
        WHEN age >= 40 AND age < 45 THEN '40-44'
        ELSE '45+'
    END AS age_bin
FROM hr_dashboard.hr_data1
where Country = '${inputs.Country.value}'
group by 2
order by 2 asc
```

```sql head_count_by_years
select date_trunc('year' , "Date Joined") as year,
count(*) as head_count 
from hr_dashboard.hr_data1
where Country = '${inputs.Country.value}'
group by 1;
```
```sql rating 
select Rating,count(*) as head_count 
from hr_dashboard.hr_data1
where Country = '${inputs.Country.value}'
group by 1
order by 1 asc
```

<style>
.bar-chart-container {
    border: 2px solid black;
    height:95%;
    width: 100%;
    max-width: 1000px;
    margin: auto;
}
</style>

<BigValue 
  data={MIN_salary} 
  value=min_salary
  fmt='num0k'
/>


<BigValue 
  data={MAX_salary} 
  value=max_salary
  fmt='num0k'
/>
<BigValue 
  data={AVG_SALARY} 
  value=avg_salary
  fmt='num0k'
/>

<Dropdown 
    data={country_filter} 
    name=Country 
    value=Country 
    title="Country" 
/>
<Grid cols=2>
<div class="bar-chart-container">
<BarChart 
    data={head_count}
    x=Department
    y=head_count
    series = Gender
    type=grouped
     colorPalette={[
        '#009292',
        '#074652',
        'fe6db6'
     ]}
     labels=true
     title = 'Head Count By Department and Gender'
/>
</div>
<div class="bar-chart-container">
<BarChart 
    data={age_bins}
    x=age_bin
    y=Head_Count
    colorPalette={[
        '#009292',
        '#074652'
    ]}
    labels=true
    title= 'Head Count By Ages'
/>
</div>
</Grid>
<Grid cols=2>
<div class="bar-chart-container">
<LineChart 
    data={head_count_by_years}
    x=year
    y=head_count
    colorPalette={['#074652']}
    title="Sales per Year"
    labels=true
/>
</div>
<div class="bar-chart-container">
<BarChart 
    data={rating}
    x=Rating
    y=head_count
    colorPalette={['#074652']}
    labels=true
    title = 'Head Count By Rating'
/>
</div>
</Grid>