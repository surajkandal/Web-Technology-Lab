# Employee Task Management System
## Quick Start & User Manual

---

## 🚀 Quick Start (5 Minutes)

### 1. Start the Application
```bash
cd d:\WTL\Springbootadv10
mvn spring-boot:run
```

**Wait for message**: `Started EmployeeTaskManagementApplication in X seconds`

### 2. Open in Browser
- URL: **http://localhost:8080**
- You should see the dashboard automatically

### 3. Your First Task
```
1. Click on "Engineering" in left sidebar
2. See all Engineering faculty
3. Look for "Dr. Rajesh Kumar"
4. Click the "+Task" button next to his name
5. Enter task details:
   - Title: "Midterm Exam Duty"
   - Type: "EXAM_DUTY"
   - Due Date: Any date in the future
   - Priority: "HIGH"
6. Click "Save"
7. Task added successfully! ✓
```

---

## 📊 Dashboard Overview

The main dashboard shows:

| Card | Shows |
|------|-------|
| 👥 Faculty Count | Total employees in system or college |
| 📋 Total Tasks | All tasks assigned |
| ⏳ Pending Tasks | Tasks not started yet |
| ✅ Completed Tasks | Finished tasks |
| ⚙️ In Progress | Tasks being worked on |
| ⚠️ Overdue Tasks | Missed deadline tasks |

**College Breakdown Grid**: 
- Click any college name to see detailed view
- Shows task counts per college
- Compare college performance

---

## 👥 Employee Management

### To View All Faculty

1. Click **"Employees"** link in sidebar
   OR click college name to see college-specific view
2. See table with:
   - Name with avatar
   - Designation (Prof, Asst Prof, etc.)
   - Department
   - Email
   - Action buttons

### To Search for Employee

1. Use search bar at top
2. Type name (e.g., "Rajesh")
3. Hit Enter or click Search button
4. Results filter instantly
5. Click **"Clear"** to reset

### To Add New Employee

1. Click **"+ Add Employee"** button
2. Fill form fields:
   ```
   Name:          [Required] Full name
   Department:    [Required] Department name
   Designation:   [Required] Position title
   Email:         [Required] Unique email address
   Phone:         [Optional] Contact number
   College:       [Optional] Select college
   ```
3. Click **"Save"** button
4. Redirected to employee list (success message shown)

### To Edit Employee

1. Find employee in list
2. Click **"Edit"** button
3. Update desired fields
4. Click **"Save"** button
5. Changes saved immediately

### To View Employee Details

1. Click **"View"** button next to employee
2. See:
   - All employee information
   - List of assigned tasks
   - Task status overview
   - Quick links to edit or add new task

### To Delete Employee

1. Click **"Delete"** button
2. Confirm deletion (popup warning)
3. Employee and all tasks deleted
4. Cannot be undone!

---

## 📋 Task Management

### To Create New Task

1. Click **"Tasks"** in main menu
   OR click **"+Task"** next to any employee
2. Fill form:
   ```
   Title:         [Required] Task name
   Description:   [Optional] Details
   Type:          [Required] Choose from:
                  - EXAM_DUTY
                  - PROJECT_GUIDE
                  - DOCUMENTATION
                  - EVENT_COORDINATION
                  - REPORT_SUBMISSION
                  - OTHER
   Assigned to:   [Required] Select employee
   Due Date:      [Required] Pick date
   Priority:      [Required] HIGH / MEDIUM / LOW
   Status:        [Optional] Default = PENDING
   ```
3. Click **"Save"** button
4. Task created and visible in list

### To View Task List

1. Click **"Tasks"** in main menu
2. See all tasks with:
   - Title/Description
   - Assigned employee
   - Due date
   - Status badge (colored)
   - Action buttons

**Color Coding**:
- 🔵 Blue = PENDING (not started)
- 🟡 Yellow = IN_PROGRESS (being worked)
- 🟢 Green = COMPLETED (finished)
- ⚫ Red = OVERDUE (past due date)

### To Filter Tasks

**By Status**:
1. Use "Status" dropdown to filter
2. Or click quick filters in sidebar:
   - ⏳ Pending Tasks
   - ⚙️ In Progress
   - ⚠️ Overdue Tasks

**By College**:
1. Select college from sidebar
2. Only shows that college's tasks

### To Search Tasks

1. Select college (if filtering by college)
2. Choose status filter
3. Results update automatically

### To Update Task Status

1. Find task in list
2. Click **"View"** to see details
3. Use status dropdown on detail view
4. Select new status:
   - PENDING → IN_PROGRESS → COMPLETED
5. Status saves automatically

**Auto-Overdue Feature**:
- If due date passes and status is PENDING
- System automatically marks as OVERDUE
- No manual action needed

### To Edit Task

1. Find task in list
2. Click **"Edit"** button
3. Update any fields
4. Click **"Save"** button
5. Changes persist immediately

### To View Task Details

1. Click task title or **"View"** button
2. See complete information:
   - All task details
   - Assigned employee info
   - Full description
   - All dates
   - Current status

### To Delete Task

1. Click **"Delete"** on task
2. Confirm deletion
3. Task removed from system

---

## 🔍 Searching & Filtering

### Multi-Filter Search Example

**Scenario**: Find all overdue project guide tasks in Engineering

```
1. Click "Engineering" in sidebar
2. Go to Tasks page
3. Filter by Status: "OVERDUE"
4. Now showing: Overdue tasks for Engineering college
5. Manually look for PROJECT_GUIDE type tasks
6. Or use Ctrl+F to search page for "Project"
```

### Search Tips

| What You Want | How To Do It |
|---------------|------------|
| Find employee by name | Go Employees → Search → Type name |
| See all tasks for person | Find employee → Click "View" |
| Find overdue tasks | Tasks → Filter by "OVERDUE" |
| Find all EXAM_DUTY tasks | Create task → Filter by type (if available) |
| Compare colleges | Dashboard → Click college name |
| Print report | Dashboard → Ctrl+P → Print |

---

## 📈 Dashboard Analysis

### Understanding Statistics

**Overall Dashboard** (No filter):
- Shows institute-wide totals
- Aggregate of all colleges

**College Dashboard** (Filtered):
- Shows only that college
- College-specific statistics
- Compare with other colleges via breakdown

### Interpreting Metrics

```
Total Faculty: 8
↓
Total Tasks: 6
↓
Task Breakdown:
- Pending: 2 tasks (not started)
- In Progress: 2 tasks (currently being done)
- Completed: 1 task (finished)
- Overdue: 1 task (missed deadline)
```

**Action Items**:
1. If high OVERDUE count → Prioritize those tasks
2. If high IN_PROGRESS with no COMPLETED → May be bottleneck
3. If PENDING count increasing → Workload management needed

---

## 🛠️ Common Tasks & Solutions

### Task: "I need to see all tasks for my department"

**Solution**:
1. Click your college name in sidebar
2. Go to "Tasks" page
3. All tasks for your college now visible
4. Can also filter by employee name

### Task: "Mark this task as completed"

**Solution**:
1. Find task in Tasks list
2. Click "View" button
3. Look for status settings
4. Change from PENDING/IN_PROGRESS to COMPLETED
5. Changes auto-save

### Task: "Find all exam duties for Dr. Rajesh"

**Solution**:
1. Click "Employees"
2. Search: type "Rajesh"
3. Click "View" next to "Dr. Rajesh Kumar"
4. See all his tasks (including exam duties)
5. Scroll through or use browser Ctrl+F

### Task: "Senior faculty assigned too many tasks"

**Solution**:
1. View that employee's task list
2. Identify highest priority tasks
3. Consider reassigning lower priority ones
4. Click task → Edit → Reassign to another employee

### Task: "Need to report monthly progress"

**Solution**:
1. Go to Dashboard
2. Note the statistics for that month
3. Take screenshot or print (Ctrl+P)
4. Use for monthly report

---

## ⚠️ Important Notes

### Data Persistence
- ⚠️ **IMPORTANT**: All data is stored in H2 in-memory database
- Data is **lost** when application restarts
- For permanent storage, export data or configure file-based DB

### Backup Important Data
1. Before stopping application:
2. Take screenshots of statistics
3. Export task list (if export feature exists)
4. Print important reports

### Email Uniqueness
- Each employee must have unique email
- Cannot create employee with duplicate email
- System will show error if trying

### Cascading Delete
- When deleting employee → all their tasks deleted too
- No "recover" option after delete
- Always confirm before deleting

---

## 🎓 Training Scenarios

### Scenario 1: Department Head
**Daily Tasks**:
1. Check dashboard each morning
2. Note pending and overdue tasks
3. Identify bottlenecks
4. Reassign workload if needed

**Weekly**:
1. Review completed tasks
2. Plan new tasks for coming week
3. Check employee workload balance

### Scenario 2: Faculty Member
**Daily Tasks**:
1. Check "My Tasks" from dashboard
2. Update status on in-progress tasks
3. Mark completed tasks as done

**Weekly**:
1. View all assigned tasks
2. Plan deadlines
3. Communicate with department head

### Scenario 3: Administrator
**Daily**:
1. Monitor system health
2. Check for errors
3. Ensure data integrity

**Weekly**:
1. Add new faculty as needed
2. Create department-level tasks
3. Generate reports

---

## 💡 Tips & Tricks

### Keyboard Shortcuts
| Shortcut | Action |
|----------|--------|
| Ctrl+P | Print current page |
| Ctrl+F | Find on page (browser) |
| Enter | Submit form quickly |
| Esc | Cancel/close modal |

### Browser Tips
- Use browser back button to undo navigation
- Bookmark dashboard for quick access
- Set browser zoom to 90% for better view on small screens

### Time-Saving Tips
1. **Bookmark favorite views**:
   - Dashboard for your college
   - Your pending tasks

2. **Use search efficiently**:
   - Search employees by last name
   - Filter tasks by priority before status

3. **Bulk operations**:
   - Update multiple task statuses quickly
   - Switch between colleges without reloading

---

## 🔐 Security Notes

### You Can Access
- Your college's employees and tasks
- General dashboard (if no filters available)
- All public information

### You Cannot Access
- ❌ Other college's private data (if implemented)
- ❌ System configuration
- ❌ Database directly (except admin)

### Best Practices
1. Don't share your browser session
2. Close browser when leaving
3. Report suspicious activity
4. Keep password safe (if login added later)

---

## 📞 Getting Help

### Documentation
- See SYSTEM_DESIGN.md for technical details
- Check this guide for user help
- API documentation for developers

### Common Error Messages

| Error | Meaning | Solution |
|-------|---------|----------|
| "Email already exists" | Email is in use | Use different email |
| "Field is required" | Missing required data | Fill all required fields |
| "Invalid email format" | Email format wrong | Use valid email (user@domain.com) |
| "Employee not found" | Record doesn't exist | Verify ID and try again |
| "Database error" | System issue | Restart application |

### When Stuck
1. Refresh the page (Ctrl+R)
2. Check if required fields are filled
3. Go back to dashboard and restart task
4. Contact system administrator

---

## 🚀 Next Steps

1. **Get familiar with interface**:
   - Explore all pages
   - Try search and filters
   - Navigate to all colleges

2. **Create sample tasks**:
   - Practice creating tasks
   - Assign to different employees
   - Update statuses

3. **Generate report**:
   - Check dashboard statistics
   - Print for reference

4. **Train others**:
   - Show colleagues how to use
   - Help with questions

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | April 2026 | Initial Release |

---

**Happy Task Managing! 🎉**

For more details, see SYSTEM_DESIGN.md
