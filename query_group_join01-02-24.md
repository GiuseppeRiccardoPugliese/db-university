#### Group by

1. Contare quanti iscritti ci sono stati ogni anno
SELECT YEAR(enrolment_date), COUNT(*) 'iscritti_annuali' FROM students GROUP BY YEAR(enrolment_date);

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
SELECT teachers.office_address, COUNT(*) 'same_office' FROM teachers WHERE office_address LIKE office_address GROUP BY teachers.office_address;

3. Calcolare la media dei voti di ogni appello d'esame (dell'esame vogliamo solo l'`id`)
SELECT exam_id, FLOOR(AVG(vote)) FROM exam_student GROUP BY exam_id;

4. Contare quanti corsi di laurea ci sono per ogni dipartimento
SELECT departments.name, COUNT(*) 'degrees_count' FROM departments JOIN degrees ON departments.id = degrees.department_id GROUP BY departments.name;


#### Join

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
SELECT students.name , students.surname, degrees.name FROM students JOIN degrees ON students.degree_id = degrees.id WHERE degrees.name LIKE 'Corso di Laurea in Economia';

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze
SELECT departments.name, degrees.level FROM degrees JOIN departments ON degrees.department_id = departments.id WHERE departments.name LIKE 'Dipartimento di Neuroscienze' AND degrees.level LIKE 'magistrale';

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
SELECT * FROM courses JOIN course_teacher ON courses.id = course_teacher.course_id JOIN teachers ON course_teacher.teacher_id = teachers.id WHERE teachers.id LIKE '44';

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
SELECT students.name, students.surname, degrees.name, departments.name  FROM students JOIN degrees ON students.degree_id = degrees.id JOIN departments ON degrees.department_id = departments.id ORDER BY students.surname , students.name;

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
SELECT degrees.name, courses.name,teachers.name, teachers.surname  FROM degrees
JOIN courses 
ON degrees.id = courses.degree_id
JOIN course_teacher
ON courses.id = course_teacher.course_id
JOIN teachers
ON course_teacher.teacher_id = teachers.id

6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)
SELECT DISTINCT teachers.* FROM teachers
JOIN course_teacher
ON teachers.id = course_teacher.teacher_id
JOIN courses
ON course_teacher.course_id = courses.id
JOIN degrees
ON courses.degree_id = degrees.id
JOIN departments
ON degrees.department_id = departments.id
WHERE departments.name LIKE 'Dipartimento di Matematica'
