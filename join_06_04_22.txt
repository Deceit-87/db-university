-- 1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

    SELECT `degrees`.`name`,`students`.`name`,`students`.`surname
    FROM `degrees`
    INNER JOIN `students` ON `students`.`degree_id` = `degrees`.`id`
    WHERE `degrees`.`name` = 'corso di laurea in economia';


---------------------------------------------------------------------------------------------------


-- 2. Selezionare tutti i Corsi di Laurea del Dipartimento di Neuroscienze

    SELECT `degrees`.`name`
    FROM `degrees`
    INNER JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
    WHERE `departments`.`name`= 'Dipartimento di Neuroscienze';



---------------------------------------------------------------------------------------------------


-- 3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

    SELECT `courses`.`name`
    FROM `courses`
    INNER JOIN `course_teacher` ON `course_teacher`.`course_id` = `courses`.`id`
    INNER JOIN `teachers` ON `teachers`.`id` = `course_teacher`.`teacher_id`
    WHERE `teachers`.`id` = 44;




---------------------------------------------------------------------------------------------------


-- 4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il
      relativo dipartimento, in ordine alfabetico per cognome e nome

    SELECT `students`.`surname`,`students`.`name`,`degrees`.*,`departments`.`name`
    FROM `students`
    INNER JOIN `degrees` ON `degrees`.`id` = `students`.`degree_id`
    INNER JOIN `departments` ON `departments`.`id` = `degrees`.`department_id`
    ORDER  BY `students`.`surname` ,`students`.`name` ASC;


---------------------------------------------------------------------------------------------------


-- 5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

    SELECT `degrees`.`name`,`courses`.`name`,`teachers`.`surname`,`teachers`.`name`
    FROM `degrees`
    INNER JOIN `courses` ON `courses`.`degree_id`= `degrees`.`id`
    INNER JOIN `course_teacher` ON `course_teacher`.`course_id`=`courses`.`id`
    INNER JOIN `teachers` ON `teachers`.`id`= `course_teacher`.`teacher_id`
    ORDER BY `degrees`.`name`,`teachers`.`surname`,`teachers`.`name`ASC


---------------------------------------------------------------------------------------------------


-- 6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)

    SELECT `teachers`.`surname`,`teachers`.`name`,`departments`.`name`
    FROM `teachers`
    INNER JOIN `course_teacher` ON `teachers`.`id`=`course_teacher`.`teacher_id`
    INNER JOIN `courses` ON `course_teacher`.`course_id` = `courses`.`id`
    INNER JOIN `degrees` ON `courses`.`degree_id`=`degrees`.`id`
    INNER JOIN `departments` ON `degrees`.`department_id`= `departments`.`id`
    WHERE `departments`.`name` = 'Dipartimento di Matematica'
    GROUP BY `teachers`.`id`
    ORDER BY `teachers`.`surname`,`teachers`.`name`ASC  


---------------------------------------------------------------------------------------------------


-- 7. BONUS: Selezionare per ogni studente quanti tentativi d’esame ha sostenuto per
-- superare ciascuno dei suoi esami

    SELECT `students`.`surname`,`students`.`name`,COUNT(`exam_student`.`vote`) AS `esami_disputati`,`courses`.`name`
    FROM `students`
    INNER JOIN `exam_student` ON `students`.`id`= `exam_student`.`student_id`
    INNER JOIN `exams` ON `exam_student`.`exam_id` = `exams`.`id`
    INNER JOIN `courses` ON `exams`.`course_id` = `courses`.`id` 
    GROUP BY `students`.`id`,`courses`.`id`
    HAVING MAX(`exam_student`.`vote`) >= 18 
