# Table 1: JobOpenings
# Table 2: Applicants

![tablestructure](https://github.com/NootanVijapure/subqueries_part3/assets/30225165/72f6ba1d-dbca-4bc6-aa11-f34cbbf14fb3)

# data entry sample data
![dataentrytable1](https://github.com/NootanVijapure/subqueries_part3/assets/30225165/c91699b4-e1ba-4b55-bd64-f4ac7dbd3417)
![dataentrytable2](https://github.com/NootanVijapure/subqueries_part3/assets/30225165/c6250e16-afa8-402e-81ce-36e5d2c1e626)


## 1.List All Job Openings:
 Retrieve a list of all available job openings, including details about the title, department, minimum experience, and minimum education.
## 2. Applicants and Their Jobs:
Display the names of applicants and the jobs they have applied for. Include all applicants, even if they haven't applied for any job.
## 3. Qualified Applicants:
Create a report showing the names of applicants who meet or exceed the minimum experience and education requirements for the jobs they have applied for.
## 4. Job Applicants Without Match:
Identify applicants who have applied for a job that currently has no opening.
## 5.Experience Range Search:
Find all job openings that require a minimum of 3 to 5 years of experience.
## 6. Education Level Analysis:
Retrieve a list of job titles and the count of applicants who have applied for each job, grouped by education level (e.g., Bachelor's, Master's, PhD).

```SQL
-- 1.
SELECT * FROM jobopenings;

-- 2.
SELECT 
	apl.applicantid,
	apl.applicantname,
	jo.jobid,
	jo.jobtitle
FROM applicants apl
JOIN jobopenings jo ON apl.appliedforjobid = jo.jobid
GROUP BY apl.applicantid, apl.applicantname, jo.jobid, jo.jobtitle
ORDER BY apl.applicantname ASC;

-- 3.

SELECT 
    apl.applicantid,
    apl.applicantname,
    apl.experienceyears,
    apl.education
FROM 
    applicants apl
JOIN 
    jobopenings jo ON apl.appliedforjobid = jo.jobid
WHERE 
    CASE 
        WHEN POSITION('Master' IN apl.education) > 0 THEN 'Master'
        WHEN POSITION('PhD' IN apl.education) > 0 THEN 'PhD'
        WHEN POSITION('Bachelor' IN apl.education) > 0 THEN 'Bachelor'
    END
    = ANY(ARRAY['PhD', 'Bachelor', 'Master'])
    AND apl.experienceyears >= jo.minimumexperience;

-- 4.

SELECT
    *
FROM 
    applicants apl
LEFT JOIN 
    jobopenings jo ON apl.appliedforjobid = jo.jobid
WHERE
	jo.jobid IS NULL;
	
-- 5.
SELECT
	*
FROM 
	jobopenings jo
WHERE
	jo.minimumexperience BETWEEN 3 AND 5;
	
-- 6.
SELECT
    jo.jobtitle,
    apl.education,
    COUNT(apl.applicantid) AS applicant_count
FROM
    jobopenings jo
LEFT JOIN
    applicants apl ON jo.jobid = apl.appliedforjobid
GROUP BY
    jo.jobtitle,
    apl.education;
```
