# Module 07

## Test results (GUI)

### `/all-student`

![/all-student test result (GUI)](./images/test_plan_1_gui.png)

### `/highest-gpa`

![/highest-gpa test result (GUI)](./images/test_plan_2_gui.png)

### `/all-student-name`

![/all-student-name test result (GUI)](./images/test_plan_3_gui.png)

## Test results (CLI)

### `/all-student`

![/all-student test result (CLI)](./images/test_result_log_1.png)

### `/highest-gpa`

![/highest-gpa test result (CLI)](./images/test_result_log_2.png)

### `/all-student-name

![/all-student-name test result (CLI)](./images/test_result_log_3.png)

## Reflection

### 1. JMeter vs. IntelliJ Profiler

### 2. How the profiling process helps

The profiling processes helps me by showing me which of my methods are consuming the most resources when performing a specific task.

In these examples the benefits aren't really apparent because each controller only calls one corresponding service method, so it's obvious where the performance bottlenecks are, but I imagine when my app gets bigger, and each method may call several different methods, profiling will help me find which of these methods are actually bottlenecking my performance, and therefore should be the focus of my optimization efforts.

### 3. Is IntelliJ Profiler effective?

### 4. Performance testing and profiling challenges

### 5. IntelliJ Profiler benefits

### 6. Handling inconsistent findings

### 7. Optimization strategies

Optimizing the `getAllStudentsWithCourses` method was trivial.
The original implementation was manually constructing `StudentCourse` instances
by first getting all students from the `StudentRepository`,
and then querying the `StudentCourseRepository` for each students' `StudentCourse`s,
and then creating new StudentCourse objects that are just copies of the originals.
My optimized version just calls `studentCourseRepository.findAll()` directly.

The original `findStudentWithHighestGpa` method queried all students from the `StudentRepository`,
then iterated through all of them to find the one with the highest GPA.
I optimized this method by making use of Spring Data JPA's query methods.
I simply declared a new method, `findTopByOrderByGpaDesc`, on `StudentRepository`,
and Spring automatically implements it for me.
Then I can just call it from `findStudentWithHighestGpa`.
This way we move the sorting operation to the database, which is much better at handling these things.

To optimize the `joinStudentNames` method,
I just replaced `String` with `StringBuilder`.
`String` concatenation in Java is infamous for being very expensive.
Java `String`s are actually immutable,
so when you "concatenate" to a `String`, it actually creates a new `String` entirely.
`StringBuilder` avoids these expensive object creations by using an internal `char` array.
