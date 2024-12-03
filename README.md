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

## Data Model Description:# Library Management System Data Model

## Overview
This data model represents a library management system designed to handle operations such as member registration, media item inventory, loans, reviews, and employee responsibilities. The schema provides a comprehensive structure that connects members, staff, and resources, facilitating efficient library operations.

## Entities and Relationships

### 1. Member
- **Attributes**: `idmember`, `memberName`, `email`, `Address`
- **Relationships**:
  - A member can have **zero or many cards** (1:0..N cardinality) through the `card` table.
  - Members can also write reviews for media items (1:0..N cardinality) through the `review` table.

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

<img width="886" alt="Screenshot 2024-12-03 at 12 07 11 AM" src="https://github.com/user-attachments/assets/66ac3fba-93f6-4ad6-9901-4c06d0ca864d">

## Data Dictionary

## Queries

### Query 1
<img width="479" alt="Screenshot 2024-12-03 at 12 10 37 AM" src="https://github.com/user-attachments/assets/bfe780e5-9f9b-458e-afd6-d836c8ddfd3f">

### Query 2
<img width="468" alt="Screenshot 2024-12-03 at 12 11 11 AM" src="https://github.com/user-attachments/assets/112245d0-6d67-4aeb-9f51-d2525ef23126">

### Query 3
<img width="434" alt="Screenshot 2024-12-03 at 12 12 03 AM" src="https://github.com/user-attachments/assets/05c6de7e-7dc4-4e43-ad43-f05b56973e38">



## Data Visualization

### Visualization 1
<img width="541" alt="Screenshot 2024-12-03 at 12 09 18 AM" src="https://github.com/user-attachments/assets/59eeaab4-2424-47bc-bfa3-e28185867600">

### Visualization 2
<img width="545" alt="Screenshot 2024-12-03 at 12 09 59 AM" src="https://github.com/user-attachments/assets/41a8849a-b8c3-44fb-b136-3b2b38ded6e9">

