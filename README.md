# Pewlett-Hackard-Analysis.

## Overvie of Pewlett-Hackard-Analysis.

Theres a lot of employees that are going to be retired soon. So the company needs to think on this people and which positions are needed to be fulfill in the future. We are using CSV files and SQL to find the response to this questions

### Purpose

We need to find the answers to the questions "Who's gonna a be retired and how many positions will be available?"

## Pewlett-Hackard Result

- For the first analysis, we create a query to find the retirement information of the people that born between 1952 and 1955. 

![image](https://user-images.githubusercontent.com/88845919/139559532-fcb94f18-0b60-4bdc-9520-604cff41ace7.png)

    --Create retirements titles table
    SELECT e.emp_no,
      e.first_name,
      e.last_name,
      ti.title,
      ti.from_date,
      ti.to_date
    INTO retirement_titles
    FROM employees as e
    Inner Join titles as ti
    ON (e.emp_no = ti.emp_no)
    WHERE (e.birth_date BETWEEN '1952-01-01' AND '1955-12-31')
    ORDER BY emp_no;

- Then we filter this info to eliminate duplicate values. This give us an unique employee list.

![image](https://user-images.githubusercontent.com/88845919/139559562-48c335d2-6455-4855-9911-c8de26d6cd4d.png)

    -- Use Dictinct with Orderby to remove duplicate rows
    SELECT DISTINCT ON (emp_no) emp_no,
    first_name,
    last_name,
    title
    INTO unique_titles
    FROM retirement_titles
    ORDER BY emp_no, to_date DESC;

- For the next action we look for the count of employees by title. This in order to find the positions that need to be fulfill.

![image](https://user-images.githubusercontent.com/88845919/139559587-3567b83e-0b27-46ca-94f5-d4aaf3fd20dc.png)


    --Create retiring titles count
    SELECT COUNT (ut.emp_no), ut.title
    INTO retiring_titles
    FROM unique_titles as ut
    group by ut.title
    order by count desc;
    
- For the second part. We find the employees that are eligible to participate in a mentorship program.

![image](https://user-images.githubusercontent.com/88845919/139559663-e6d7d8d6-f1ca-44c0-9e58-d9ba65eb3980.png)

    SELECT DISTINCT ON (emp_no) e.emp_no,
      e.first_name,
      e.last_name,
      e.birth_date,
      de.from_date,
      de.to_date,
      ti.title
    INTO mentorship_eligibilty
    FROM employees as e
    Inner Join dept_emp as de
    ON e.emp_no = de.emp_no
    Inner join titles as ti
    ON (e.emp_no = ti.emp_no)
    WHERE (de.to_date = '9999-01-01') AND
    (e.birth_date BETWEEN '1965-01-01' AND '1965-12-31')
    ORDER BY emp_no;
    
### Summary

- How many roles will need to be filled as the "silver tsunami" begins to make an impact?

There are 7 different titles to considere. The following image shows the anount of people by title of this silver tsunami. It's a sum of 90,398.

    select sum(retiring_titles.count)
    from retiring_titles;

![retiring_titles](https://user-images.githubusercontent.com/88845919/139559947-b262532e-db22-449c-9862-ac08ed69fc4a.PNG)

- Are there enough qualified, retirement-ready employees in the departments to mentor the next generation of Pewlett Hackard employees?

The total number of employees that are eligible to participate in a mentorship program is 1,549. So there's no ehought people to mentor the next generation.

    select count(mentorship_eligibilty.emp_no)
    from mentorship_eligibilty;

![mentorship_eligibilty](https://user-images.githubusercontent.com/88845919/139560002-27b7b91b-9a66-4633-b466-6e48c1aabd8a.PNG)
