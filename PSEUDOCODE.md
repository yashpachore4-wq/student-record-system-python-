# Pseudocode – Student Record and Academic Management System

```
GLOBAL students ← empty dictionary
GLOBAL departments ← empty set

FUNCTION validate(marks, attendance, email, subjects)
    IF LENGTH(marks) ≠ LENGTH(subjects) THEN RETURN False
    FOR EACH m IN marks
        IF m < 0 OR m > 100 THEN RETURN False
    IF attendance < 0 OR attendance > 100 THEN RETURN False
    IF email DOES NOT CONTAIN "@" AND "." THEN RETURN False
    RETURN True
END FUNCTION

FUNCTION add_student()
    READ roll_no, reg_no, dob, name, dept, subjects, marks, attendance, clubs, email, phone, address
    IF any input is empty OR NOT validate(marks, attendance, email, subjects) THEN
        PRINT "Invalid input"
        RETURN
    IF roll_no IN students THEN
        PRINT "Duplicate record"
        RETURN
    profile ← {ids: (roll_no, reg_no, dob), name: name, department: dept,
               subjects: subjects, marks: marks, attendance: attendance,
               clubs: SET(clubs), contact: {email, phone, address}}
    students[roll_no] ← profile
    ADD dept TO departments
    PRINT "Student added"
END FUNCTION

FUNCTION search_student()
    IF students IS EMPTY THEN PRINT "No records"; RETURN
    READ roll_no
    IF roll_no IN students THEN PRINT students[roll_no]
    ELSE PRINT "Student not found"
END FUNCTION

FUNCTION update_student()
    READ roll_no
    IF roll_no NOT IN students THEN PRINT "Student not found"; RETURN
    DISPLAY menu (name, department, contact, marks, attendance, clubs)
    READ choice, new_value
    IF new_value is invalid THEN PRINT "Invalid input"; RETURN
    students[roll_no][choice] ← new_value
    IF choice = department THEN departments ← SET of all student departments
    PRINT "Record updated"
END FUNCTION

FUNCTION delete_student()
    READ roll_no
    IF roll_no NOT IN students THEN PRINT "Student not found"; RETURN
    READ confirm
    IF confirm ≠ "y" THEN PRINT "Cancelled"; RETURN
    DELETE students[roll_no]
    departments ← SET of all remaining student departments
    PRINT "Record deleted"
END FUNCTION

FUNCTION display_all()
    IF students IS EMPTY THEN PRINT "No records"; RETURN
    FOR EACH roll_no, p IN students
        PRINT roll_no, p.name, p.department, p.subjects, p.marks, p.attendance
    PRINT "Total students:", LENGTH(students)
END FUNCTION

FUNCTION average(p)
    IF p.marks IS EMPTY THEN RETURN 0
    RETURN SUM(p.marks) / LENGTH(p.marks)
END FUNCTION

FUNCTION class_average()
    IF students IS EMPTY THEN PRINT "No records"; RETURN
    total ← 0
    FOR EACH p IN students
        total ← total + average(p)
        PRINT p.name, average(p)
    PRINT "Class average:", total / LENGTH(students)
END FUNCTION

FUNCTION highest_scorer()
    IF students IS EMPTY THEN PRINT "No records"; RETURN
    best ← NONE; best_avg ← -1
    FOR EACH p IN students
        IF average(p) > best_avg THEN best ← p; best_avg ← average(p)
    PRINT best.name, best.ids[0], best_avg
END FUNCTION

FUNCTION list_by_department()
    READ dept
    IF dept NOT IN departments THEN PRINT "No such department"; RETURN
    FOR EACH p IN students
        IF p.department = dept THEN PRINT p.ids[0], p.name
END FUNCTION

FUNCTION count_students()
    dept_count ← empty dictionary
    FOR EACH p IN students
        dept_count[p.department] ← dept_count.get(p.department, 0) + 1
    PRINT "Total:", LENGTH(students), dept_count
END FUNCTION

FUNCTION generate_reports()
    DISPLAY "1 Highest scorer  2 By department  3 Count  4 Class average"
    READ choice
    CASE choice OF
        1: highest_scorer()
        2: list_by_department()
        3: count_students()
        4: class_average()
        OTHERWISE: PRINT "Invalid choice"
END FUNCTION

PROCEDURE main()
    LOOP
        DISPLAY menu: 1 Add, 2 Search, 3 Update, 4 Delete, 5 Display, 6 Average, 7 Reports, 0 Exit
        READ choice
        IF choice = 0 THEN BREAK
        CALL the function matching choice (invalid → PRINT "Invalid choice")
    END LOOP
END PROCEDURE
```
