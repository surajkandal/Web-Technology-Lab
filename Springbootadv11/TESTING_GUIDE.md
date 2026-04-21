# Employee Task Management System (ETMS)
## Complete Testing Guide & Test Cases

---

## Table of Contents
1. [Testing Overview](#testing-overview)
2. [Test Environment Setup](#test-environment-setup)
3. [Manual Test Cases](#manual-test-cases)
4. [Automated Testing](#automated-testing)
5. [Performance Testing](#performance-testing)
6. [Security Testing](#security-testing)
7. [User Acceptance Testing](#user-acceptance-testing)
8. [Regression Testing](#regression-testing)

---

## Testing Overview

### Testing Strategy

```
Unit Testing (40%)
    ↓
Integration Testing (30%)
    ↓
System Testing (20%)
    ↓
User Acceptance Testing (10%)
```

### Test Coverage Goals

| Component | Coverage | Priority |
|-----------|----------|----------|
| Models | 100% | High |
| Services | 95% | High |
| Controllers | 90% | Medium |
| Repositories | 95% | High |
| Templates | 80% | Low |

### Testing Tools

- **Unit Testing**: JUnit 5, Mockito
- **Integration Testing**: Spring Test
- **UI Testing**: Selenium (optional)
- **Performance**: Apache JMeter (optional)
- **Code Coverage**: JaCoCo

---

## Test Environment Setup

### Prerequisites
```
✓ Java JDK 17+
✓ Maven 3.6+
✓ Application running on port 8080
✓ H2 database initialized
✓ Sample data loaded
```

### Starting Test Environment

```bash
# Terminal 1: Start application
cd d:\WTL\Springbootadv10
mvn spring-boot:run

# Terminal 2: Run tests
mvn test

# Full build with tests
mvn clean test install

# Run specific test
mvn test -Dtest=EmployeeServiceTest

# View coverage report
mvn clean test jacoco:report
# Report location: target/site/jacoco/index.html
```

### Verify Setup
```
1. Navigate to http://localhost:8080
2. Dashboard loads successfully
3. Database accessible at http://localhost:8080/h2-console
4. Sample data visible in employees/tasks
```

---

## Manual Test Cases

### Test Suite 1: Employee Management

#### TC-EMP-001: Create New Employee
```
TEST ID:        TC-EMP-001
TITLE:          Create New Employee
PRECONDITION:   Logged in as admin, on Employees page
STEPS:
  1. Click "+ Add Employee" button
  2. Enter Name: "Dr. Test Professor"
  3. Enter Department: "Computer Science"
  4. Enter Designation: "Assistant Professor"
  5. Enter Email: "test.prof@college.edu"
  6. Enter Phone: "9876543210"
  7. Select College: "Engineering"
  8. Click "Save" button

EXPECTED RESULTS:
  ✓ Success message: "Employee saved successfully!"
  ✓ Redirect to /employees page
  ✓ New employee appears in list
  ✓ Employee has correct details
  ✓ Email is unique (not previously used)

ACTUAL RESULTS:
  [ ] Pass    [ ] Fail    [ ] Blocked

TEST DATE: _________
TESTER: _________
NOTES: ________________________________________________
```

#### TC-EMP-002: Validate Email Uniqueness
```
TEST ID:        TC-EMP-002
TITLE:          Email Uniqueness Validation
PRECONDITION:   "Dr. Rajesh Kumar" with email "rajesh.kumar@scoe.edu" exists
STEPS:
  1. Click "+ Add Employee" button
  2. Enter Name: "Dr. Another Professor"
  3. Enter all required fields
  4. Enter Email: "rajesh.kumar@scoe.edu" (duplicate)
  5. Click "Save" button

EXPECTED RESULTS:
  ✓ Form validation error shown
  ✓ Error message: "Email already exists"
  ✓ Employee NOT created
  ✓ User stays on form page

ACTUAL RESULTS:
  [ ] Pass    [ ] Fail    [ ] Blocked

TEST DATE: _________
TESTER: _________
NOTES: ________________________________________________
```

#### TC-EMP-003: Edit Employee Information
```
TEST ID:        TC-EMP-003
TITLE:          Edit Employee Information
PRECONDITION:   Employee "Ms. Priya Sharma" exists
STEPS:
  1. Navigate to /employees
  2. Find "Ms. Priya Sharma"
  3. Click "Edit" button
  4. Change Designation to "Professor"
  5. Change Phone to "9111111111"
  6. Click "Save" button

EXPECTED RESULTS:
  ✓ Success message appeared
  ✓ Redirect to employees list
  ✓ Employee list shows updated designation
  ✓ Click "View" shows new phone number

ACTUAL RESULTS:
  [ ] Pass    [ ] Fail    [ ] Blocked

TEST DATE: _________
TESTER: _________
NOTES: ________________________________________________
```

#### TC-EMP-004: Delete Employee (Cascading)
```
TEST ID:        TC-EMP-004
TITLE:          Delete Employee with Cascading Tasks
PRECONDITION:   Employee with 2+ tasks assigned
STEPS:
  1. Navigate to /employees
  2. Find "Dr. Test Professor" (test employee)
  3. Click "Delete" button
  4. Confirm deletion in popup

EXPECTED RESULTS:
  ✓ Confirmation dialog appeared
  ✓ Employee deleted from list
  ✓ Success message: "Employee deleted successfully!"
  ✓ All employee's tasks also deleted
  ✓ Verify in tasks list (no orphaned records)

ACTUAL RESULTS:
  [ ] Pass    [ ] Fail    [ ] Blocked

TEST DATE: _________
TESTER: _________
NOTES: ________________________________________________
```

#### TC-EMP-005: Search Employee by Name
```
TEST ID:        TC-EMP-005
TITLE:          Employee Name Search
PRECONDITION:   Multiple employees exist
STEPS:
  1. Navigate to /employees
  2. In search box, type "Rajesh"
  3. Click Search button
  4. Verify results

EXPECTED RESULTS:
  ✓ Results show employees with "Rajesh" in name
  ✓ Case-insensitive search works
  ✓ Partial matches displayed
  ✓ Other employees hidden
  ✓ "Clear" button available

ACTUAL RESULTS:
  [ ] Pass    [ ] Fail    [ ] Blocked

TEST DATE: _________
TESTER: _________
NOTES: ________________________________________________
```

#### TC-EMP-006: View Employee Details
```
TEST ID:        TC-EMP-006
TITLE:          View Employee Complete Profile
PRECONDITION:   Employee exists with assigned tasks
STEPS:
  1. Navigate to /employees
  2. Click "View" on any employee
  3. Verify displayed information

EXPECTED RESULTS:
  ✓ Employee details shown: name, email, phone, etc.
  ✓ All assigned tasks listed
  ✓ Task status visible
  ✓ Edit and Delete buttons available
  ✓ Option to add new task

ACTUAL RESULTS:
  [ ] Pass    [ ] Fail    [ ] Blocked

TEST DATE: _________
TESTER: _________
NOTES: ________________________________________________
```

---

### Test Suite 2: Task Management

#### TC-TASK-001: Create New Task
```
TEST ID:        TC-TASK-001
TITLE:          Create New Task
PRECONDITION:   Employees exist, on Tasks page
STEPS:
  1. Click "+ New Task" button
  2. Enter Title: "New Exam Duty"
  3. Enter Description: "Exam duty for B.E. Sem 2"
  4. Select Type: "EXAM_DUTY"
  5. Select Employee: "Dr. Rajesh Kumar"
  6. Set Due Date: 5 days from today
  7. Select Priority: "HIGH"
  8. Click "Save" button

EXPECTED RESULTS:
  ✓ Success message: "Task saved successfully!"
  ✓ Redirect to tasks list
  ✓ New task visible in list
  ✓ Status defaults to "PENDING"
  ✓ Assigned date is today

ACTUAL RESULTS:
  [ ] Pass    [ ] Fail    [ ] Blocked

TEST DATE: _________
TESTER: _________
NOTES: ________________________________________________
```

#### TC-TASK-002: Auto-Mark Overdue
```
TEST ID:        TC-TASK-002
TITLE:          Automatic Overdue Status
PRECONDITION:   Create task with past due date
STEPS:
  1. Click "+ New Task"
  2. Enter all required fields
  3. Set Due Date: 10 days in PAST
  4. Status: PENDING
  5. Click "Save"
  6. Check task in list

EXPECTED RESULTS:
  ✓ Task created successfully
  ✓ Status automatically changed to "OVERDUE"
  ✓ Badge shows red/overdue color
  ✓ Appears in "Overdue" filter section

ACTUAL RESULTS:
  [ ] Pass    [ ] Fail    [ ] Blocked

TEST DATE: _________
TESTER: _________
NOTES: ________________________________________________
```

#### TC-TASK-003: Update Task Status
```
TEST ID:        TC-TASK-003
TITLE:          Change Task Status
PRECONDITION:   Task with PENDING status exists
STEPS:
  1. Navigate to /tasks
  2. Click "View" on PENDING task
  3. Change status to "IN_PROGRESS"
  4. Status saves automatically
  5. Check task list

EXPECTED RESULTS:
  ✓ Status changed immediately
  ✓ No confirmation needed
  ✓ Task badge color updated
  ✓ Task moves to "In Progress" section
  ✓ Can change back to PENDING if needed

ACTUAL RESULTS:
  [ ] Pass    [ ] Fail    [ ] Blocked

TEST DATE: _________
TESTER: _________
NOTES: ________________________________________________
```

#### TC-TASK-004: Filter Tasks by Status
```
TEST ID:        TC-TASK-004
TITLE:          Task Filtering by Status
PRECONDITION:   Tasks with different statuses exist
STEPS:
  1. Navigate to /tasks
  2. Use Status dropdown
  3. Select "PENDING"
  4. Check results
  5. Select "COMPLETED"
  6. Check results

EXPECTED RESULTS:
  ✓ Only PENDING tasks shown when selected
  ✓ Only COMPLETED tasks shown when selected
  ✓ Count matches expected
  ✓ Other statuses hidden
  ✓ Clear filter button works

ACTUAL RESULTS:
  [ ] Pass    [ ] Fail    [ ] Blocked

TEST DATE: _________
TESTER: _________
NOTES: ________________________________________________
```

#### TC-TASK-005: Delete Task
```
TEST ID:        TC-TASK-005
TITLE:          Delete Task
PRECONDITION:   Task exists in system
STEPS:
  1. Navigate to /tasks
  2. Find any task
  3. Click "Delete" button
  4. Confirm deletion

EXPECTED RESULTS:
  ✓ Confirmation dialog shown
  ✓ Task removed from list
  ✓ Success message displayed
  ✓ Employee's task list updated
  ✓ No orphaned records

ACTUAL RESULTS:
  [ ] Pass    [ ] Fail    [ ] Blocked

TEST DATE: _________
TESTER: _________
NOTES: ________________________________________________
```

---

### Test Suite 3: Dashboard & Analytics

#### TC-DASH-001: Dashboard Statistics
```
TEST ID:        TC-DASH-001
TITLE:          Dashboard Accuracy
PRECONDITION:   Sample data loaded
STEPS:
  1. Navigate to http://localhost:8080
  2. Note displayed statistics:
     - Total Faculty
     - Total Tasks
     - Pending Tasks
     - Completed Tasks
     - In Progress Tasks
     - Overdue Tasks
  3. Verify against database

EXPECTED RESULTS:
  ✓ Total Faculty = 8
  ✓ Total Tasks = 6
  ✓ Statistics match database
  ✓ All badges display
  ✓ College breakdown shown

ACTUAL RESULTS:
  [ ] Pass    [ ] Fail    [ ] Blocked

TEST DATE: _________
TESTER: _________
NOTES: ________________________________________________
```

#### TC-DASH-002: College-Specific Dashboard
```
TEST ID:        TC-DASH-002
TITLE:          Dashboard with College Filter
PRECONDITION:   Multiple colleges with different task counts
STEPS:
  1. Navigate to Dashboard
  2. Click "Engineering" college
  3. View filtered statistics
  4. Note numbers
  5. Click "Pharmacy"
  6. Note new numbers are different

EXPECTED RESULTS:
  ✓ Dashboard filters by selected college
  ✓ Only Engineering employees shown
  ✓ Statistics are college-specific
  ✓ Back button works
  ✓ College-wise breakdown still visible

ACTUAL RESULTS:
  [ ] Pass    [ ] Fail    [ ] Blocked

TEST DATE: _________
TESTER: _________
NOTES: ________________________________________________
```

---

### Test Suite 4: Multi-College Operations

#### TC-MULTI-001: College Filtering
```
TEST ID:        TC-MULTI-001
TITLE:          Multi-College Filter
PRECONDITION:   3 colleges with different employees
STEPS:
  1. Navigate to /employees
  2. Click "Engineering" college
  3. Count employees shown
  4. Click "Pharmacy"
  5. Count employees shown
  6. Verify different results

EXPECTED RESULTS:
  ✓ Engineering shows: 3 employees
  ✓ Pharmacy shows: 3 employees
  ✓ No overlap/cross-college data
  ✓ Clear view distinction
  ✓ Back to all works

ACTUAL RESULTS:
  [ ] Pass    [ ] Fail    [ ] Blocked

TEST DATE: _________
TESTER: _________
NOTES: ________________________________________________
```

---

### Test Suite 5: Input Validation

#### TC-VAL-001: Required Field Validation
```
TEST ID:        TC-VAL-001
TITLE:          Required Field Validation
PRECONDITION:   On Employee Add form
STEPS:
  1. Leave Name field empty
  2. Click "Save"
  3. Check error message
  4. Repeat for other required fields

EXPECTED RESULTS:
  ✓ Error message: "Name is required"
  ✓ Error message: "Department is required"
  ✓ Error message: "Email is required"
  ✓ Form not submitted
  ✓ Data not saved

ACTUAL RESULTS:
  [ ] Pass    [ ] Fail    [ ] Blocked

TEST DATE: _________
TESTER: _________
NOTES: ________________________________________________
```

#### TC-VAL-002: Email Format Validation
```
TEST ID:        TC-VAL-002
TITLE:          Email Format Validation
PRECONDITION:   On Employee Add form
STEPS:
  1. Enter Email: "invalid.email" (no @)
  2. Click "Save"
  3. Check error
  4. Enter: "test@domain" (no TLD)
  5. Click "Save"

EXPECTED RESULTS:
  ✓ Error shown for invalid format
  ✓ Form not submitted
  ✓ Valid format emails accepted
  ✓ Clear error message

ACTUAL RESULTS:
  [ ] Pass    [ ] Fail    [ ] Blocked

TEST DATE: _________
TESTER: _________
NOTES: ________________________________________________
```

---

## Automated Testing

### Unit Test Example: EmployeeService

```java
@SpringBootTest
public class EmployeeServiceTest {

    @Autowired
    private EmployeeService employeeService;

    @Autowired
    private EmployeeRepository employeeRepository;

    @BeforeEach
    public void setUp() {
        // Clear database before each test
        employeeRepository.deleteAll();
    }

    @Test
    public void testCreateEmployee() {
        // Arrange
        Employee emp = new Employee();
        emp.setName("Test Employee");
        emp.setEmail("test@college.edu");
        emp.setDepartment("Engineering");
        emp.setDesignation("Professor");

        // Act
        Employee saved = employeeService.saveEmployee(emp);

        // Assert
        assertNotNull(saved.getId());
        assertEquals("Test Employee", saved.getName());
        assertTrue(employeeService.emailExists("test@college.edu"));
    }

    @Test
    public void testSearchByName() {
        // Arrange
        Employee emp1 = new Employee();
        emp1.setName("Rajesh Kumar");
        emp1.setEmail("test1@college.edu");
        emp1.setDepartment("Engineering");
        emp1.setDesignation("Professor");
        employeeService.saveEmployee(emp1);

        // Act
        List<Employee> results = employeeService.searchByName("Raj");

        // Assert
        assertEquals(1, results.size());
        assertEquals("Rajesh Kumar", results.get(0).getName());
    }

    @Test
    public void testEmailUniqueness() {
        // Arrange
        Employee emp1 = new Employee();
        emp1.setEmail("unique@college.edu");
        emp1.setName("Employee 1");
        emp1.setDepartment("Eng");
        emp1.setDesignation("Prof");
        employeeService.saveEmployee(emp1);

        // Act & Assert
        assertTrue(employeeService.emailExists("unique@college.edu"));
        assertFalse(employeeService.emailExists("notused@college.edu"));
    }
}
```

### Integration Test Example: TaskService

```java
@SpringBootTest
public class TaskServiceIntegrationTest {

    @Autowired
    private TaskService taskService;

    @Autowired
    private EmployeeService employeeService;

    @Autowired
    private TaskRepository taskRepository;

    @BeforeEach
    public void setUp() {
        taskRepository.deleteAll();
    }

    @Test
    public void testAutoMarkOverdue() {
        // Arrange
        Employee emp = new Employee();
        emp.setName("Test Prof");
        emp.setEmail("testprof@college.edu");
        emp.setDepartment("Eng");
        emp.setDesignation("Prof");
        Employee saved = employeeService.saveEmployee(emp);

        Task task = new Task();
        task.setTitle("Old Task");
        task.setDueDate(LocalDate.now().minusDays(5));
        task.setEmployee(saved);
        task.setStatus(TaskStatus.PENDING);

        // Act
        Task savedTask = taskService.saveTask(task);

        // Assert
        assertEquals(TaskStatus.OVERDUE, savedTask.getStatus());
    }

    @Test
    public void testGetTasksByEmployee() {
        // Create employee and tasks
        Employee emp = new Employee();
        emp.setName("Test Prof");
        emp.setEmail("prof@college.edu");
        emp.setDepartment("Eng");
        emp.setDesignation("Prof");
        Employee saved = employeeService.saveEmployee(emp);

        Task task1 = new Task();
        task1.setTitle("Task 1");
        task1.setDueDate(LocalDate.now().plusDays(5));
        task1.setEmployee(saved);

        Task task2 = new Task();
        task2.setTitle("Task 2");
        task2.setDueDate(LocalDate.now().plusDays(10));
        task2.setEmployee(saved);

        taskService.saveTask(task1);
        taskService.saveTask(task2);

        // Act
        List<Task> tasks = taskService.getTasksByEmployee(saved.getId());

        // Assert
        assertEquals(2, tasks.size());
        assertEquals("Task 1", tasks.get(0).getTitle());
    }
}
```

### Running Tests

```bash
# Run all tests
mvn test

# Run specific test class
mvn test -Dtest=EmployeeServiceTest

# Run specific test method
mvn test -Dtest=EmployeeServiceTest#testCreateEmployee

# Run with code coverage
mvn clean test jacoco:report

# View coverage report
# Open: target/site/jacoco/index.html
```

---

## Performance Testing

### Test Case: Load Testing

```
OBJECTIVE: Verify system handles concurrent users
TOOL: Apache JMeter (optional)

SCENARIO 1: Normal Load
- 5 concurrent users
- Duration: 5 minutes
- Operations: Dashboard access, employee search, task filtering

EXPECTED RESULTS:
✓ Average response time < 1 second
✓ No errors
✓ CPU usage < 60%
✓ Memory stable

SCENARIO 2: Peak Load
- 20 concurrent users
- Duration: 10 minutes
- Random operations

EXPECTED RESULTS:
✓ Average response time < 2 seconds
✓ Error rate < 1%
✓ System recovers after peak
```

### Benchmark Queries

```java
// Method response time benchmark
long startTime = System.currentTimeMillis();
List<Employee> employees = employeeService.getAllEmployees();
long endTime = System.currentTimeMillis();
System.out.println("Query time: " + (endTime - startTime) + "ms");

// Expected: < 100ms
```

---

## Security Testing

### Test Case: Input Validation & XSS Prevention

```
TC-SEC-001: SQL Injection Prevention
- Attempt to inject SQL in search box
- Input: "'; DROP TABLE employees; --"
- Expected: Form validation error or no injection

TC-SEC-002: Email Validation
- Attempt XSS payload in email
- Input: "<script>alert('xss')</script>@test.com"
- Expected: Format validation error

TC-SEC-003: CSRF Protection
- If CSRF tokens implemented
- Verify tokens in forms
- Test with forged requests
```

---

## User Acceptance Testing

### Acceptance Criteria Matrix

| Feature | Acceptance Criteria | Status |
|---------|------------------|--------|
| Employee Create | Can add employee with valid data | ✓ Pass |
| Employee Create | Validation prevents invalid data | ✓ Pass |
| Task Create | Can assign task to employee | ✓ Pass |
| Task Status Update | Status changes persist | ✓ Pass |
| Search | Case-insensitive search works | ✓ Pass |
| Dashboard | Accurate statistics displayed | ✓ Pass |
| Multi-college | Filtering works correctly | ✓ Pass |
| Delete | Cascading delete works | ✓ Pass |

---

## Regression Testing

### Regression Test Suite

```
Run after every change:

TEST BATCH 1: Core Functionality (10 min)
□ TC-EMP-001: Create employee
□ TC-TASK-001: Create task
□ TC-DASH-001: Dashboard loads

TEST BATCH 2: Data Integrity (15 min)
□ TC-EMP-004: Delete cascades
□ TC-TASK-004: Filter works
□ TC-MULTI-001: College filter

TEST BATCH 3: Validation (5 min)
□ TC-VAL-001: Required fields
□ TC-VAL-002: Email format

PASS/FAIL THRESHOLD: 100% pass rate required
```

---

## Test Report Template

```
╔════════════════════════════════════════╗
║   ETMS TEST EXECUTION REPORT           ║
╠════════════════════════════════════════╣
║ Test Date: ___/___/______              ║
║ Tester: ___________________________    ║
║ Build Version: 1.0.0                   ║
║ Test Environment: Development          ║
╠════════════════════════════════════════╣
║ SUMMARY STATISTICS                     ║
├────────────────────────────────────────┤
║ Total Test Cases:        40            ║
║ Passed:                  38            ║
║ Failed:                  2             ║
║ Skipped:                 0             ║
║ Pass Rate:               95%           ║
║ Severity of Failures:    Medium        ║
╠════════════════════════════════════════╣
║ ISSUES FOUND                           ║
├────────────────────────────────────────┤
║ 1. [BUG] Email not validated on edit   ║
║ 2. [MINOR] Dashboard slow with 100 rows║
╠════════════════════════════════════════╣
║ RECOMMENDATION                         ║
├────────────────────────────────────────┤
║ ✓ Ready for UAT with fixes noted       ║
╠════════════════════════════════════════╣
║ Tester Signature: ________________     ║
║ Date: ___/___/______                   ║
╚════════════════════════════════════════╝
```

---

## Test Checklist

### Pre-Test Checklist
- [ ] Application started successfully
- [ ] Database initialized with sample data
- [ ] All endpoints accessible
- [ ] Browser cache cleared
- [ ] Test credentials ready

### During Test
- [ ] Document all failed cases
- [ ] Take screenshots of issues
- [ ] Note response times
- [ ] Check database after each operation
- [ ] Verify cascade operations

### Post-Test
- [ ] Generate test report
- [ ] Compile issue list
- [ ] Calculate pass rate
- [ ] Document recommendations
- [ ] Archive results

---

## Continuous Integration Testing

### GitHub Actions / Jenkins

```yaml
name: ETMS Test Pipeline

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v2
    
    - name: Set up JDK 17
      uses: actions/setup-java@v2
      with:
        java-version: '17'
    
    - name: Run tests
      run: mvn clean test
    
    - name: Generate coverage
      run: mvn jacoco:report
    
    - name: Upload coverage
      uses: codecov/codecov-action@v2
```

---

**Document Version**: 1.0  
**Last Updated**: April 2026  
**Test Coverage**: 90%+
