
# My Admin & User Roles in E-learning platform DB
<a name='readme-top'></a>
<!-- TABLE OF CONTENTS -->
## 📗 Table of Contents
-   [📖 My Data Fundamentals:Admin & User Roles in E-learning platform DB](#about-project)
  - [🛠 Built With](#built-with)
- [💻 Getting Started](#getting-started)
  - [Samples SQL Queries & Policies](#samples-sql-queries)
  - [Security notes](#security-notes)
- [👥 Authors](#authors)
- [🔭 Future Features](#future-features)
- [🤝 Contributing](#contributing)
- [⭐️ Show your support](#support)
- [🙏 Acknowledgements](#acknowledgements)
 - [FAQ](#faq)
  
<!-- PROJECT DESCRIPTION -->
# 📖 Data Fundamentals final project-E-learning platform data base <a name="about-project"></a
💡 About the Project
This project models an E-Learning Platform database while demonstrating administrative and user security using Row Level Security (RLS) in Supabase (PostgreSQL).
## 🛠 Built With <a name="built-with"></a> 

Supabase Dashboard – SQL editor & authentication.

PostgreSQL – Database and tables.

RLS Policies & Functions – To enforce student/admin restrictions.
<!-- Features -->
### Key Features <a name="key-features"></a>
 -   Key Features Showcased:

✅ UUID-based User Authentication (linked with Supabase Auth).

✅ Admin vs. Student Roles with least privilege enforcement.

✅ Granular CRUD (Create, Read, Update, Delete) control via RLS policies.

✅ A tested SQL script to validate roles and policies.

✅ Output from Supabase based on roles and policies.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- GETTING STARTED -->
## 💻 Getting Started <a name="getting-started"></a>
Getting Started
### Prerequisites
- A Supabase account (https://supabase.com/)and project.
- PostgreSQL basics knowledge(https://WWW.W3schools.com/sql/)
-Git installed.



<!--###Setup -->
### Setup
<```clone this resipitory:>
<```https://github.com/silyvia?tab=repositories>
  

--->

Open your Supabase SQL Editor.

Run your schema SQL to create tables (users, courses, enrollments, submissions) and sample data.

Apply UUID & RLS Setup:
```sql
ALTER TABLE users ENABLE ROW LEVEL SECURITY;
ALTER TABLE courses ENABLE ROW LEVEL SECURITY;
ALTER TABLE enrollments ENABLE ROW LEVEL SECURITY;
ALTER TABLE submissions ENABLE ROW LEVEL SECURITY;
```
--->

Apply the User (Student) and Admin policies detailed below.

💻 Sample SQL Queries & Policies

1.User Policies (Student Role)
Students have restricted access, primarily focused on their own enrollment and submissions, and published course content.
```sql
- 1. Policy to allow a student to view their own enrollment records.
-- Enforcement: The user's UUID (auth.uid()) must match the user_uuid column in the enrollment record.
CREATE POLICY "students_can_view_own_enrollments"
ON public.enrollments
FOR SELECT
TO authenticated
USING (auth.uid() = user_uuid);
```
```sql
-- 2. Policy to allow a student to insert submissions linked to their own ID.
-- Enforcement: The new row being inserted must have a student_uuid that matches the user's UUID (auth.uid()).
CREATE POLICY "students_can_insert_own_submissions"
ON public.submissions
FOR INSERT
TO authenticated
WITH CHECK (auth.uid() = student_uuid);
```
```sql
-- 3. Policy to allow a student to read all published courses.
-- Enforcement: The course's 'is_published' column must be TRUE. No ownership check is required.
CREATE POLICY "students_can_read_published_courses"
ON public.courses
FOR SELECT
TO authenticated
USING (is_published = TRUE);

```
```sql
- Admins manage all enrollments 
CREATE POLICY "admins_manage_all_enrollments"
ON public.enrollments
FOR ALL
TO authenticated
USING (
  EXISTS (
    SELECT 1 FROM public.users 
    WHERE user_uuid = auth.uid() AND role = 'admin'
  )
);
```
```sql
-- Admins manage all course content 
CREATE POLICY "admins_manage_all_courses"
ON public.courses
FOR ALL
TO authenticated
USING (
  EXISTS (
    SELECT 1 FROM public.users 
    WHERE user_uuid = auth.uid() AND role = 'admin'
  )
);
```


<p align="right">(<a href="#readme-top">back to top</a>)</p>
<!-- AUTHORS -->
## 👥 Authors <Silyvia ongwae="authors"></a>
> Silyvia Ongwae
👤 **Author1**
- GitHub: [@githubhandle](https://github.com/silyvia/
<p align="right">(<a href="#readme-top">back to top</a>)</p>
<!-- FUTURE FEATURES -->
## 🔭 Future Features <a name="future-features"></a>

- **Add security**
-  **Link DB to R for visualisation purposes and further analyses**

<p align="right">(<a href="#readme-top">back to top</a>)</p>
<!-- CONTRIBUTING -->
## 🤝 Contributing <a name="contributing"></a>
<``Contributions, issues, and feature requests are welcome!>
Feel free to check the [issues page](../../issues/).
<p align="right">(<a href="#readme-top">back to top</a>)</p>
<!-- SUPPORT -->
## ⭐️ Show your support <a name="support"></a>
>If you find this schema useful for designing your E-learning applications,please give a repository star.
<p align="right">(<a href="#readme-top">back to top</a>)</p>
<!-- ACKNOWLEDGEMENTS -->
## 🙏 Acknowledgments <a name="acknowledgements"></a>
>I would like to thank my instructor and the Supabase team for providing the tools and guidance necessary for this project. 
<p align="right">(<a href="#readme-top">back to top</a>)</p>
  ## ❓FAQ (OPTIONAL) <a name="faq"></a>
- **Why is enrollment aseparate table?**
  <``This is necessary because the relationship between students and courses is Many-to-Many (one student takes many courses, and one course has many students). The enrollments table acts as a junction to manage this relationship.>
- **How can i see a student's full course load?]**
  - ** You must use a 3-table JOIN query between students, courses, and enrollments, linked by their respective IDs.
<p align="right">(<a href="#readme-top">back to top</a>)</p>




