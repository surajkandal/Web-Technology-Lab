# Employee Task Management System (ETMS)
## Complete System Design Document

---

## Table of Contents
1. [Executive Summary](#executive-summary)
2. [System Architecture](#system-architecture)
3. [Data Model](#data-model)
4. [Features & Functionality](#features--functionality)
5. [Technical Stack](#technical-stack)
6. [Database Design](#database-design)
7. [API Endpoints](#api-endpoints)
8. [Installation & Setup](#installation--setup)
9. [Usage Guide](#usage-guide)
10. [Testing Scenarios](#testing-scenarios)

---

## Executive Summary

The **Employee Task Management System (ETMS)** is a Spring Boot web application designed to digitalize and streamline the process of assigning and tracking tasks for faculty members in a multi-college environment. 

### Problem Statement
The Engineering Department of a college manages tasks manually through emails and spreadsheets, leading to:
- Confusion and miscommunication
- Missed deadlines
- Lack of centralized tracking
- Poor accountability

### Solution
ETMS provides:
- Centralized employee and task database
- Real-time task tracking and status updates
- Multi-college management
- Search and filtering capabilities
- Statistical dashboards for management insights

---

## System Architecture

### Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    Presentation Layer                        │
│                  (Thymeleaf Templates)                       │
│     Dashboard | Employees | Tasks | Forms                    │
└────────────────────┬────────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────────┐
│                   Controller Layer                           │
│  HomeController | EmployeeController | TaskController       │
└────────────────────┬────────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────────┐
│                    Service Layer                             │
│  CollegeService | EmployeeService | TaskService             │
│        (Business Logic & Validation)                         │
└────────────────────┬────────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────────┐
│                  Repository Layer                            │
│  CollegeRepository | EmployeeRepository | TaskRepository     │
│          (Spring Data JPA)                                   │
└────────────────────┬────────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────────┐
│                  Persistence Layer                           │
│              (H2 Database - In-Memory)                       │
└─────────────────────────────────────────────────────────────┘
```

### Design Patterns Used

1. **MVC Pattern** - Separation of concerns into Model, View, Controller
2. **Service Layer Pattern** - Business logic encapsulation
3. **Repository Pattern** - Data access abstraction through Spring Data JPA
4. **DTO Pattern** - Data transfer between layers (implicit through Thymeleaf)
5. **Singleton Pattern** - Spring beans auto-managed

---

## Data Model

### Entity Relationships

```
┌──────────────┐
│   College    │◄────────────┐
├──────────────┤             │
│ id (PK)      │             │
│ name         │             │
│ code         │             │
│ employees[]  │             │
└──────────────┘             │
       ▲                      │
       │ 1:N                  │
       │            ┌─────────────────┐
       │            │    Employee     │
       │            ├─────────────────┤
       └────────────│ id (PK)         │
                    │ name            │
                    │ department      │ 1:N
                    │ designation     ├──────────┐
                    │ email (UK)      │          │
                    │ phone           │          │
                    │ college_id (FK) │          │
                    │ tasks[]         │          │
                    └─────────────────┘          │
                                                 │
                    ┌────────────────────────────┘
                    │
                    │ M:1
                    ▼
        ┌──────────────────────┐
        │       Task           │
        ├──────────────────────┤
        │ id (PK)              │
        │ title                │
        │ description          │
        │ taskType (ENUM)      │
        │ status (ENUM)        │
        │ assignedDate         │
        │ dueDate              │
        │ priority             │
        │ employee_id (FK)     │
        └──────────────────────┘
```

### Database Schema

#### Colleges Table
```sql
CREATE TABLE colleges (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) UNIQUE NOT NULL,
    code VARCHAR(50)
);
```

#### Employees Table
```sql
CREATE TABLE employees (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    department VARCHAR(255) NOT NULL,
    designation VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    phone VARCHAR(20),
    college_id BIGINT,
    FOREIGN KEY (college_id) REFERENCES colleges(id)
);
```

#### Tasks Table
```sql
CREATE TABLE tasks (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    task_type VARCHAR(50),
    status VARCHAR(50),
    assigned_date DATE,
    due_date DATE NOT NULL,
    priority VARCHAR(50),
    employee_id BIGINT,
    FOREIGN KEY (employee_id) REFERENCES employees(id)
);
```

---

## Features & Functionality

### 1. Employee Management
- **Create Employee**: Add new faculty members with college assignment
- **View Employee**: See employee details and their assigned tasks
- **Edit Employee**: Update employee information
- **Delete Employee**: Remove employee records (cascades task deletion)
- **Search Employee**: Search by name across all colleges
- **Filter by College**: View employees of specific college

**Validations**:
- Name required (non-blank)
- Department required
- Designation required
- Valid email format
- Unique email constraint
- Phone number optional

### 2. Task Management
- **Create Task**: Assign new task with all details
- **View Task**: See complete task information
- **Edit Task**: Update task details
- **Delete Task**: Remove task from system
- **Assign to Employee**: Link task to faculty member
- **Update Status**: Change task status (PENDING → IN_PROGRESS → COMPLETED)
- **Auto-Overdue**: Tasks past due date with PENDING status auto-marked as OVERDUE

**Task Types Available**:
1. EXAM_DUTY - Exam invigilations
2. PROJECT_GUIDE - Project supervision
3. DOCUMENTATION - Document preparation
4. EVENT_COORDINATION - Event management
5. REPORT_SUBMISSION - Report submissions
6. OTHER - Miscellaneous tasks

**Task Status Workflow**:
- PENDING (Initial) → IN_PROGRESS → COMPLETED
- PENDING → OVERDUE (automatic if due date passed)

**Priority Levels**:
- HIGH - Urgent tasks
- MEDIUM - Important tasks
- LOW - Regular tasks

### 3. Dashboard & Analytics
- **Overall Statistics**: Total employees, tasks, task breakdown by status
- **College-Specific Dashboard**: Filter view by college
- **Task Statistics**: 
  - Pending tasks count
  - Completed tasks count
  - In-progress tasks count
  - Overdue tasks count
- **College-wise Breakdown**: Task statistics for each college
- **Recent Tasks**: Latest tasks across system

### 4. Search & Filter
- **Employee Search**: By name (case-insensitive, partial match)
- **Employee Filter**: By college department
- **Task Filter**: By status and college
- **Multi-filter**: Combine filters for refined search

### 5. Multi-College Support
- Separate views per college
- College selection in sidebar
- College context in all operations
- College-specific statistics

---

## Technical Stack

### Backend
| Component | Technology | Version |
|-----------|-----------|---------|
| Framework | Spring Boot | 3.2.4 |
| JDK | Java | 17 |
| ORM | Spring Data JPA | Jakarta Persistence |
| Database | H2 | Latest |
| Validation | Jakarta Bean Validation | Latest |
| Build | Maven | 3.x |

### Frontend
| Component | Technology |
|-----------|-----------|
| Templating | Thymeleaf | 3.x |
| Styling | Bootstrap-inspired CSS3 |
| Interactivity | Native JavaScript |
| Icons | Unicode Emoji |

### Additional Libraries
- Jackson (JSON serialization)
- Lombok (optional, for getters/setters)
- Spring Validation

---

## Database Design

### H2 Database Configuration
```properties
# Database URL (In-Memory)
spring.datasource.url=jdbc:h2:mem:etmsdb

# Driver
spring.datasource.driver-class-name=org.h2.Driver

# Credentials
spring.datasource.username=sa
spring.datasource.password=

# JPA/Hibernate Configuration
spring.jpa.hibernate.ddl-auto=create-drop
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true

# H2 Console (Admin Access)
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console
```

### Indexes & Performance
- Email column indexed (unique constraint)
- Foreign keys properly defined
- Eager/Lazy loading configured appropriately
- Order by clauses for task queries

### Data Initialization
- Automatic data loading from `data.sql` on application startup
- Sample data includes:
  - 3 colleges (Engineering, Pharmacy, BHMS)
  - 8 sample employees
  - 6 sample tasks

---

## API Endpoints

### Homepage & Dashboard
```
GET /
  Parameters: college (Long, optional)
  Response: Dashboard view with statistics
  Functionality: Shows overall or college-specific dashboard
```

### Employee Endpoints
```
GET /employees
  Parameters: search (String, optional), college (Long, optional)
  Response: Employee list view
  Functionality: List all employees with optional search/filter

GET /employees/new
  Response: Employee form template
  Functionality: Show form to add new employee

POST /employees/save
  Parameters: 
    - employee object (name, department, designation, email, phone)
    - collegeId (Long, optional)
  Response: Redirect to /employees with success message
  Functionality: Save new or update existing employee

GET /employees/edit/{id}
  Parameters: id (Long)
  Response: Employee form template
  Functionality: Show form with employee data for editing

GET /employees/view/{id}
  Parameters: id (Long)
  Response: Employee detail view with tasks
  Functionality: Display employee information and assigned tasks

GET /employees/delete/{id}
  Parameters: id (Long)
  Response: Redirect to /employees
  Functionality: Delete employee and cascade delete tasks
```

### Task Endpoints
```
GET /tasks
  Parameters: status (String, optional), college (Long, optional)
  Response: Task list view
  Functionality: List all tasks with optional status/college filter

GET /tasks/new
  Parameters: employeeId (Long, optional)
  Response: Task form template
  Functionality: Show form to create new task

POST /tasks/save
  Parameters:
    - task object (title, description, taskType, status, dueDate, priority)
    - employeeId (Long, optional)
  Response: Redirect to /tasks with success message
  Functionality: Save new or update existing task

GET /tasks/edit/{id}
  Parameters: id (Long)
  Response: Task form template
  Functionality: Show form with task data for editing

GET /tasks/view/{id}
  Parameters: id (Long)
  Response: Task detail view
  Functionality: Display task information with employee details

GET /tasks/delete/{id}
  Parameters: id (Long)
  Response: Redirect to /tasks
  Functionality: Delete task

POST /tasks/updateStatus/{id}
  Parameters: 
    - id (Long)
    - status (String) - One of: PENDING, IN_PROGRESS, COMPLETED, OVERDUE
  Response: Redirect to previous task view
  Functionality: Update task status and save
```

### Admin Endpoints
```
GET /h2-console
  Response: H2 Database Management Console
  Functionality: Direct database access for administrators
```

---

## Installation & Setup

### Prerequisites
- Java JDK 17 or higher
- Maven 3.6 or higher
- Any modern web browser
- Git (for cloning)

### Installation Steps

1. **Clone or Extract Project**
   ```bash
   cd d:\WTL\Springbootadv10
   ```

2. **Clean and Compile**
   ```bash
   mvn clean compile
   ```

3. **Run the Application**
   ```bash
   mvn spring-boot:run
   ```
   
   Or build JAR and run:
   ```bash
   mvn clean package
   java -jar target/etms-1.0.0.jar
   ```

4. **Access Application**
   - Main Application: http://localhost:8080
   - H2 Console: http://localhost:8080/h2-console

### H2 Console Access
- URL: http://localhost:8080/h2-console
- Driver Class: org.h2.Driver
- JDBC URL: jdbc:h2:mem:etmsdb
- Username: sa
- Password: (leave blank)

---

## Usage Guide

### Dashboard Navigation

1. **Main Dashboard** (Home)
   - Click "Dashboard" link
   - See overall statistics
   - View college breakdown
   - Click college names to filter

2. **Managing Employees**
   - Click college name in sidebar to see employees
   - Use search bar to find by name
   - Click "+ Add Employee" to create new
   - Click "View" to see employee details
   - Click "Edit" to modify information
   - Click "+Task" to assign new task
   - Click "Delete" to remove employee

3. **Managing Tasks**
   - See all tasks with status
   - Filter by status: Pending, In Progress, Completed, Overdue
   - Filter by college in sidebar
   - Create new task with "/" + New Task button
   - Edit task details
   - Update status via dropdown
   - Delete completed or unwanted tasks

4. **Searching & Filtering**
   - Use search bar on employee list
   - Select colleges from sidebar
   - Use status filters on task list
   - Combine multiple filters for precise results

### Typical Workflow

**To Assign a Task to Faculty**:
1. Navigate to Tasks page
2. Click "+ New Task" button
3. Fill task details (title, type, due date, priority)
4. Select employee from dropdown
5. Click "Save"
6. Task appears in employee's task list

**To Track Task Progress**:
1. Go to Tasks page
2. Search for specific task
3. Click "View" to see details
4. Update status via Inline dropdown
5. Status automatically saves

**To Generate Report**:
1. Go to Dashboard
2. Note statistics for overall or college
3. Export data manually or use browser print function
4. Share with department heads

---

## Testing Scenarios

### Unit Test Cases

#### Employee Management Tests
```
✓ Test 1: Create new employee
  - Verify employee saved with all fields
  - Verify email uniqueness
  - Verify college association

✓ Test 2: Update employee information
  - Verify data persistence
  - Verify validation on update
  - Verify relationship integrity

✓ Test 3: Delete employee
  - Verify employee record deleted
  - Verify cascade delete of tasks
  - Verify no orphaned records

✓ Test 4: Search employee by name
  - Verify case-insensitive search
  - Verify partial match works
  - Verify returns correct results
```

#### Task Management Tests
```
✓ Test 5: Create task with all types
  - Verify each task type can be created
  - Verify status defaults to PENDING
  - Verify due date is set correctly

✓ Test 6: Auto-mark overdue
  - Create task with past due date
  - Verify status automatically set to OVERDUE
  - Verify notification (if implemented)

✓ Test 7: Update task status
  - Verify status transitions allowed
  - Verify status not reverted
  - Verify timestamp updated

✓ Test 8: Assign task to employee
  - Verify task linked to employee
  - Verify inverse relationship set
  - Verify appears in employee's task list

✓ Test 9: Delete task
  - Verify task deleted
  - Verify employee's task list updated
  - Verify no orphaned records
```

#### Dashboard Tests
```
✓ Test 10: Calculate dashboard statistics
  - Verify count of employees
  - Verify count of tasks
  - Verify count by status
  - Verify count by college

✓ Test 11: College-specific dashboard
  - Verify correct data filtered
  - Verify statistics accurate
  - Verify cross-college data not shown
```

#### Search & Filter Tests
```
✓ Test 12: Multi-filter search
  - Search by college AND name
  - Filter by college AND status
  - Verify results are intersection

✓ Test 13: Edge cases
  - Search with empty string
  - Search with special characters
  - Search with very long strings
```

### Integration Test Cases

#### Full Workflow Tests
```
✓ Scenario 1: New Faculty Onboarding
  1. Add new employee from Engineering college
  2. Assign 3 different tasks
  3. Verify dashboard shows updated counts
  4. Edit employee designation
  5. Verify changes reflected everywhere

✓ Scenario 2: Task Completion Workflow
  1. Create task with PENDING status
  2. Change to IN_PROGRESS
  3. Add important note
  4. Change to COMPLETED
  5. Verify not shown in pending count

✓ Scenario 3: Multi-College Operations
  1. Filter dashboard by Engineering
  2. Add employee to Engineering
  3. Switch to Pharmacy view
  4. Verify employee not shown
  5. Switch back to consolidated view
  6. Verify employee visible

✓ Scenario 4: Deadline Management
  1. Create task with due date 5 days from now
  2. Verify status is PENDING
  3. Manually update system date (or run report)
  4. Verify task marked OVERDUE
  5. Verify appears in overdue section
```

### Performance Tests
```
✓ Test 15: Bulk operations
  - Create 100 tasks
  - Verify dashboard loads < 2 seconds
  - Search across all data
  - Verify response time < 1 second

✓ Test 16: Concurrent access
  - Simulate 10 concurrent users
  - Perform CRUD operations
  - Verify data consistency
  - Verify no conflicts
```

### User Acceptance Tests
```
✓ UAT 1: Navigation ease
  - New user can navigate without help
  - All buttons clearly labeled
  - No dead links

✓ UAT 2: Data entry validation
  - Form validation messages clear
  - Error messages helpful
  - Validation prevents bad data

✓ UAT 3: Visual feedback
  - Success messages appear
  - Confirmations before delete
  - Loading states visible

✓ UAT 4: Permission handling
  - Users see only appropriate data
  - Cannot modify other college's data
  - Cannot delete protected records
```

---

## Troubleshooting

### Common Issues

1. **Application won't start**
   - Verify Java 17 is installed: `java -version`
   - Verify Maven is installed: `mvn -version`
   - Check if port 8080 is available

2. **Database connection fails**
   - H2 is in-memory, starts fresh each run
   - Check application.properties for correct URL
   - Verify datasourceURL format

3. **Data not persisting**
   - H2 in-memory database resets on restart
   - For persistence, switch to file-based or external DB

4. **Form validation errors**
   - All marked required fields must be filled
   - Email must be in valid format
   - No duplicate emails allowed

---

## Future Enhancements

1. **Authentication & Authorization**
   - User login system
   - Role-based access control
   - Admin vs. Faculty views

2. **Notifications**
   - Email notifications for task assignments
   - Deadline reminders
   - Task status change alerts

3. **Reporting**
   - PDF report generation
   - Excel export
   - Graphical analytics

4. **Advanced Features**
   - Task comments/discussion
   - Task dependencies
   - Resource allocation
   - Time tracking

5. **Database**
   - Switch from H2 to MySQL/PostgreSQL
   - Backup & disaster recovery
   - Data archiving

6. **UI/UX**
   - Responsive mobile design
   - Dark/Light theme toggle
   - Accessibility improvements

---

## Contact & Support

For issues, feature requests, or questions:
- Contact: Engineering Department IT
- Email: support@college.edu
- Documentation: Internal Wiki
- Issue Tracker: GitHub Issues

---

**Document Version**: 1.0  
**Last Updated**: April 2026  
**Status**: Active & Maintained
