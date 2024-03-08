# Data Normalization and Entity-Relationship Diagramming
## Part 1:

**This is the example dataset given to us, however this is not in 4th normal form:**
| assignment_id | student_id | due_date | professor | assignment_topic                | classroom | grade | relevant_reading    | professor_email   |
| :------------ | :--------- | :------- | :-------- | :------------------------------ | :-------- | :---- | :------------------ | :---------------- |
| 1             | 1          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 80    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 2             | 7          | 18.11.21 | Logston   | Single table queries            | 60FA 314  | 25    | Dümmlers Chapter 11 | e.logston@foo.edu |
| 1             | 4          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 75    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 5             | 2          | 05.05.21 | Logston   | Python and pandas               | 60FA 314  | 92    | Dümmlers Chapter 14 | e.logston@foo.edu |
| 4             | 2          | 04.07.21 | Nevarez   | Spreadsheet aggregate functions | WWH 201   | 65    | Zehnder Page 87     | i.nevarez@foo.edu |
| ...           | ...        | ...      | ...       | ...                             | ...       | ...   | ...                 | ...               |

In order to convert the data to 4th normal form, we would have to satisfy the conditions of the other normal forms first, being:
### 1st normal form (ALready Satisfied)
- **Fixed Schema Requirement**
  - Relational database systems require tables to have a fixed schema.
  - Each table must contain the same number of fields across all records.
  - This schema setup requires no extra work to maintain in modern relational database systems.
- **Singular Value Fields**
  - Every field in a table should hold a singular value.
  - Ensures data consistency and integrity within the database.

### 2nd normal form
- **First Normal Form Requirement**
  - Records in both second and third normal forms must adhere to the first normal form criteria.
- **Uniqueness of Non-Key Field Facts**
  - Non-key fields must convey facts that pertain exclusively to the entity identified by the primary key.
- **Prohibition of Partial or Unrelated Facts**
  - Non-key fields cannot contain facts about only a portion of the entity or about entirely different entities.
- **Fact Relationship Types**
  - The facts represented can include one-to-many relationships (e.g., an employee's department) or one-to-one relationships (e.g., an employee's spouse).

### 3rd normal form
- **Third Normal Form Prerequisite**
  - To qualify for third normal form, records must already meet the criteria of second normal form.
- **Violation of Third Normal Form**
  - Occurs if a non-key field provides information about another non-key field, breaching third normal form standards.

### 4th normal form
- **Third Normal Form Compliance**
  - Records must adhere to third normal form requirements.
- **Limitation on Multi-Valued Facts**
  - A table should not encapsulate multiple independent multi-valued facts about a single entity.

## 4NF Violations:
AKA why does the example table not satisfy 4th Normal form?
1. Dependencies:
   - Specific fields such as professor, classroom, relevant_reading, and professor_email are dependent only on the assignment_id.
   - These dependencies do not require the entire composite key for their determination
   - The professor_email is determined by the professor, a non-prime attribute.
   - This creates a dependency, where professor_email should ideally depend directly on the primary key rather than through another attribute.
3. Multi-Valued Dependencies:
   - The dataset indicates the presence of multi-valued dependencies, which is a violation of the Fourth Normal Form (4NF).
   - Examples include:
     - A professor can be associated with multiple assignments across different courses.
     - Courses can have multiple sections, with each section possibly being taught by different professors in various classrooms.
     - Consequently, attributes like assignment_topic, due_date, classroom, and relevant_reading can change independently based on either the professor or the course, demonstrating multi-valued dependencies.

## Solution
In order to convert the table to 4th normal form, the easiet solution would be to just make a bunch of different tables to remove dependencies.
#### Example tables:
***Course Table***
| course_id | course_name              |
|-----------|--------------------------|
| 1         | Database Design          |
| 2         | Introduction to Python   |
| 3         | Advanced Spreadsheet Use |

***Professors Table***
| professor_id | professor_name | professor_email   |
|--------------|----------------|-------------------|
| 1            | Melvin         | l.melvin@foo.edu  |
| 2            | Logston        | e.logston@foo.edu |
| 3            | Ishar          | i.Ishar@foo.edu   |

***Classrooms Table***
| classroom_id | classroom_location |
|--------------|--------------------|
| 1            | WWH 101            |
| 2            | 60FA 314           |
| 3            | WWH 201            |

***8Assignments Table***
| assignment_id | course_id | professor_id | assignment_topic                | due_date  | relevant_reading   |
|---------------|-----------|--------------|---------------------------------|-----------|--------------------|
| 1             | 1         | 1            | Ishar normalization              | 23.02.21  | Ishar Chapter 3 |
| 2             | 1         | 2            | Single table Ishar            | 18.11.21  | Ishar Chapter 11|
| 3             | 2         | 2            | Python and Ishar               | 05.05.21  | Ishar Chapter 14|
| 4             | 3         | 3            | Spreadsheet aggregate Ishar | 04.07.21  | Ishar Page 87    |

***Sections table***
| section_id | course_id | professor_id | classroom_id |
|------------|-----------|--------------|--------------|
| 1          | 1         | 1            | 1            |
| 2          | 2         | 2            | 3            |
| 3          | 2         | 4            | 2            |
| 4          | 3         | 3            | 3            |

***Grades Table***
| grade_id | student_id | assignment_id | grade |
|----------|------------|---------------|-------|
| 1        | 1          | 1             | 14    |
| 2        | 7          | 2             | 25    |
| 3        | 4          | 1             | 78    |
| 4        | 2          | 3             | 96    |
| 5        | 2          | 4             | 48    |


The normalization process up to the Fourth Normal Form involves dividing the original dataset into distinct tables, each focusing on a single aspect of the university, Like courses, professors, assignments, and sections. This division helps ensure that each table has a primary key that uniquely identifies records, aligning with the Second Normal Form by making all attributes in a table solely dependent on the primary key. Further refinement removes transitive dependencies, eliminating indirect relationships that can cause redundancy and update anomalies.
The move to 4NF specifically addresses multi-valued dependencies by ensuring tables don't mix independent facts about an entity. For instance, assignments, professors, and classrooms are separated into different tables to clearly represent their relationships without implying incorrect dependencies. The creation of specific tables like "Sections" and "Grades" clarifies the interactions between entities, such as linking courses to professors and classrooms, and detailing students' grades for assignments without muddling these facts with unrelated details.

![ER Diagram](./images/ERdiagram.drawio.svg)


