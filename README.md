/*1. Selezionare tutti gli studenti nati nel 1990 (160)
2. Selezionare tutti i corsi che valgono più di 10 crediti (479)
3. Selezionare tutti gli studenti che hanno più di 30 anni
4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di
laurea (286)
5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del
20/06/2020 (21)
6. Selezionare tutti i corsi di laurea magistrale (38)
7. Da quanti dipartimenti è composta l'università? (12)
8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)*/

/*ES 1*/
SELECT *
FROM `students`
WHERE YEAR(`date_of_birth`) = 1990 ;

/*ES 2 */
SELECT *
FROM `courses`
WHERE `cfu` > 10;

/*ES 3 */
SELECT * 
FROM `students`
WHERE YEAR(`date_of_birth`) >= 1995;

/*ES 4 */
SELECT *
FROM courses
WHERE period = 'I semestre';

/*ES 5 */
SELECT *
FROM exams
WHERE HOUR(hour) >= 14
AND date = '2020-06-20';

/*ES 6*/
SELECT *
FROM degrees
WHERE level = 'magistrale';

/*ES 7 */
SELECT COUNT(id) AS numero_dp
FROM departments;

/*ES 8 */
SELECT COUNT(id) AS numero_PROF
FROM teachers
WHERE phone IS null;

/*1. Selezionare tutti gli studenti nati nel 1990 (160) 
2. Selezionare tutti i corsi che valgono più di 10 crediti (479)
 3. Selezionare tutti gli studenti che hanno più di 30 anni 
 4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286) 
 5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21) 
 6. Selezionare tutti i corsi di laurea magistrale (38) 
 7. Da quanti dipartimenti è composta l'università? (12)
 8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)*/

/*ES 1*/ SELECT * FROM students WHERE YEAR(date_of_birth) = 1990 ;

/*ES 2 */ SELECT * FROM courses WHERE cfu > 10;

/*ES 3 */ SELECT * FROM students WHERE YEAR(date_of_birth) >= 1995;

/*ES 4 */ SELECT * FROM courses WHERE period = 'I semestre';

/*ES 5 */ SELECT * FROM exams WHERE HOUR(hour) >= 14 AND date = '2020-06-20';

/*ES 6*/ SELECT * FROM degrees WHERE level = 'magistrale';

/*ES 7 */ SELECT COUNT(id) AS numero_dp FROM departments;

/*ES 8 */ SELECT COUNT(id) AS numero_PROF FROM teachers WHERE phone IS null;

/*1. Contare quanti iscritti ci sono stati ogni anno
2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
3. Calcolare la media dei voti di ogni appello d'esame
4. Contare quanti corsi di laurea ci sono per ogni dipartimento*/

/*ES 1*/
SELECT COUNT(*) AS `numero_iscritti`, YEAR(`enrolment_date`) AS `anno`
FROM students  
GROUP BY anno;  

/*ES 2*/
SELECT COUNT(*) AS `numero_insegnanti`, `office_address` AS `ufficio` 
FROM teachers
GROUP BY ufficio;

/*ES 3*/
SELECT AVG(`vote`) AS `media`,  `exam_id` AS `esame`
from exam_student
GROUP BY esame;

/*ES 4*/
SELECT COUNT(*) AS `corsi`, `department_id` AS `dipartimento`
FROM degrees
GROUP BY dipartimento;

/*1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di
Neuroscienze
3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui
sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e
nome
5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
6. Selezionare tutti i docenti che insegnano nel Dipartimento di
Matematica (54)
7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti
per ogni esame, stampando anche il voto massimo. Successivamente,
filtrare i tentativi con voto minimo 18*/

/*ES 1*/
SELECT *
FROM students
JOIN degrees ON students.degree_id = degrees.id
WHERE degrees.name = 'Corso di Laurea in Economia';


/*ES 2 */
SELECT *
FROM degrees
JOIN departments ON departments.id = degrees.department_id
WHERE departments.name LIKE '%Neuroscienze'
AND degrees.level = 'magistrale';

/*ES 3*/

SELECT *
FROM courses
JOIN course_teacher ON courses.id = course_teacher.course_id
JOIN teachers ON teachers.id = course_teacher.teacher_id
WHERE teachers.name = 'Fulvio'
AND teachers.surname = 'Amato';

/*ES 4*/

SELECT *
FROM students
JOIN degrees ON degrees.id = students.degree_id
JOIN departments ON departments.id = degrees.department_id
ORDER BY students.surname, students.name ASC ;

/*ES 5*/

SELECT *
FROM degrees
JOIN courses ON courses.degree_id = degrees.id
JOIN course_teacher ON course_teacher.course_id = courses.id
JOIN teachers ON course_teacher.teacher_id = teachers.id;

/*ES 6*/

SELECT DISTINCT teachers.name
FROM teachers
JOIN course_teacher ON course_teacher.teacher_id = teachers.id
JOIN courses ON courses.id = course_teacher.course_id
JOIN degrees ON degrees.id = courses.degree_id
JOIN departments ON departments.id = degrees.department_id
WHERE departments.name = 'Dipartimento di Matematica'
