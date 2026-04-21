# ETMS Quick Reference Guide
## Employee Task Management System - Cheat Sheet

Print this page or bookmark for quick access!

---

## 🚀 QUICK START

```
Step 1: Start App
  cd d:\WTL\Springbootadv10
  mvn spring-boot:run

Step 2: Open Browser
  http://localhost:8080

Step 3: Login & Explore
  Use sample data immediately
  No login required (yet)
```

---

## 🌐 MAIN URLS

| Feature | URL |
|---------|-----|
| **Dashboard** | http://localhost:8080 |
| **Employees** | http://localhost:8080/employees |
| **Tasks** | http://localhost:8080/tasks |
| **Database** | http://localhost:8080/h2-console |

---

## 👥 EMPLOYEE MANAGEMENT

### Create New Employee
```
1. Go to Employees page
2. Click "+ Add Employee"
3. Fill form:
   - Name (required)
   - Department (required)
   - Designation (required)
   - Email (required, unique)
   - Phone (optional)
   - College (optional)
4. Click Save
```

### Edit Employee
```
1. Find employee in list
2. Click Edit button
3. Modify fields
4. Click Save
```

### Delete Employee
```
1. Click Delete button
2. Confirm in popup
⚠️ WARNING: Deletes all tasks too!
```

### Search Employee
```
1. Use search box
2. Type name (supports partial match)
3. Click Search
4. Or click Clear to reset
```

---

## 📋 TASK MANAGEMENT

### Create New Task
```
1. Go to Tasks page
2. Click "+ New Task"
3. Fill form:
   - Title (required)
   - Description (optional)
   - Type (required): 
     • EXAM_DUTY
     • PROJECT_GUIDE
     • DOCUMENTATION
     • EVENT_COORDINATION
     • REPORT_SUBMISSION
     • OTHER
   - Assigned to (required)
   - Due Date (required)
   - Priority: HIGH/MEDIUM/LOW
4. Click Save
```

### Update Task Status
```
1. Find task in list
2. Click View
3. Change status:
   PENDING → IN_PROGRESS → COMPLETED
4. Changes save automatically
⚡ OVERDUE: Auto-marked if date passed
```

### Filter Tasks
```
By Status:
  1. Go to Tasks page
  2. Use Status dropdown
  3. Select: PENDING, IN_PROGRESS, COMPLETED, OVERDUE

By College:
  1. Click college in sidebar
  2. See only that college's tasks
```

### Delete Task
```
1. Find task in list
2. Click Delete
3. Confirm in popup
```

---

## 📊 DASHBOARD CARDS

```
├─ 👥 Total Faculty       [All employees count]
├─ 📋 Total Tasks         [All tasks count]
├─ ⏳ Pending Tasks        [Not started]
├─ ✅ Completed Tasks      [Finished]
├─ ⚙️ In Progress          [Being worked on]
└─ ⚠️ Overdue Tasks        [Missed deadline]
```

**College Breakdown**: Shows stats for each college

---

## 🔍 SEARCH & FILTER TIPS

### Multi-Filter Example
```
Find: Overdue tasks for Engineering
1. Click "Engineering" in sidebar
2. Go to Tasks page
3. Filter by Status: "OVERDUE"
✓ Now see: All Engineering overdue tasks
```

### Search Operators
```
✓ Partial match: "Raj" finds "Rajesh"
✓ Case-insensitive: "rajesh" = "RAJESH" = "Rajesh"
✓ Multiple results: Shows all matches
✓ Clear filter: Resets search
```

---

## ⚠️ IMPORTANT NOTES

### Data Persistence
```
⚠️ H2 Database = In-Memory
✓ Data exists while app runs
✗ Data lost on app restart
✓ Good for: Testing, Development
✗ NOT for: Production use
```

### Email Rules
```
✓ Must be unique (no duplicates)
✓ Format: user@domain.com
✓ Cannot edit to existing email
```

### Cascade Delete
```
⚠️ Delete employee = Delete all tasks
✓ Single click deletion (confirm)
✗ Cannot undo!
```

---

## 🛠️ TROUBLESHOOTING

### App Won't Start
```
Error: "Port 8080 already in use"
Fix: 
  1. Close other apps using port
  2. Or change: server.port=8081 (in config)
  3. Try: mvn clean spring-boot:run
```

### No Data Showing
```
Error: "All lists appear empty"
Fix:
  1. Data.sql might not load
  2. Restart application
  3. Check: http://localhost:8080/h2-console
  4. Run: SELECT * FROM employees;
```

### Form Shows Errors
```
Error: "Email already exists" or "Field required"
Fix:
  1. Fill all required fields
  2. Use unique email
  3. Use valid email format
  4. Check for typos
  5. Try again
```

### Page Won't Load
```
Error: "Page not responding"
Fix:
  1. Hard refresh: Ctrl+Shift+R
  2. Clear cache: Ctrl+Shift+Delete
  3. Restart browser
  4. Check: Is app still running?
```

---

## 📁 FILE LOCATIONS

```
Project: d:\WTL\Springbootadv10

Key Files:
├─ pom.xml (Dependencies)
├─ src/main/resources/
│  ├─ application.properties (Config)
│  ├─ data.sql (Sample Data)
│  └─ templates/ (HTML pages)
├─ README.md (Overview)
├─ USER_MANUAL.md (Guide)
├─ SYSTEM_DESIGN.md (Architecture)
├─ DEVELOPER_GUIDE.md (Code)
├─ TESTING_GUIDE.md (Tests)
└─ DOCUMENTATION_INDEX.md (Navigation)
```

---

## ⌨️ KEYBOARD SHORTCUTS

```
Ctrl+P    →  Print page
Ctrl+R    →  Refresh page
Ctrl+Shift+R → Hard refresh
Ctrl+F    →  Find on page
Ctrl+Shift+Delete → Clear cache
Enter     →  Submit form
Esc       →  Cancel/Close modal
```

---

## 📞 HELP RESOURCES

```
For:                    Check:
────────────────────────────────────────
How to use system?      → USER_MANUAL.md
Architecture details?   → SYSTEM_DESIGN.md
Code reference?         → DEVELOPER_GUIDE.md
Test cases?            → TESTING_GUIDE.md
Quick overview?        → README.md
Finding information?   → DOCUMENTATION_INDEX.md
```

---

## 🎯 COMMON TASKS

### Task: "I lost data after restart"
```
✓ Expected behavior (in-memory H2)
✓ Solution: Take backups before restart
✓ Permanent fix: Switch to MySQL/PostgreSQL
```

### Task: "User can't find task"
```
1. Check if assigned correctly
2. Verify due date not changed
3. Filter by status
4. Search by title
5. Check college assignment
```

### Task: "Employee changed department"
```
1. Go to Employees page
2. Find employee
3. Click Edit
4. Change Department
5. Click Save
✓ Done! Changes reflected everywhere
```

### Task: "Print department report"
```
1. Go to Dashboard
2. Click college name (if needed)
3. Note all statistics
4. Press Ctrl+P
5. Print to PDF or paper
✓ Report ready!
```

---

## 📊 SAMPLE DATA REFERENCE

### Built-in Colleges
```
1. Engineering (Code: ENGG)
   - 3 faculty members
   - 3+ tasks

2. Pharmacy (Code: PHARMA)
   - 3 faculty members
   - 2+ tasks

3. BHMS (Code: BHMS)
   - 2 faculty members
   - 2+ tasks
```

### Task Types
```
1. EXAM_DUTY          - Exam duties
2. PROJECT_GUIDE      - Project supervision
3. DOCUMENTATION      - Document preparation
4. EVENT_COORDINATION - Event management
5. REPORT_SUBMISSION  - Report submissions
6. OTHER             - Miscellaneous
```

### Task Status
```
PENDING      - Not started (blue)
IN_PROGRESS  - Being worked (yellow)
COMPLETED    - Finished (green)
OVERDUE      - Missed deadline (red)
```

### Priority Levels
```
HIGH    - Urgent/Important
MEDIUM  - Important
LOW     - Regular/Routine
```

---

## ✅ DAILY CHECKLIST

### As an Admin
```
□ Check dashboard
□ Review pending tasks
□ Assign new tasks
□ Follow up on overdue
□ Verify data accuracy
```

### As a Faculty
```
□ View my tasks
□ Update task status
□ Check deadlines
□ Mark completed tasks
□ Plan for upcoming tasks
```

### As Department Head
```
□ Review college stats
□ Check workload distribution
□ Identify bottlenecks
□ Plan assignments
□ Generate report
```

---

## 🔐 SECURITY NOTES

### Current State
```
✓ Input validation
✓ Email uniqueness
✓ SQL injection prevention
✗ No user authentication
✗ No permissions/roles
```

### Best Practices
```
✓ Use unique strong emails
✓ Verify employee data
✓ Regular data backups
✓ Monitor system usage
✓ Report issues immediately
```

---

## 📈 PERFORMANCE NOTES

### Expected Times
```
Dashboard load   < 1 second
Search          < 100ms
Filter          < 200ms
Create          < 50ms
Update          < 30ms
Delete          < 30ms
```

### Tips for Speed
```
✓ Use specific searches
✓ Filter before searching
✓ Close unused tabs
✓ Clear browser cache weekly
✓ Restart app monthly
```

---

## 🎓 LEARNING RESOURCES

### Beginner (1 hour)
```
1. Read: README.md
2. Explore: Dashboard
3. Try: Add employee
4. Try: Create task
```

### Intermediate (3 hours)
```
1. Read: USER_MANUAL.md
2. Read: SYSTEM_DESIGN.md (overview sec.)
3. Practice: All features
4. Explore: Database
```

### Advanced (8+ hours)
```
1. Read: DEVELOPER_GUIDE.md
2. Study: Source code
3. Read: TESTING_GUIDE.md
4. Implement: Custom features
```

---

## 📋 USEFUL COMMANDS

### Start Application
```bash
mvn spring-boot:run
```

### Build Application
```bash
mvn clean package
```

### Run Tests
```bash
mvn test
```

### Clean Project
```bash
mvn clean
```

### Run JAR
```bash
java -jar target/etms-1.0.0.jar
```

### Check Java Version
```bash
java -version
```

### Check Maven Version
```bash
mvn -version
```

---

## 🌟 BEST PRACTICES

### Organization
```
✓ Name employees clearly
✓ Use consistent department names
✓ Set realistic due dates
✓ Assign clear task titles
✓ Update status regularly
```

### Data Quality
```
✓ Use valid email addresses
✓ Complete all required fields
✓ Avoid duplicate entries
✓ Review data regularly
✓ Archive old completed tasks
```

### System Management
```
✓ Regular database backups
✓ Monitor performance metrics
✓ Keep dependencies updated
✓ Review user feedback
✓ Plan capacity
```

---

## 🎉 GETTING HELP

### Documentation
```
All answers in 6 files:
1. README.md - Start here
2. USER_MANUAL.md - How-to guide
3. SYSTEM_DESIGN.md - Architecture
4. DEVELOPER_GUIDE.md - Code ref
5. TESTING_GUIDE.md - QA guide
6. DOCUMENTATION_INDEX.md - Navigation
```

### Live Help
```
Email: support@college.edu
Available: Business hours
Response: Within 24 hours
```

### Self Help
```
1. Check DOCUMENTATION_INDEX.md
2. Search relevant document
3. Review troubleshooting section
4. Check FAQ (if available)
5. Contact support
```

---

## 📌 QUICK LINKS

| Resource | Location |
|----------|----------|
| **Application** | http://localhost:8080 |
| **Database** | http://localhost:8080/h2-console |
| **Project** | d:\WTL\Springbootadv10 |
| **Docs** | All .md files in project root |
| **Source** | src/main/java/com/scoe/etms |
| **Config** | src/main/resources |

---

## ⭐ TOP 5 FEATURES

1. **Multi-College Support** - Manage multiple departments
2. **Task Tracking** - End-to-end task lifecycle management
3. **Smart Filtering** - Find exactly what you need
4. **Real-time Dashboard** - See key metrics instantly
5. **Easy to Use** - Intuitive interface, minimal training

---

## 🚀 NEXT STEPS

1. **Bookmark this page** ← Bookmark now!
2. **Read: README.md** (10 min)
3. **Run: mvn spring-boot:run** (5 min)
4. **Access: localhost:8080** (instant)
5. **Explore & Enjoy!** ✨

---

**ETMS Quick Reference v1.0 | April 2026**

Happy Task Managing! 🎉

---

**More Help?** See DOCUMENTATION_INDEX.md
