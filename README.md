
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
# 📖 Data Fundamentals final project-E-learning platform data base <a name="about-project"></a>
This project models a **E-learning platform database**while demonstrating **admin and user roles** using  **Roles Level Security (RSL)** in supabase.
## 🛠 Built With <a name="built-with"></a>   
<!-- Features -->
### Key Features <a name="key-features"></a>
- []**Tables**
- []**Schema**
- []**Acess control**
<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- GETTING STARTED -->
## 💻 Getting Started <a name="getting-started"></a>
To rebuild DB,follow these steps.

### Prerequisites
 To run this project, you need:
- [supabase account](https://supabase.com/)
- [knowledge of SQL commands](https://WWW.W3schools.com/sql/)
- A schema for creating tables in the DB.

<!--###Setup -->
### Setup
<```clone this resipitory:>
<```https://github.com/silyvia?tab=repositories>
  

--->
### Install
Install this project with:
<```Provision Database: Create a new project instance within the Supabase dashboard.>

<```Locate Script: The full database schema and sample data are contained in the schema.sql file.>

--->
<!--###DB Creation--->
### DB Schema
To run the project, execute the following command:
```sql
-- E-learning Platform Database Schema (Week 1)
-- Tables: students, courses, enrollments

-- 1. Create the students Table
-- Stores user/student profile data.
CREATE TABLE public.students (
    student_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    student_name TEXT NOT NULL,
    email TEXT UNIQUE NOT NULL,
    major TEXT, -- e.g., 'Computer Science', 'Data Analytics'
    registration_date DATE DEFAULT CURRENT_DATE
);
```

-- Insert 6 rows of sample student data
INSERT INTO public.students (student_name, email, major)
VALUES 
    ('Sarah Kamau', 'sarah.k@email.com', 'Data Analytics'),
    ('David Omondi', 'david.o@email.com', 'Web Development'),
    ('Aisha Gichuru', 'aisha.g@email.com', 'Marketing'),
    ('John Mburu', 'john.m@email.com', 'Computer Science'),
    ('Emily Wanjiku', 'emily.w@email.com', 'Data Analytics'),
    ('Peter Ndungâ€™u', 'peter.n@email.com', 'Web Development');

```
```sql
-- 2. Create the courses Table
-- Stores the course catalog.
CREATE TABLE public.courses (
    course_id INT PRIMARY KEY,
    course_name TEXT UNIQUE NOT NULL, -- e.g., 'Intro to SQL', 'Advanced Python'
    instructor TEXT NOT NULL,
    credits NUMERIC(2, 1) -- Course credit hours (e.g., 3.0)
);

-- Insert 6 rows of sample course data
INSERT INTO public.courses (course_id, course_name, instructor, credits)
VALUES 
    (101, 'Introduction to SQL', 'Dr. Mwangi', 3.0),
    (102, 'Data Modeling Fundamentals', 'Prof. Adisa', 4.0),
    (103, 'Python for Beginners', 'Ms. Chepkoech', 3.0),
    (104, 'Cloud Computing Basics', 'Mr. Kioko', 2.0),
    (105, 'Web Design Principles', 'Dr. Mwangi', 3.0),
    (106, 'Advanced Data Visualization', 'Prof. Adisa', 4.0);
```
```sql

-- 3. Create the enrollments Table (The central linking table)
-- Records which student is taking which course.
CREATE TABLE public.enrollments (
    enrollment_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    
    -- FOREIGN KEY 1: Links to the student
    student_id UUID REFERENCES public.students(student_id) ON DELETE CASCADE, 
    
    -- FOREIGN KEY 2: Links to the course
    course_id INT REFERENCES public.courses(course_id) ON DELETE RESTRICT,
    
    enrollment_date DATE DEFAULT CURRENT_DATE,
    grade_received TEXT -- e.g., 'A', 'B+', 'In Progress'
);

-- Insert 6 rows of sample enrollment data
INSERT INTO public.enrollments (student_id, course_id, grade_received)
VALUES 
    ((SELECT student_id FROM public.students WHERE student_name = 'Sarah Kamau'), (SELECT course_id FROM public.courses WHERE course_name = 'Data Modeling Fundamentals'), 'B+'),
    ((SELECT student_id FROM public.students WHERE student_name = 'David Omondi'), (SELECT course_id FROM public.courses WHERE course_name = 'Web Design Principles'), 'A'),
    ((SELECT student_id FROM public.students WHERE student_name = 'Aisha Gichuru'), (SELECT course_id FROM public.courses WHERE course_name = 'Introduction to SQL'), 'A-'),
    ((SELECT student_id FROM public.students WHERE student_name = 'John Mburu'), (SELECT course_id FROM public.courses WHERE course_name = 'Cloud Computing Basics'), 'In Progress'),
    ((SELECT student_id FROM public.students WHERE student_name = 'Sarah Kamau'), (SELECT course_id FROM public.courses WHERE course_name = 'Advanced Data Visualization'), 'B'),
    ((SELECT student_id FROM public.students WHERE student_name = 'Peter Ndungâ€™u'), (SELECT course_id FROM public.courses WHERE course_name = 'Python for Beginners'), 'In Progress');

   
```
-The tables should look like this in Supabase:
Enrollment
<img width="1912" height="835" alt="image" src="https://github.com/user-attachments/assets/aff01f98-52d9-4906-ac4e-b41183a9ec36" />

courses
<img width="1911" height="782" alt="image" src="https://github.com/user-attachments/assets/bee139b6-0c64-40a4-95ed-b305ad861b36" />

Students

<img width="1916" height="755" alt="image" src="https://github.com/user-attachments/assets/4316746e-a05b-4bed-9ee5-df935ac733e0" />


-To test the tables, I used two queries:
```sql
 select * from students
```
```sql
select*
from students
where major='Data analytics'
```
-Here are results of the queries:
**All students**
<img width="1576" height="705" alt="image" src="https://github.com/user-attachments/assets/a3beb5df-857a-4e94-9f0e-f6f24414a2ef" />

**All students majoring in Data Analytics**
<img width="1898" height="568" alt="image" src="https://github.com/user-attachments/assets/5d9e427f-7fad-40ed-a67a-6538e52e4037" />


### ERD DIAGRAM
-The ERD screenshot from Supabase looks like this:

<img width="915" height="580" alt="image" src="https://github.com/user-attachments/assets/27028d25-16e1-44a5-b460-6e4e9e889933" />


 
```
 -->
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




