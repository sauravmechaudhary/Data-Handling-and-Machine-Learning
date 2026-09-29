# Week 1 – Exercise Answers

Student Name: Saurav Chaudhary

---

## Part A: Getting, Storing and Retrieving Data

**Exercise 1 – Choose a representation**
1. 100 measurements → **CSV** (small, rectangular).
2. Continuous application events → **JSONL** (one record per line, appendable).
3. Customer with several addresses/phones → **JSON** (nested lists).
4. Students, courses, enrollments queried repeatedly → **Relational database** (related tables, joins).
5. Table for spreadsheet software → **CSV**.

**Exercise 2 – What does one record represent?**
1. One quiz result of one student on one quiz.
2. 2 students (STU_A, STU_B).
3. 3 records.
4. `student_key`.

**Exercise 3 – Raw vs derived**
- Source: `student_key`, `quiz_score`, `submitted_at`.
- Derived: `average_score`, `days_since_submission`, `number_of_attempts`.
- Documenting this matters because derived values depend on calculation choices, can become outdated, and must be reproducible from the raw data.

**Exercise 4 – Join carefully**
The student's row is repeated 5 times, once per attempt. Student-level attributes are then counted 5 times, which biases counts and averages. Aggregate per student before joining, or analyse at attempt level on purpose.

**Exercise 5 – Files or database?**
A combination: keep the JSONL files as raw data, load and validate them in pandas, store them in SQLite for the repeated group → student → attempts/grades queries, and read the results back into pandas for analysis.

---

## Part B: Types, Meaning and Valid Operations

**Exercise 1 – What is the data?**

| Variable | Type | Meaningful operations | Misleading technical type? |
|---|---|---|---|
| Student number | Identifier | Equality, lookup | Yes – stored as a number, but not a measurement |
| Age | Numerical, ratio | Mean, differences, ratios | No |
| Study programme | Nominal | Counts, mode, grouping | Possibly, if coded as numbers |
| Satisfaction 1–5 | Ordinal | Ordering, median, counts | Yes – integers suggest equal intervals |
| Completed courses | Numerical, discrete, ratio | Sum, mean, ratios | No |
| Registration date | Datetime | Ordering, durations | Yes – a string must be converted |
| Student feedback | Text | Text operations, NLP | Yes – not a normal category |

**Exercise 2 – Does the operation make sense?**
1. Mean age – **Yes** (ratio scale).
2. Mean student ID – **No** (identifier).
3. Most common city – **Yes** (mode of nominal data).
4. Highest satisfaction category – **Yes** (ordinal data has an order).
5. Timestamp difference – **Yes** (gives a duration).
6. "Helsinki" × 3 – **No** (arithmetic on a category).
7. Ratio of weights – **Yes** (ratio scale, true zero).
8. Average postcode – **No** (identifier or label).

**Exercise 3 – Find the problem**
City is nominal. The codes 1–3 are arbitrary labels, so a mean of 2.1 has no meaning, and recoding the cities would change it. Use frequencies or the mode instead.

**Exercise 4 – Missing `weekly_study_hours`**
1. **Drop the rows** – misleading if the missing values are not random (e.g. less-engaged students skip the question → average study time is overestimated).
2. **Fill with 0** – treats "not reported" as "no study" → underestimates study time.
3. **Fill with mean/median** – reduces variance and hides differences between groups (e.g. part-time vs full-time students).
4. **Add a missing-value flag + impute** – a model may learn how the data was collected rather than student behaviour.

