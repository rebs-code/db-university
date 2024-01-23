# Solve queries with SQL

## DAY ONE

1. Selezionare tutti gli studenti nati nel 1990 (160)

SELECT * FROM `students` WHERE YEAR(`date_of_birth`) = 1990;

2. Selezionare tutti i corsi che valgono più di 10 crediti (479)

SELECT * FROM `courses` WHERE `cfu` > 10;

3. Selezionare tutti gli studenti che hanno più di 30 anni

SELECT * FROM `students` WHERE YEAR(CURRENT_DATE()) - YEAR(`date_of_birth`) > 30;

4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di
   laurea (286)

SELECT * FROM `courses` WHERE (`year` = 1 AND `period` = 'I semestre');

5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del
   20/06/2020 (21)

SELECT * FROM `exams` WHERE TIME(`hour`) > '14:00:00' AND `date` = '2020-06-20';

6. Selezionare tutti i corsi di laurea magistrale (38)

SELECT * FROM `degrees` WHERE `name` LIKE '%magistrale%';

7. Da quanti dipartimenti è composta l'università? (12)

SELECT COUNT(`id`) AS 'numero_dipartimenti' FROM `departments`;

8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)

SELECT * FROM `teachers` WHERE `phone` IS NULL;

## DAY TWO

### Queries with JOIN

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

SELECT `students`.`name`, `students`.`surname`, `degrees`.`name` AS 'degree_name' FROM `students` INNER JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id` WHERE `degrees`.`name`= 'Corso di Laurea in Economia'; (68 total)

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di
Neuroscienze

SELECT `degrees`.`name`, `degrees`.`level`, `departments`.`name` AS 'department' FROM `degrees` INNER JOIN `departments` ON `degrees`.`department_id` = `departments`.`id` WHERE `degrees`.`level` = 'magistrale' AND `departments`.`name` = 'Dipartimento di Neuroscienze'; (1 total)

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

SELECT `courses`.`name` AS 'course_name', `teachers`.`name` AS 'teacher_name', `teachers`.`surname` AS 'teacher_surname' FROM `courses` INNER JOIN `course_teacher` ON `courses`.`id` = `course_teacher`.`course_id` INNER JOIN `teachers` ON `course_teacher`.`teacher_id` = `teachers`.`id` WHERE `teachers`.`id` = 44; (total 11)

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome

SELECT `students`.`surname`, `students`.`name`, `degrees`.`name` AS 'degree_name', `departments`.`name` AS 'department_name' FROM `students` INNER JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id` INNER JOIN `departments` ON `degrees`.`department_id` = `departments`.`id` ORDER BY `students`.`surname`; (5000 total)

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

SELECT `degrees`.`name`, `courses`.`name` AS 'course_name', `teachers`.`name` AS 'teacher_name', `teachers`.`surname` AS 'teacher_surname' FROM `degrees` INNER JOIN `courses` ON `degrees`.`id` = `courses`.`degree_id` INNER JOIN `teachers` ON `courses`.`id` = `teachers`.`id`;

6. Selezionare tutti i docenti che insegnano nel Dipartimento di
Matematica (54)

SELECT DISTINCT `teachers`.`name` AS 'teacher_name', `teachers`.`surname` AS 'teacher_surname', `departments`.`name` AS 'department_name' FROM `teachers` INNER JOIN `course_teacher` ON `teachers`.`id` = `course_teacher`.`teacher_id` INNER JOIN `courses` ON `course_teacher`.`course_id` = `courses`.`id` INNER JOIN `degrees` ON `courses`.`degree_id` = `degrees`.`id` INNER JOIN `departments` ON `degrees`.`department_id` = `departments`.`id` WHERE `departments`.`id` = '5';


7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti
per ogni esame, stampando anche il voto massimo. Successivamente,
filtrare i tentativi con voto minimo 18

### Queries with GROUP BY

1. Contare quanti iscritti ci sono stati ogni anno

SELECT COUNT(`id`), YEAR(`enrolment_date`) AS 'year' FROM `students` GROUP BY YEAR(`enrolment_date`);

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio

SELECT COUNT(`id`) AS 'teachers_office_in_building', `teachers`.`office_address` AS 'office_address' FROM `teachers` GROUP BY `teachers`.`office_address`;

3. Calcolare la media dei voti di ogni appello d'esame

SELECT `exams`.`id` AS 'exam_id', AVG(`exam_student`.`vote`) AS average_vote FROM `exams` INNER JOIN `exam_student` ON `exams`.`id` = `exam_student`.`exam_id` GROUP BY `exams`.`id`;

4. Contare quanti corsi di laurea ci sono per ogni dipartimento

SELECT COUNT(`degrees`.`id`), `departments`.`name` FROM `degrees` INNER JOIN `departments` ON `degrees`.`department_id` = `departments`.`id` GROUP BY `departments`.`name`;