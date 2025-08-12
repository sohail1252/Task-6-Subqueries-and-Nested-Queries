-- 1. Create tables
CREATE TABLE Students (
    student_id INT PRIMARY KEY,
    student_name VARCHAR(50),
    class_id INT
);

CREATE TABLE Exams (
    exam_id INT PRIMARY KEY,
    student_id INT,
    score INT
);

-- Insert sample data
INSERT INTO Students (student_id, student_name, class_id) VALUES
(1, 'Alice', 101),
(2, 'Bob', 101),
(3, 'Charlie', 102);

INSERT INTO Exams (exam_id, student_id, score) VALUES
(1, 1, 85),
(2, 1, 90),
(3, 2, 75),
(4, 3, 95);

-- Query using subqueries
SELECT 
    student_name,
    -- 2. Scalar subquery: Get average score for each student
    (SELECT AVG(score) FROM Exams e WHERE e.student_id = s.student_id) AS avg_score,
    -- 3. Correlated subquery with EXISTS: Check if student has any exam scores
    CASE 
        WHEN EXISTS (SELECT 1 FROM Exams e WHERE e.student_id = s.student_id) 
        THEN 'Yes' 
        ELSE 'No' 
    END AS has_exams,
    -- 3. Subquery with IN: Check if student is in class 101
    CASE 
        WHEN class_id IN (SELECT class_id FROM Students WHERE class_id = 101) 
        THEN 'Class 101' 
        ELSE 'Other Class' 
    END AS class_check
FROM Students s
WHERE 
    -- 3. Subquery with =: Only include students with at least one score >= 80
    student_id = ANY (SELECT student_id FROM Exams WHERE score >= 80);
