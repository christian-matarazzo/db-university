1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
# SOLUZIONE:
select *
from students
join degrees on students.degree_id = degrees.id
where degrees.name = "Corso di Laurea in Economia";


2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di
Neuroscienze:
# SOLUZIONE :

select *
from degrees
join departments on degrees.department_id = departments.id
where departments.name = 'Dipartimento di Neuroscienze'
and degrees.level = 'magistrale';

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
# SOLUZIONE:

select teachers.name, teachers.surname, teacher_id, courses.name
from course_teacher
join teachers on course_teacher.teacher_id = teachers.id
join courses on course_teacher.course_id = courses.id
where teachers.name = "Fulvio" and surname= "Amato" 

 <!-- un appunto, non userei la richiesta per id qui, perché se domani dovessero cambiare nome e cognome spostando chi cerchiamo, l'id riporterebbe comunque un teacher_id = 44 che potrebbe non essere Fulvio Amato --> 

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui
sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e
nome

# SOLUZIONE :

select students.name as student_name, students.surname as student_surname, departments.name as departments, courses.name as courses
from students
join departments on students.degree_id = departments.id
join courses on students.degree_id = courses.id
order by student_name, student_surname, departments, courses asc

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

6. Selezionare tutti i docenti che insegnano nel Dipartimento di
Matematica (54)

7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti
per ogni esame, stampando anche il voto massimo. Successivamente,
filtrare i tentativi con voto minimo 18.