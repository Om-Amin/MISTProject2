# MIST 4610 Project 2

## Team Name: 
MIS Avengers

CRN: 29704

## Team Members:

- Om Amin [@Om-Amin](https://github.com/Om-Amin)
- Rohit Sharma [@Rohits629](https://github.com/Rohits629)
- Aaron Jackson [@Aaronjackson3x](https://github.com/Aaronjackson3x)
- Kristen Wang [@kristennw](https://github.com/kristennw)
- Oliver Tripcony [@OliverTripcony](https://github.com/OliverTripcony)

## Overview
This data model represents a library management system designed to handle operations such as member registration, media item inventory, loans, reviews, and employee responsibilities. The schema provides a comprehensive structure that connects members, staff, and resources, facilitating efficient library operations.

## Library Management System Data Model Entities and Relationships

### 1. Member

- **Attributes**: `idmember`, `memberName`, `email`, `city`, `state`, `zipcode`

- **Relationships**:
  - A member can have **zero or many cards** (1:0..N cardinality) through the `card` table.
  - Members can also write reviews for media items (1:0..N cardinality) through the `review` table.

- **Changes**: The `Address` attribute has been removed and replaced with `city`, `state`, and `zipcode` to better standardize and organize location data.

### 2. Card
- **Attributes**: `idCard`, `issue_date`, `expiration_date`, `idmember`, `status`
- **Relationships**:
  - Each card belongs to exactly one member (1:1 cardinality), but a member can hold multiple cards (1:N modality).

### 3. MediaItem
- **Attributes**: `idmediaItem`, `title`, `release_year`, `language`, `idsection`, `idType`, `genre`
- **Relationships**:
  - Each media item is part of one section (1:1 cardinality), and each section can have multiple media items (1:N modality).
  - Media items are linked to media types (e.g., book, magazine) in a 1:1 relationship.

### 4. Loan
- **Attributes**: `idloan`, `loanDate`, `returnedDate`, `expectedDueDate`, `idmember`, `idmediaItem`
- **Relationships**:
  - Loans form a **many-to-many (M:N)** relationship between members and media items through `idmember` and `idmediaItem`.

### 5. Review
- **Attributes**: `idReview`, `rating`, `reviewText`, `idmediaItem`, `idmember`
- **Relationships**:
  - Reviews connect members and media items in a **many-to-one (M:N)** relationship, as multiple reviews can exist for one media item, and a single member can review multiple items.

### 6. MediaCreator
- **Attributes**: `idmediaItem`, `idCreator`
- **Relationships**:
  - This associative table connects media items to their creators, forming a **many-to-many (M:N)** relationship between `mediaItem` and `creator`.

### 7. Creator
- **Attributes**: `idCreator`, `creator_fname`, `creator_lname`, `typeCreator`
- **Relationships**:
  - Each creator can contribute to multiple media items, while each media item can have multiple creators (M:N cardinality).

### 8. Employee
- **Attributes**: `idemployee`, `employee_Name`, `position`, `salary`, `idSection`, `employee_idemployee`
- **Relationships**:
  - Employees are related to sections in a **many-to-one (M:N)** relationship, as multiple employees can belong to a single section.
  - The `employee_idemployee` column establishes a hierarchical structure to identify the "section boss" or manager of a specific section.

### 9. Section
- **Attributes**: `idsection`, `sectionName`, `employee_idemployee`
- **Relationships**:
  - Each section is managed by one employee (1:1 modality), and a section can have multiple employees reporting to it (1:N cardinality).
  - Sections also organize media items.

### 10. MediaType
- **Attributes**: `idType`, `Description`
- **Relationships**:
  - Each media type (e.g., book, magazine) is associated with multiple media items (1:N cardinality), indicating the format or classification of the media.

## Cardinality and Modality Summary
- **1:1**: Media items to sections, ensuring each item belongs to one section.
- **1:N**: Members to cards and sections to media items, representing the hierarchy and ownership structure.
- **M:N**: Media creators and media items, enabling flexible collaboration between creators and media resources.
- **Optional Relationships**: Members may not have reviews, reflecting flexibility in participation.
- **Mandatory Relationships**: Loans must reference both members and media items, ensuring complete transaction tracking.

## Use Cases
This data model is designed to:
1. Track member activity, including issued cards and borrowing behavior.
2. Manage and classify media items, ensuring proper organization and availability.
3. Monitor employee responsibilities and their associated sections.
4. Facilitate reporting on library usage trends, such as popular genres, loan frequencies, and review ratings.
5. Support operational decision-making, such as staffing needs and inventory management.

---

![final_library](https://github.com/user-attachments/assets/330ab20f-cb90-477c-abd5-77928d257dcc)

## Data Dictionary

## Table: `member`

| Column Name | Description                               | Data Type | Size | Key |
|-------------|-------------------------------------------|-----------|------|-----|
| `idmember`  | PK; unique identifier for each member    | INT       | 11   | PK  |
| `memberName`| Name of the member                       | VARCHAR   | 80   |     |
| `email`     | Email of the member                      | VARCHAR   | 85   |     |
| `city`      | City where the member resides            | VARCHAR   | 50   |     |
| `state`     | State where the member resides           | VARCHAR   | 2    |     |
| `zipcode`   | ZIP code of the member's address         | INT       | 10   |     |

---

## Table: `card`

| Column Name      | Description                             | Data Type | Size | Key |
|------------------|-----------------------------------------|-----------|------|-----|
| `idCard`         | PK; unique identifier for each card    | INT       | 11   | PK  |
| `issue_date`     | Date the card was issued               | DATE      |      |     |
| `expiration_date`| Date the card expires                  | DATE      |      |     |
| `idmember`       | FK; links to member                    | INT       | 11   | FK  |
| `status`         | Status of the card (e.g., Active, Expired) | VARCHAR   | 45   |     |

---

## Table: `employee`

| Column Name        | Description                                    | Data Type | Size | Key |
|--------------------|------------------------------------------------|-----------|------|-----|
| `idemployee`       | PK; unique identifier for each employee       | INT       | 11   | PK  |
| `employee_Name`    | Name of the employee                          | VARCHAR   | 45   |     |
| `position`         | Job position of the employee                  | VARCHAR   | 45   |     |
| `salary`           | Salary of the employee                        | DECIMAL   | 10,2 |     |
| `idSection`        | FK; links to section                          | INT       | 11   | FK  |
| `employee_idemployee`| FK; links to mentor/manager (self-reference) | INT       | 11   | FK  |

---

## Table: `section`

| Column Name         | Description                              | Data Type | Size | Key |
|---------------------|------------------------------------------|-----------|------|-----|
| `idsection`         | PK; unique identifier for each section  | INT       | 11   | PK  |
| `sectionName`       | Name of the section                     | VARCHAR   | 45   |     |
| `employee_idemployee`| FK; links to employee managing the section | INT       | 11   | FK  |

---

## Table: `mediaType`

| Column Name  | Description                              | Data Type | Size | Key |
|--------------|------------------------------------------|-----------|------|-----|
| `idType`     | PK; unique identifier for media type     | INT       | 11   | PK  |
| `Description`| Description of the media type           | VARCHAR   | 45   |     |

---

## Table: `mediaItem`

| Column Name  | Description                                    | Data Type | Size | Key |
|--------------|------------------------------------------------|-----------|------|-----|
| `idmediaItem`| PK; unique identifier for each media item      | INT       | 11   | PK  |
| `title`      | Title of the media item                       | VARCHAR   | 45   |     |
| `release_year`| Release year of the media item               | DATE      |      |     |
| `language`   | Language of the media item                    | VARCHAR   | 45   |     |
| `idsection`  | FK; links to section                          | INT       | 11   | FK  |
| `idType`     | FK; links to media type                       | INT       | 11   | FK  |
| `genre`      | Genre of the media item                       | VARCHAR   | 45   |     |

---

## Table: `loan`

| Column Name       | Description                                     | Data Type | Size | Key |
|-------------------|-------------------------------------------------|-----------|------|-----|
| `idloan`          | PK; unique identifier for each loan            | INT       | 11   | PK  |
| `loanDate`        | Date the item was loaned                       | DATE      |      |     |
| `returnedDate`    | Date the item was returned                     | DATE      |      |     |
| `expectedDueDate` | Expected due date for the item                 | DATE      |      |     |
| `idmember`        | FK; links to member                            | INT       | 11   | FK  |
| `idmediaItem`     | FK; links to media item                        | INT       | 11   | FK  |

---

## Table: `creator`

| Column Name     | Description                              | Data Type | Size | Key |
|-----------------|------------------------------------------|-----------|------|-----|
| `idCreator`     | PK; unique identifier for each creator  | INT       | 11   | PK  |
| `creator_fname` | First name of the creator               | VARCHAR   | 45   |     |
| `creator_lname` | Last name of the creator                | VARCHAR   | 45   |     |
| `typeCreator`   | Type of creator (e.g., Author, Director) | VARCHAR   | 45   |     |

---

## Table: `mediaCreator`

| Column Name  | Description                              | Data Type | Size | Key |
|--------------|------------------------------------------|-----------|------|-----|
| `idmediaItem`| FK; links to media item                 | INT       | 11   | FK  |
| `idCreator`  | FK; links to creator                    | INT       | 11   | FK  |

---

## Table: `review`

| Column Name   | Description                                  | Data Type | Size | Key |
|---------------|----------------------------------------------|-----------|------|-----|
| `idReview`    | PK; unique identifier for each review       | INT       | 11   | PK  |
| `rating`      | Rating given to the media item              | DECIMAL   | 10,2 |     |
| `reviewText`  | Review text provided                       | TEXT      | 200  |     |
| `idmediaItem` | FK; links to media item                    | INT       | 11   | FK  |
| `idmember`    | FK; links to member                        | INT       | 11   | FK  |

## Queries

| Feature                     | Query 1 | Query 2 | Query 3 | Query 4 | Query 5 |
|-----------------------------|---------|---------|---------|---------|---------|
| multiple table join          |   X     |    X     |   X     |    x    |    X    |
| subquery                     |         |        |         |     x    |        |
| GROUP BY                     |    X    |         |    X     |        |    X    |
| GROUP BY with HAVING         |         |    X     |         |        |         |
| multi-condition WHERE        |         |        |         |    x     |     X    | 
| built-in functions (AVG, COUNT) |   X     |    X     |    X     |        |    X   | 
| REGEXP                       |         |         |         |         |     X    | 
| NOT IN                       |         |        |         |         |         |

### Query 1
This query lists the name, email, address, and language of library members who have borrowed media items in a language that isn't English and do not currently have an active library card.

![image](https://github.com/user-attachments/assets/5f41d1c7-a711-49d0-b761-0d8ddce9b813)

Query 1 is useful as it can be used to improve member retention and address potential inventory gaps in different languages. If a language other than English has a significant amount of members whose cards have expired the library can focus attention on content acquisition in this language. After doing so, the library could potentially reach out to these past members, highlighting their improved offerings in hopes of gaining their business back.

### Query 2
This query lists media items that have been borrowed by less than three unique members. It also provides the genre and language of the media item, along with the amount of unique borrowers for each media item.

<img width="468" alt="Screenshot 2024-12-03 at 12 11 11 AM" src="https://github.com/user-attachments/assets/112245d0-6d67-4aeb-9f51-d2525ef23126">

Query 2 is useful as it highlights media that is underutilized, suggesting it is potentially outdated or has limited interest. Using this information the library can improve its offerings by removing media items that may not be of importance to the library audience. Consequently, this will improve the collection's relevance by aligning inventory with member preferences.

### Query 3
This query lists the name of each genre, the total amount of reviews, and the average rating (in descending order) for genres that have at least two or more reviews.

<img width="434" alt="Screenshot 2024-12-03 at 12 12 03 AM" src="https://github.com/user-attachments/assets/05c6de7e-7dc4-4e43-ad43-f05b56973e38">

Query 3 is useful as it identifies popular genres with high average ratings. Using this information the library can focus on content that is valued most by members, improving member satisfaction. The query also only lists genres that have at least 2 reviews, reducing potential outliers/bias that may skew average ratings.

### Query 4 
Query 4 finds the names and emails of users who have not returned their loaned item and do not have an active card status.

![image](https://github.com/user-attachments/assets/3d22e00c-2444-4868-bde2-ea26b7f5d1f9)

Query 4 is useful as it allows the library to be able to contact previous members who have yet to return their loaned items to issue a reminder to do so, or to be able to issue fines to recoup the costs of lost media items.

### Query 5 
Query 5 finds the names, positions, and salaries of employees who are mentoring other employees and keeping track of how many many mentees they have.

![image](https://github.com/user-attachments/assets/2ce651e2-dde4-4c04-96e9-997c7671f2bb)

Query 5 is useful as it identifies non-managerial employees who actively contribute to their team by mentoring other employees. This information can be used to assess potential promotions and pay raises for such employees, placing them in managerial positions. Additionally, managers can ensure that mentors have even amounts of mentees, reducing the workload for overburdened employees and fostering productivity. 



## Data Visualization

### Visualization 1
<img width="541" alt="Screenshot 2024-12-03 at 12 09 18 AM" src="https://github.com/user-attachments/assets/59eeaab4-2424-47bc-bfa3-e28185867600">

Visualization 1 is a line chart that shows the number of library cards issued each year from 2015 to 2023. The chart makes it easy to see trends over time, like which years had more or fewer cards issued. It helps the library understand when they were successful in attracting new members and when there might have been challenges. For example, a sudden increase could mean a successful campaign, while a drop might show the need for better outreach. Overall, this visualization helps the library plan for the future by learning from past patterns.

### Visualization 2
<img width="545" alt="Screenshot 2024-12-03 at 12 09 59 AM" src="https://github.com/user-attachments/assets/41a8849a-b8c3-44fb-b136-3b2b38ded6e9">

Visualization 2 is a bar chart that displays the number of books loaned per genre. It helps identify which genres are most popular among borrowers, such as Fantasy and Science Fiction, which have the highest counts. By highlighting borrowing trends, this visualization provides valuable insights into member preferences. Managers can use this information to allocate resources, acquire more books in popular genres, or promote underutilized ones to balance the collection and meet member demand effectively.

### Visualization 3
<img width="1505" alt="Screenshot 2024-12-03 at 8 31 40 PM" src="https://github.com/user-attachments/assets/6126fea5-fd6e-4413-8511-71fc66261977">

Visualization 3 is a bar chart that shows the average salary for employees by position within the organization. The visualization provides a clear comparison of compensation across roles, highlighting the highest average salary for Managers at $85,400 and the lowest for Assistants at $43,000. This data can help managers analyze pay structures, ensuring equity and competitiveness in salaries. It can also support strategic decisions, such as budgeting for new hires or allocating resources for salary adjustments to attract and retain talent effectively.

### Dashboard Visualization


## Bookmarked Queries
Name of the database: cs_ota82991

Each query listed above is stored in the database using stored procedures using the format: CALL TP_P1();


