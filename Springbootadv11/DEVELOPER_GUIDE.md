# Employee Task Management System
## Developer Guide & API Reference

---

## Table of Contents
1. [Architecture Overview](#architecture-overview)
2. [Setting Up Development Environment](#setting-up-development-environment)
3. [Project Structure](#project-structure)
4. [Core Classes Reference](#core-classes-reference)
5. [REST API Parameters](#rest-api-parameters)
6. [Service Layer Reference](#service-layer-reference)
7. [Repository Methods](#repository-methods)
8. [Database Queries](#database-queries)
9. [Adding New Features](#adding-new-features)
10. [Troubleshooting & Debugging](#troubleshooting--debugging)

---

## Architecture Overview

### Layered Architecture

```
┌─ PRESENTATION LAYER ─────────────────────┐
│   Thymeleaf Templates (.html)            │
│   Handles user interface and forms       │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│   CONTROLLER LAYER                       │
│   Handles HTTP requests/responses        │
│   Classes: *Controller                   │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│   SERVICE LAYER                          │
│   Contains business logic                │
│   Classes: *Service                      │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│   REPOSITORY LAYER                       │
│   Data access using Spring Data JPA      │
│   Interfaces: *Repository                │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│   PERSISTENCE LAYER                      │
│   H2 In-Memory Database                  │
└──────────────────────────────────────────┘
```

### Request Flow Example

```
User Request: GET /employees
       ↓
HomeController.listEmployees()
       ↓
EmployeeService.getAllEmployees()
       ↓
EmployeeRepository.findAll()
       ↓
H2 Database Query
       ↓
Returns List<Employee>
       ↓
Service processes data
       ↓
Controller passes to view
       ↓
Thymeleaf renders HTML
       ↓
Browser displays page
```

---

## Setting Up Development Environment

### System Requirements
```
- OS: Windows / Linux / macOS
- Java: JDK 17+ (check: java -version)
- Maven: 3.6+ (check: mvn -version)
- IDE: IntelliJ IDEA / VS Code / Eclipse
- Browser: Chrome / Firefox / Edge
```

### Installation & Setup

**1. Clone project**
```bash
cd d:\WTL\Springbootadv10
```

**2. Import into IDE (IntelliJ)**
```
File → Open → Select project directory
Close project structure dialog
Right-click pom.xml → Add as Maven Project
```

**3. Build project**
```bash
mvn clean install
```

**4. Run from command line**
```bash
mvn spring-boot:run
```

**5. Access application**
```
http://localhost:8080
```

### IDE Configuration

**IntelliJ IDEA**:
- Settings → Java Compiler → Java 17
- Settings → Build → Maven → Use Java 17
- Run → Edit Configurations → VM options: `-Dspring.profiles.active=dev`

**VS Code**:
- Install "Spring Boot Extension Pack"
- Install "Extension Pack for Java"
- F5 to debug
- Ctrl+F5 to run

---

## Project Structure

```
springboot9/
│
├── pom.xml (Maven configuration)
│
├── src/main/java/com/scoe/etms/
│   │
│   ├── EmployeeTaskManagementApplication.java (Main entry point)
│   │
│   ├── model/ (Entity classes)
│   │   ├── College.java
│   │   ├── Employee.java
│   │   └── Task.java
│   │
│   ├── controller/ (Request handlers)
│   │   ├── HomeController.java
│   │   ├── EmployeeController.java
│   │   └── TaskController.java
│   │
│   ├── service/ (Business logic)
│   │   ├── CollegeService.java
│   │   ├── EmployeeService.java
│   │   └── TaskService.java
│   │
│   └── repository/ (Data access)
│       ├── CollegeRepository.java
│       ├── EmployeeRepository.java
│       └── TaskRepository.java
│
├── src/main/resources/
│   │
│   ├── application.properties (Configuration)
│   ├── data.sql (Initial data)
│   │
│   ├── templates/ (Thymeleaf templates)
│   │   ├── dashboard.html
│   │   ├── employees/
│   │   │   ├── form.html
│   │   │   ├── list.html
│   │   │   └── view.html
│   │   └── tasks/
│   │       ├── form.html
│   │       ├── list.html
│   │       └── view.html
│   │
│   └── static/ (Static assets)
│       └── css/
│           └── style.css
│
└── target/ (Build output)
    └── etms-1.0.0.jar
```

---

## Core Classes Reference

### 1. College.java (Entity)

```java
@Entity
@Table(name = "colleges")
public class College {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;                    // Unique identifier
    
    @NotBlank
    @Column(unique = true)
    private String name;                // College name
    
    private String code;                // College code
    
    @OneToMany(mappedBy = "college")
    private List<Employee> employees;   // Associated employees
}
```

**Usage**:
```java
College eng = new College();
eng.setName("Engineering");
eng.setCode("ENGG");
collegeService.saveCollege(eng);
```

### 2. Employee.java (Entity)

```java
@Entity
@Table(name = "employees")
public class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @NotBlank
    private String name;                // Full name
    
    @NotBlank
    private String department;          // Department name
    
    @NotBlank
    private String designation;         // Position (Prof, AP, etc.)
    
    @Email
    @NotBlank
    @Column(unique = true)
    private String email;               // Unique email
    
    private String phone;               // Contact number
    
    @ManyToOne
    @JoinColumn(name = "college_id")
    private College college;            // Associated college
    
    @OneToMany(mappedBy = "employee")
    private List<Task> tasks;           // Assigned tasks
}
```

**Usage**:
```java
Employee emp = new Employee();
emp.setName("Dr. Rajesh Kumar");
emp.setDepartment("Engineering");
emp.setDesignation("Professor");
emp.setEmail("rajesh@college.edu");
emp.setPhone("9876543210");
emp.setCollege(college);
employeeService.saveEmployee(emp);
```

### 3. Task.java (Entity)

```java
@Entity
@Table(name = "tasks")
public class Task {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @NotBlank
    private String title;               // Task name
    
    private String description;         // Detailed description
    
    @Enumerated(EnumType.STRING)
    private TaskType taskType;          // Type of task
    
    @Enumerated(EnumType.STRING)
    private TaskStatus status;          // Current status
    
    private LocalDate assignedDate;     // When assigned
    
    @NotNull
    private LocalDate dueDate;          // Deadline
    
    private String priority;            // HIGH/MEDIUM/LOW
    
    @ManyToOne
    @JoinColumn(name = "employee_id")
    private Employee employee;          // Assigned to employee
    
    public enum TaskType {
        EXAM_DUTY, PROJECT_GUIDE, DOCUMENTATION, 
        EVENT_COORDINATION, REPORT_SUBMISSION, OTHER
    }
    
    public enum TaskStatus {
        PENDING, IN_PROGRESS, COMPLETED, OVERDUE
    }
}
```

**Usage**:
```java
Task task = new Task();
task.setTitle("Exam Duty");
task.setDescription("Invigilate B.E. Sem 4");
task.setTaskType(TaskType.EXAM_DUTY);
task.setStatus(TaskStatus.PENDING);
task.setDueDate(LocalDate.now().plusDays(7));
task.setPriority("HIGH");
task.setEmployee(employee);
taskService.saveTask(task);
```

---

## REST API Parameters

### Employee Endpoints

#### GET /employees
**Purpose**: Retrieve list of employees  
**Parameters**:
```
search  : String (optional) - Search by name
college : Long (optional)   - Filter by college ID
```
**Returns**: Model with `employees` list  
**Example**: `/employees?search=Rajesh&college=1`

#### POST /employees/save
**Purpose**: Save new or update employee  
**Parameters**:
```
employee.name         : String - Full name (required)
employee.department   : String - Department (required)
employee.designation  : String - Position (required)
employee.email        : String - Email (required, unique)
employee.phone        : String - Phone (optional)
collegeId            : Long   - College ID (optional)
```
**Returns**: Redirect to /employees  
**Validation**: Email unique, all required fields filled

#### GET /employees/edit/{id}
**Purpose**: Get employee edit form  
**Parameters**:
```
id : Long - Employee ID (path variable)
```
**Returns**: Form template with employee data

#### GET /employees/view/{id}
**Purpose**: View employee details  
**Parameters**:
```
id : Long - Employee ID (path variable)
```
**Returns**: Employee detail page with tasks

#### GET /employees/delete/{id}
**Purpose**: Delete employee  
**Parameters**:
```
id : Long - Employee ID (path variable)
```
**Returns**: Redirect to /employees  
**Note**: Cascades delete all tasks

---

### Task Endpoints

#### GET /tasks
**Purpose**: Retrieve list of tasks  
**Parameters**:
```
status  : String (optional) - Filter by status
college : Long (optional)   - Filter by college ID
```
**Valid Status Values**: PENDING, IN_PROGRESS, COMPLETED, OVERDUE  
**Example**: `/tasks?status=PENDING&college=1`

#### POST /tasks/save
**Purpose**: Save new or update task  
**Parameters**:
```
task.title        : String     - Task title (required)
task.description  : String     - Description (optional)
task.taskType     : TaskType   - Type enum (required)
task.status       : TaskStatus - Status enum (optional)
task.dueDate      : LocalDate  - Deadline (required)
task.priority     : String     - HIGH/MEDIUM/LOW (required)
employeeId        : Long       - Employee ID (optional)
```
**Returns**: Redirect to /tasks  
**Auto-Behavior**: PENDING tasks past due date auto-marked OVERDUE

#### GET /tasks/edit/{id}
**Purpose**: Get task edit form  
**Parameters**:
```
id : Long - Task ID (path variable)
```
**Returns**: Form template with task data

#### GET /tasks/view/{id}
**Purpose**: View task details  
**Parameters**:
```
id : Long - Task ID (path variable)
```
**Returns**: Task detail page

#### POST /tasks/updateStatus/{id}
**Purpose**: Update task status  
**Parameters**:
```
id     : Long   - Task ID (path variable)
status : String - New status (PENDING, IN_PROGRESS, COMPLETED, OVERDUE)
```
**Returns**: Redirect to task view  
**Validation**: Only valid statuses accepted

#### GET /tasks/delete/{id}
**Purpose**: Delete task  
**Parameters**:
```
id : Long - Task ID (path variable)
```
**Returns**: Redirect to /tasks

---

## Service Layer Reference

### EmployeeService

```java
@Service
public class EmployeeService {
    
    // Get all employees
    public List<Employee> getAllEmployees()
    // Returns: all employees from database
    
    // Get employee by ID
    public Optional<Employee> getEmployeeById(Long id)
    // Returns: Optional containing employee or empty
    
    // Save employee (new or update)
    public Employee saveEmployee(Employee employee)
    // Returns: saved employee with ID
    
    // Delete employee
    public void deleteEmployee(Long id)
    // Returns: void, cascades delete tasks
    
    // Search by name
    public List<Employee> searchByName(String name)
    // Case-insensitive, partial match
    // Returns: matching employees
    
    // Search by department
    public List<Employee> searchByDepartment(String department)
    // Returns: employees in that department
    
    // Check if email exists
    public boolean emailExists(String email)
    // Returns: true if email already used
    
    // Get total employee count
    public long getTotalEmployees()
    // Returns: count of all employees
    
    // Get employees by college
    public List<Employee> getEmployeesByCollege(Long collegeId)
    // Returns: employees belonging to college
}
```

### TaskService

```java
@Service
public class TaskService {
    
    // Get all tasks
    public List<Task> getAllTasks()
    // Returns: all tasks from database
    
    // Get task by ID
    public Optional<Task> getTaskById(Long id)
    // Returns: Optional containing task
    
    // Save task with auto-overdue logic
    public Task saveTask(Task task)
    // Auto-marks PENDING tasks past due as OVERDUE
    // Returns: saved task with ID
    
    // Delete task
    public void deleteTask(Long id)
    // Returns: void
    
    // Get tasks by employee
    public List<Task> getTasksByEmployee(Long employeeId)
    // Ordered by due date ascending
    // Returns: tasks assigned to employee
    
    // Get tasks by status
    public List<Task> getTasksByStatus(TaskStatus status)
    // Returns: all tasks with specified status
    
    // Assign task to employee
    public Task assignTaskToEmployee(Task task, Long employeeId)
    // Sets employee and saves task
    // Returns: saved task
    
    // Update task status
    public Task updateTaskStatus(Long taskId, TaskStatus status)
    // Changes status and persists
    // Returns: updated task
    
    // Count tasks by status
    public long countByStatus(TaskStatus status)
    // Returns: count of tasks with status
    
    // Count tasks by college and status
    public long countByCollegeAndStatus(Long collegeId, TaskStatus status)
    // Returns: count for specific college and status
    
    // Count tasks by college
    public long countByCollege(Long collegeId)
    // Returns: total tasks in college
    
    // Get total task count
    public long getTotalTasks()
    // Returns: count of all tasks
    
    // Get tasks by college
    public List<Task> getTasksByCollege(Long collegeId)
    // Ordered by due date descending
    // Returns: all tasks in college
}
```

### CollegeService

```java
@Service
public class CollegeService {
    
    // Get all colleges
    public List<College> getAllColleges()
    // Returns: all colleges
    
    // Get college by ID
    public Optional<College> getCollegeById(Long id)
    // Returns: Optional containing college
    
    // Save college
    public College saveCollege(College college)
    // Returns: saved college with ID
    
    // Delete college (cascades?)
    public void deleteCollege(Long id)
    // Returns: void
    
    // Get college by name
    public Optional<College> getCollegeByName(String name)
    // Case-insensitive search
    // Returns: Optional containing college
}
```

---

## Repository Methods

### EmployeeRepository

```java
@Repository
public interface EmployeeRepository extends JpaRepository<Employee, Long> {
    
    // Search by name (case-insensitive, partial match)
    List<Employee> findByNameContainingIgnoreCase(String name);
    // Example: findByNameContainingIgnoreCase("raj") → matches "Rajesh"
    
    // Search by department
    List<Employee> findByDepartmentIgnoreCase(String department);
    // Example: findByDepartmentIgnoreCase("Engineering")
    
    // Check email uniqueness
    boolean existsByEmail(String email);
    // Example: existsByEmail("test@college.edu") → returns true/false
    
    // Get employees by college
    List<Employee> findByCollegeId(Long collegeId);
    // Example: findByCollegeId(1) → all Engineering employees
}
```

### TaskRepository

```java
@Repository
public interface TaskRepository extends JpaRepository<Task, Long> {
    
    // Get tasks by employee
    List<Task> findByEmployeeId(Long employeeId);
    
    // Get tasks by status
    List<Task> findByStatus(TaskStatus status);
    
    // Get tasks by employee and status
    List<Task> findByEmployeeIdAndStatus(Long employeeId, TaskStatus status);
    
    // Custom: Count tasks by status
    @Query("SELECT COUNT(t) FROM Task t WHERE t.status = ?1")
    long countByStatus(TaskStatus status);
    
    // Custom: Get tasks by employee, ordered by due date
    @Query("SELECT t FROM Task t WHERE t.employee.id = ?1 ORDER BY t.dueDate ASC")
    List<Task> findByEmployeeIdOrderByDueDate(Long employeeId);
    
    // Custom: Count tasks by college and status
    @Query("SELECT COUNT(t) FROM Task t WHERE t.employee.college.id = ?1 AND t.status = ?2")
    long countByCollegeIdAndStatus(Long collegeId, TaskStatus status);
    
    // Custom: Count tasks by college
    @Query("SELECT COUNT(t) FROM Task t WHERE t.employee.college.id = ?1")
    long countByCollegeId(Long collegeId);
    
    // Custom: Get all tasks by college, ordered by due date
    @Query("SELECT t FROM Task t WHERE t.employee.college.id = ?1 ORDER BY t.dueDate DESC")
    List<Task> findByCollegeId(Long collegeId);
}
```

### CollegeRepository

```java
@Repository
public interface CollegeRepository extends JpaRepository<College, Long> {
    
    // Find college by name (case-insensitive)
    Optional<College> findByNameIgnoreCase(String name);
    // Example: findByNameIgnoreCase("engineering") → returns Engineering college
}
```

---

## Database Queries

### Sample JPQL Queries Used

**Show all colleges**:
```java
String sql = "SELECT c FROM College c";
// Repository method: collegeRepository.findAll()
```

**Show employees in Engineering**:
```java
String sql = "SELECT e FROM Employee e WHERE e.college.id = ?1";
// Repository method: employeeRepository.findByCollegeId(1)
```

**Count pending tasks**:
```java
String sql = "SELECT COUNT(t) FROM Task t WHERE t.status = 'PENDING'";
// Repository method: taskRepository.countByStatus(TaskStatus.PENDING)
```

**Get overdue tasks**:
```java
String sql = "SELECT t FROM Task t WHERE t.status = 'OVERDUE' ORDER BY t.dueDate ASC";
// Repository method: taskRepository.findByStatus(TaskStatus.OVERDUE)
```

**Complex: Count completed tasks in specific college**:
```java
String sql = "SELECT COUNT(t) FROM Task t " +
             "WHERE t.employee.college.id = ?1 AND t.status = 'COMPLETED'";
// Repository method: 
// taskRepository.countByCollegeIdAndStatus(collegeId, TaskStatus.COMPLETED)
```

---

## Adding New Features

### How to Add a New Task Type

**Step 1: Update Task.java**
```java
public enum TaskType {
    EXAM_DUTY, PROJECT_GUIDE, DOCUMENTATION, 
    EVENT_COORDINATION, REPORT_SUBMISSION, OTHER,
    NEW_TYPE_HERE    // ← Add here
}
```

**Step 2: Database migration** (if using persistent DB)
```sql
-- No changes needed for H2 (enum stored as string)
-- For real DB, alter table if necessary
```

**Step 3: Update form.html**
```html
<select th:field="*{taskType}">
    <option value="">Select Type</option>
    <option th:each="type : ${taskTypes}" 
            th:value="${type}" 
            th:text="${type.name().replace('_', ' ')}">
        <!-- Option will auto-generate from enum -->
    </option>
</select>
```

**Step 4: No other changes needed!**
- UI automatically includes new type
- Database handles enum values
- Service layer unchanged

### How to Add a New Report/Dashboard Widget

**Step 1: Add method to appropriate Service**
```java
// In TaskService.java
public long countByTypeAndStatus(TaskType type, TaskStatus status) {
    // Implementation
}
```

**Step 2: Add repository method**
```java
// In TaskRepository.java
@Query("SELECT COUNT(t) FROM Task t WHERE t.taskType = ?1 AND t.status = ?2")
long countByTypeAndStatus(TaskType type, TaskStatus status);
```

**Step 3: Add controller mapping**
```java
// In HomeController.java
@GetMapping("/report/by-type")
public String reportByType(Model model) {
    model.addAttribute("examDutyComplete", taskService.countByTypeAndStatus(
        TaskType.EXAM_DUTY, TaskStatus.COMPLETED));
    // Add more counts
    return "report/by-type";
}
```

**Step 4: Create template**
```html
<!-- Create src/main/resources/templates/report/by-type.html -->
<h2>Tasks by Type</h2>
<p>Exam Duties Completed: <span th:text="${examDutyComplete}"></span></p>
```

### How to Add Search/Filter

**Step 1: Add repository method**
```java
public interface TaskRepository extends JpaRepository<Task, Long> {
    List<Task> findByPriority(String priority);
}
```

**Step 2: Add service method**
```java
public List<Task> getTasksByPriority(String priority) {
    return taskRepository.findByPriority(priority);
}
```

**Step 3: Update controller**
```java
@GetMapping("/tasks")
public String listTasks(Model model, @RequestParam(required = false) String priority) {
    if (priority != null) {
        model.addAttribute("tasks", taskService.getTasksByPriority(priority));
    } else {
        model.addAttribute("tasks", taskService.getAllTasks());
    }
    return "tasks/list";
}
```

**Step 4: Update template**
```html
<form method="get" action="/tasks">
    <select name="priority">
        <option value="">All Priorities</option>
        <option value="HIGH">High</option>
        <option value="MEDIUM">Medium</option>
        <option value="LOW">Low</option>
    </select>
    <button type="submit">Filter</button>
</form>
```

---

## Troubleshooting & Debugging

### Common Issues

**1. Application won't start**
```
Error: Cannot find port 8080
Solution: 
  - Port already in use: lsof -i :8080 (kill process)
  - Change port in application.properties: server.port=8081
```

**2. Compilation error**
```
Error: The type List is not generic
Solution:
  - Ensure Java 17+ is used
  - Rebuild: mvn clean compile
```

**3. No data showing**
```
Error: Employees/tasks list shows empty
Solution:
  - Check data.sql exists in resources
  - Verify spring.sql.init.mode=always in properties
  - H2 reset: Restart application (it's in-memory)
```

**4. Form validation not working**
```
Error: Form submits with empty required fields
Solution:
  - Clear browser cache
  - Check @NotBlank annotations exist on model
  - Verify BindingResult parameter in controller
```

### Debugging Techniques

**1. Enable SQL Logging**
```properties
# In application.properties
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
logging.level.org.hibernate.SQL=DEBUG
```

**2. Debug breakpoints in IDE**
```java
// IntelliJ: Click line number, set breakpoint
// Debug mode: Run → Debug Application
// Inspect variables: F4
```

**3. Check H2 Console**
```
1. Navigate to http://localhost:8080/h2-console
2. Login with credentials: sa / (blank password)
3. Run SQL queries to verify data
4. Check table schemas
```

**4. View server logs**
```
# Terminal shows:
- Application startup messages
- HTTP request logs
- SQL query logs
- Error stack traces
```

**5. Browser DevTools**
```
F12 → Network tab:
  - Check HTTP status codes
  - View request/response payloads
  - Check timing

F12 → Console tab:
  - JavaScript errors
  - Network errors
```

### Testing in H2 Console

```sql
-- Check available colleges
SELECT * FROM colleges;

-- Count employees per college
SELECT college_id, COUNT(*) FROM employees GROUP BY college_id;

-- Find tasks by status
SELECT * FROM tasks WHERE status = 'PENDING';

-- Complex query: Employees with >2 tasks
SELECT e.id, e.name, COUNT(t.id) as task_count 
FROM employees e 
LEFT JOIN tasks t ON e.id = t.employee_id 
GROUP BY e.id 
HAVING COUNT(t.id) > 2;
```

---

## Performance Tips

1. **Optimize queries**: Use lazy loading for relationships
2. **Index frequently searched columns**: Email is indexed (unique)
3. **Pagination**: For large datasets, add pagination to repositories
4. **Caching**: Consider Spring Cache for dashboard statistics
5. **Database**: When scaling, migrate from H2 to MySQL/PostgreSQL

---

## Code Style Guidelines

```java
// Service method naming
public List<Task> getTasksByStatus() { }  // getter
public Task saveTask(Task t) { }          // setter
public void deleteTask(Long id) { }       // action

// Java naming conventions
private String firstName;     // camelCase for variables
public final int MAX_SIZE;    // UPPER_CASE for constants
TaskStatus.PENDING;           // UPPER_CASE for enums

// Repository method naming
findBy<Property>()            // Exact match
findBy<Property>Containing()  // Partial match
find<Entity>By<Property>()    // Multiple results
countBy<Property>()           // Returns count
existsBy<Property>()          // Returns boolean
```

---

**Document Version**: 1.0  
**For**: Developers & Technical Teams  
**Updated**: April 2026
