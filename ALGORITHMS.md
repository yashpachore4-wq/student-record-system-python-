# Algorithm Design – Student Record and Academic Management System

## Data structures used
| Structure | Used for |
|---|---|
| String | name, department, address, email |
| Tuple | identity data that must not change: `(roll_no, reg_no, dob)` |
| List | subjects, marks, attendance records, list of students (for reports) |
| Set | unique departments, unique subjects, unique clubs |
| Dictionary | `students = {roll_no: profile}`; profile = dict of all fields |

Profile layout:
```
students[roll_no] = {
  "ids": (roll_no, reg_no, dob),      # tuple
  "name": str, "department": str,
  "subjects": [str,...], "marks": [num,...],   # lists (parallel)
  "attendance": num,
  "clubs": {str,...},                 # set
  "contact": {"email": str, "phone": str, "address": str}
}
```
Global: `students` (dict), `departments` (set).

---
## Algorithm 1: Add New Student
1. START.
2. Read roll no, reg no, DOB, name, department, subjects, marks, attendance, clubs, contact.
3. If any field is empty, marks/attendance outside 0–100, marks count ≠ subjects count, or email invalid → show error, go to step 9.
4. If roll no already exists in `students` → show "Duplicate record", go to step 9.
5. Build profile dictionary (ids as tuple, subjects/marks as lists, clubs as set).
6. `students[roll_no] = profile`.
7. Add department to `departments` set.
8. Display "Student added".
9. END.

## Algorithm 2: Search Student by Roll Number
1. START.
2. If `students` is empty → display "No records", go to 7.
3. Read roll no.
4. If roll no in `students` → step 5, else display "Not found", go to 7.
5. Retrieve profile.
6. Display profile.
7. END.

## Algorithm 3: Update Student Information
1. START. Read roll no.
2. If not in `students` → display "Not found", END.
3. Show menu (name, department, contact, marks, attendance, clubs).
4. Read choice and new value.
5. Validate new value; if invalid → display error, END.
6. Update that field in the profile (if department changed, rebuild `departments`).
7. Display "Record updated". END.

## Algorithm 4: Delete Record
1. START. Read roll no.
2. If not in `students` → "Not found", END.
3. Ask for confirmation. If "no" → "Cancelled", END.
4. `del students[roll_no]`.
5. Rebuild `departments` set from remaining students.
6. Display "Record deleted". END.

## Algorithm 5: Display All Records
1. START.
2. If `students` is empty → "No records", END.
3. For each student in `students`: print roll no, name, department, subjects, marks, attendance.
4. Print total count. END.

## Algorithm 6: Calculate Average Marks
1. START. If `students` empty → "No records", END.
2. `total = 0`, `count = 0`.
3. For each student: `avg = sum(marks) / len(marks)` (skip if marks empty); `total += avg`; `count += 1`; display `avg`.
4. `class_avg = total / count`.
5. Display class average. END.

## Algorithm 7: Find Highest Scorer
1. START. If `students` empty → "No records", END.
2. `best = None`, `best_avg = -1`.
3. For each student compute `avg = sum(marks)/len(marks)`.
4. If `avg > best_avg` → `best = student`, `best_avg = avg`.
5. Display `best` name, roll no and `best_avg` (report all students on a tie). END.

## Algorithm 8: List Students by Department
1. START. Read department name.
2. If department not in `departments` → "No such department", END.
3. For each student: if `student.department == department` → add to result list.
4. If result list empty → "No students"; else display each. END.

## Algorithm 9: Count Students
1. START.
2. `total = len(students)`.
3. Create empty dict `dept_count`.
4. For each student: `dept_count[dept] = dept_count.get(dept, 0) + 1`.
5. Display `total` and `dept_count`. END.

## Algorithm 10: Generate Reports
1. START. Show report menu (1 highest scorer, 2 by department, 3 count, 4 class average).
2. Read choice.
3. Call Algorithm 7, 8, 9 or 6 accordingly; invalid choice → error.
4. Display report. END.
