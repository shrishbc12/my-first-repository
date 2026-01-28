
# Database Creation and Normalization
A laboratory demonstration on database creation and normalization (1NF, 2NF, 3NF) using **MYSQL** in Docker containers. This project simulates students information management system where students enrolled courses as well as grades can be accessed.

## Overview
- **Docker deployment**: Deployment of MYSQL inside a containerized environment.
- **Data Normalization**: Transforming the given data through normalization (1NF, 2NF, 3NF).
- **Real World Scenario**: Student data management system. 

## Quick Start
1.**Clone the Repository**
```
git clone https://github.com/Prasant01shrest/Normalization.git
```

2.**Start MYSQL Docker Container**
```
docker run --name Student_manage \
  -e MYSQL_ROOT_PASSWORD=root \
  -d -p 3306:3306 \
  mysql:8.0
```

3.***Wait for MYSQL to Initialize***

### 1NF Normalization
```
#Execute the Script
docker exec -it student_manage mysql -uroot -proot < Normalization/1NF

#Table Description
DESCRIBE students;

+-------------+-------------+------+-----+---------+-------+
| Field       | Type        | Null | Key | Default | Extra |
+-------------+-------------+------+-----+---------+-------+
| StudentID   | varchar(50) | NO   |     | NULL    |       |
| Name        | varchar(50) | NO   |     | NULL    |       |
| Email       | varchar(50) | NO   |     | NULL    |       |
| Major       | varchar(50) | NO   |     | NULL    |       |
| MajorHead   | varchar(50) | YES  |     | NULL    |       |
| CourseID    | varchar(50) | YES  |     | NULL    |       |
| CourseTitle | varchar(50) | NO   |     | NULL    |       |
| Credit      | int         | YES  |     | NULL    |       |
| Grade       | char(1)     | YES  |     | NULL    |       |
| Building    | varchar(50) | YES  |     | NULL    |       |
| Room        | varchar(50) | YES  |     | NULL    |       |
+-------------+-------------+------+-----+---------+-------+

#Verification Query
SELECT *FROM students
```

### 2NF Normalization
```
#Execute the Script
docker exec -it student_manage mysql -uroot -proot < Normalization/2NF

#Table Dscription
DESCRIBE students;

+------------+-------------+------+-----+---------+-------+
| Field      | Type        | Null | Key | Default | Extra |
+------------+-------------+------+-----+---------+-------+
| Student_ID | varchar(50) | NO   | PRI | NULL    |       |
| Name       | varchar(50) | NO   |     | NULL    |       |
| Email      | varchar(50) | NO   | UNI | NULL    |       |
| Major      | varchar(50) | NO   |     | NULL    |       |
| MajorHead  | varchar(50) | NO   |     | NULL    |       |
+------------+-------------+------+-----+---------+-------+

DESCRIBE courses;
+-------------+-------------+------+-----+---------+-------+
| Field       | Type        | Null | Key | Default | Extra |
+-------------+-------------+------+-----+---------+-------+
| CourseID    | varchar(50) | NO   | PRI | NULL    |       |
| CourseTitle | varchar(50) | NO   |     | NULL    |       |
| Credit      | int         | NO   |     | NULL    |       |
| Building    | varchar(50) | YES  |     | NULL    |       |
| Room        | varchar(50) | YES  |     | NULL    |       |
+-------------+-------------+------+-----+---------+-------+

DESCRIBE grade;
+------------+-------------+------+-----+---------+-------+
| Field      | Type        | Null | Key | Default | Extra |
+------------+-------------+------+-----+---------+-------+
| Student_ID | varchar(50) | NO   |     | NULL    |       |
| CourseID   | varchar(50) | NO   |     | NULL    |       |
| Grade      | char(1)     | YES  |     | NULL    |       |
+------------+-------------+------+-----+---------+-------+

#Verification Query
SELECT
       s.Student_ID,
       s.Name,
       s.Email,
       s.Major,
       s.MajorHead,
       c.CourseID,
       c.CourseTitle,
       c.Credit,
       g.Grade,
       c.Building,
       c.Room
   FROM students s
   JOIN grade g ON s.Student_ID = g.Student_ID
   JOIN courses c ON g.CourseID = c.CourseID;
```

### 3NF Normalization
```
#Execute the Script
docker exec -it student_manage mysql -uroot -proot < Normalization/3NF

#Table Description
DESCRIBE Majors;
+---------+--------------+------+-----+---------+-------+
| Field   | Type         | Null | Key | Default | Extra |
+---------+--------------+------+-----+---------+-------+
| Major   | varchar(50)  | NO   | PRI | NULL    |       |
| Advisor | varchar(100) | NO   |     | NULL    |       |
+---------+--------------+------+-----+---------+-------+

 DESCRIBE Students;
+-----------+--------------+------+-----+---------+-------+
| Field     | Type         | Null | Key | Default | Extra |
+-----------+--------------+------+-----+---------+-------+
| StudentID | varchar(10)  | NO   | PRI | NULL    |       |
| Name      | varchar(100) | NO   |     | NULL    |       |
| Email     | varchar(100) | NO   | UNI | NULL    |       |
| Major     | varchar(50)  | YES  | MUL | NULL    |       |
+-----------+--------------+------+-----+---------+-------+

 DESCRIBE Courses;
+-------------+--------------+------+-----+---------+-------+
| Field       | Type         | Null | Key | Default | Extra |
+-------------+--------------+------+-----+---------+-------+
| CourseID    | varchar(10)  | NO   | PRI | NULL    |       |
| CourseTitle | varchar(100) | NO   |     | NULL    |       |
| Credits     | int          | NO   |     | NULL    |       |
| Building    | varchar(50)  | YES  |     | NULL    |       |
| Room        | varchar(10)  | YES  |     | NULL    |       |
+-------------+--------------+------+-----+---------+-------+

DESCRIBE Grade;
+-----------+-------------+------+-----+---------+-------+
| Field     | Type        | Null | Key | Default | Extra |
+-----------+-------------+------+-----+---------+-------+
| StudentID | varchar(10) | NO   | PRI | NULL    |       |
| CourseID  | varchar(10) | NO   | PRI | NULL    |       |
| Grade     | char(1)     | YES  |     | NULL    |       |
+-----------+-------------+------+-----+---------+-------+

#Verification Query
SELECT
    s.StudentID,
    s.Name,
    s.Email,
    s.Major,
    m.Advisor,
    c.CourseID,
    c.CourseTitle,
    c.Credits,
    e.Grade,
    c.Building,
    c.Room
FROM Students s
JOIN Majors m ON s.Major = m.Major
JOIN Grade e ON s.StudentID = e.
StudentID
JOIN Courses c ON e.CourseID = c.CourseID;
```

### Project Workflow
```
Normalization/
│
├── MYSQL/
│   ├── 1NF.sql
│   ├── 2NF.sql
│   └── 3NF.sql
│
├── Outputs/
│   ├── Given(1NF).txt
│   ├── Output(2NF).txt
│   └── Output(3NF).txt
│
└── README.md
```
