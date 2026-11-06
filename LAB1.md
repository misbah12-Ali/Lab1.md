
# Practical Lab 1: Hands-On Student Exercises Solution

---

## Exercise 1: Degree Honors Rule

### Task Objective
Add a new rule to `UNIVERSITY_KB["rules"]` for students completing their degree program:

- **Condition:** cgpa >= 3.70 and disciplinary_warnings == 0
- **Status:** "Graduated with Magna Cum Laude Honors"

### Python Code Solution

```python
# Exercise 1: Adding Degree Honors Rule to Knowledge Base
honors_rule = {
    "id": "RULE_00",
    "conditions": {"cgpa_min": 3.70, "disciplinary_warnings": 0},
    "condition_type": "degree_honors",
    "grade": "A+",
    "status": "Graduated with Magna Cum Laude Honors",
    "action": "Awarded Degree with Highest Distinction and Magna Cum Laude."
}

# Insert rule into Knowledge Base
UNIVERSITY_KB["rules"].insert(0, honors_rule)
```

---

## Exercise 2: Tracing Forward Chaining

### Task Objective
Modify `evaluate_student()` function to print each evaluated rule condition step-by-step as the loop runs, demonstrating logical deduction.

### Python Code Solution

```python
# Exercise 2: Inference Engine with Step-by-Step Forward Chaining Trace
def evaluate_student(student_record, kb):
    print(f"\n--- EVALUATING STUDENT: {student_record['student_name']} (ID: {student_record['student_id']}) ---")
    
    # Step 1: Check Degree Honors Rule
    if "cgpa" in student_record and "disciplinary_warnings" in student_record:
        honors_rule = next((r for r in kb["rules"] if r["condition_type"] == "degree_honors"), None)
        if honors_rule:
            cgpa_match = student_record["cgpa"] >= honors_rule["conditions"]["cgpa_min"]
            warning_match = student_record["disciplinary_warnings"] == honors_rule["conditions"]["disciplinary_warnings"]
            
            print(f"[TRACE] Checking {honors_rule['id']}: CGPA >= 3.70 ({cgpa_match}) AND Warnings == 0 ({warning_match})")
            
            if cgpa_match and warning_match:
                print(f"[RULE MATCHED] {honors_rule['id']} Fired!")
                return honors_rule

    # Step 2: Check Attendance Constraint Rule
    print(f"[TRACE] Checking RULE_01: Attendance ({student_record['attendance']}%) < 75%")
    if student_record["attendance"] < 75:
        attendance_rule = next(r for r in kb["rules"] if r["condition_type"] == "attendance_below")
        print(f"[RULE MATCHED] {attendance_rule['id']} Fired! Student Debarred.")
        return attendance_rule

    # Step 3: Check Marks Range Rules
    marks = student_record["total_marks"]
    for rule in kb["rules"]:
        if rule["condition_type"] == "marks_range":
            min_m, max_m = rule["conditions"]["marks_min"], rule["conditions"]["marks_max"]
            print(f"[TRACE] Checking {rule['id']}: Is Marks ({marks}) between {min_m} and {max_m}?")
            if min_m <= marks <= max_m:
                print(f"[RULE MATCHED] {rule['id']} Fired!")
                return rule
```

---

## Test Execution Code

### Python Code Solution

```python
# Test Cases Execution
student_honors = {
    "student_id": "BSCS-2024-001",
    "student_name": "Zainab Fatima",
    "total_marks": 95,
    "attendance": 98,
    "cgpa": 3.85,
    "disciplinary_warnings": 0
}

student_debarred = {
    "student_id": "BSCS-2024-045",
    "student_name": "Sara Ahmed",
    "total_marks": 91,
    "attendance": 68,
    "cgpa": 3.90,
    "disciplinary_warnings": 0
}

result_1 = evaluate_student(student_honors, UNIVERSITY_KB)
print(f"Result: {result_1['status']} | Grade: {result_1['grade']}\n")

result_2 = evaluate_student(student_debarred, UNIVERSITY_KB)
print(f"Result: {result_2['status']} | Grade: {result_2['grade']}")
```

---

## Execution Output

### Console Output

```
--- EVALUATING STUDENT: Zainab Fatima (ID: BSCS-2024-001) ---
[TRACE] Checking RULE_00: CGPA >= 3.70 (True) AND Warnings == 0 (True)
[RULE MATCHED] RULE_00 Fired!
Result: Graduated with Magna Cum Laude Honors | Grade: A+

--- EVALUATING STUDENT: Sara Ahmed (ID: BSCS-2024-045) ---
[TRACE] Checking RULE_00: CGPA >= 3.70 (True) AND Warnings == 0 (True)
[TRACE] Checking RULE_01: Attendance (68%) < 75%
[RULE MATCHED] RULE_01 Fired! Student Debarred.
Result: Debarred | Grade: F
```

