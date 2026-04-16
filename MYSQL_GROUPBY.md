1. Contare quanti iscritti ci sono stati ogni anno

# SOLUZIONE:

select year(enrolment_date) as enrolment_year, count(*) as students_numbers
from students
group by year(enrolment_date)
order by enrolment_year desc


2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio

# SOLUZIONE:

select teachers.office_address as office_address, count(*) as teachers_numbers
from teachers
group by teachers.office_address
order by office_address desc


3. Calcolare la media dei voti di ogni appello d'esame

# SOLUZIONE : 

select distinct exam_id, round(avg(vote),2) as exam_media
from exam_student
group by exam_id


4. Contare quanti corsi di laurea ci sono per ogni dipartimento