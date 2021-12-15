Query con Group by
1. Contare quanti iscritti ci sono stati ogni anno
2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
3. Calcolare la media dei voti di ogni appello d'esame
4. Contare quanti corsi di laurea ci sono per ogni dipartimento


Query con Join
1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
2. Selezionare tutti i Corsi di Laurea del Dipartimento di Neuroscienze
3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)
7. BONUS: Selezionare per ogni studente quanti tentativi d’esame ha sostenuto per superare ciascuno dei suoi esami


Answers:

QUERY CON GROUP BY

1)
SELECT COUNT(id) AS `students_number`, YEAR(`enrolment_date`) AS `enrolment_year` 
FROM `students` 
GROUP BY `enrolment_year`;

2)
SELECT COUNT(id) AS `teachers_number`, `office_address` AS `office_address` 
FROM `teachers` 
GROUP BY `office_address`;

3)
SELECT `exam_id` AS `exam_id`, 
AVG(`vote`) AS `average_vote` 
FROM `exam_student` 
GROUP BY `exam_id`;

4)
SELECT `department_id`, 
COUNT(`id`) AS `courses_number` 
FROM `degrees` 
GROUP BY `department_id`;



QUERY CON JOIN

1)
SELECT `students`.`id`, `students`.`name`, `students`.`surname`
FROM `students`
JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id`
WHERE `degrees`.`name` = 'Corso di Laurea in Economia';

2) 
SELECT `degrees`.`id` AS `degree_id`, `degrees`.`name` AS `degree_name` 
FROM `degrees`
JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
WHERE `departments`.`name` = 'Dipartimento di Neuroscienze';

3)
SELECT `courses`.`id` AS `course_id`, `courses`.`name` AS `course_name`
FROM `courses`
JOIN `course_teacher` ON `courses`.`id` = `course_teacher`.`course_id`
WHERE `course_teacher`.`teacher_id` = '44';

4)
SELECT 
`students`.`name` AS `student_name`, 
`students`.`surname` AS `student_surname`,
`degrees`.`name` AS `degree`,
`departments`.`name` AS `department`
FROM `students`
LEFT JOIN `degrees` ON `degrees`.`id` = `degree_id`
LEFT JOIN `departments` ON `departments`.`id` = `department_id`
ORDER BY `student_surname` ASC, `student_name` ASC;

5)
SELECT `degrees`.`name` AS `degree`,
`courses`.`name` AS `course` , 
CONCAT(`teachers`.`name`, ' ', `teachers`.`surname`) AS `teacher`
FROM `degrees`
JOIN `courses` ON `degrees`.`id` = `degree_id`
JOIN `course_teacher` ON `courses`.`id` = `course_id`
JOIN `teachers` ON `teacher_id` = `teachers`.`id`
ORDER BY `degrees`.`id` ASC;

6)
SELECT DISTINCT `teachers`.`*`,
`departments`.`name` AS `department_name`
FROM `teachers`
JOIN `course_teacher` ON `teachers`.`id` = `teacher_id`
JOIN `courses` ON `course_id` = `courses`.`id`
JOIN `degrees` ON `degree_id` = `degrees`.`id`
JOIN `departments` ON `department_id` = `departments`.`id`
WHERE `departments`.`name` = 'Dipartimento di Matematica';

7) (BONUS)
SELECT `students`.`id` AS `student_id`, 
CONCAT(`students`.`name`, ' ', `students`.`surname`) AS `student_fullname`,
`courses`.`name` AS `course_name`,
COUNT(`exams`.`id`) AS `attempts_number`
FROM `students`
JOIN `exam_student` ON `students`.`id` = `student_id`
JOIN `exams` ON `exam_id` = `exams`.`id`
JOIN `courses` ON `course_id` = `courses`.`id`
GROUP BY `courses`.`id`, `students`.`id`
ORDER BY `students`.`surname` ASC, `students`.`name` ASC, `students`.`id` ASC;
