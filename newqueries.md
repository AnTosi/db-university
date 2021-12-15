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
    GROUP BY `exam_id`

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

