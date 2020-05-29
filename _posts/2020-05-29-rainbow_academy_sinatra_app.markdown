---
layout: post
title:      "Rainbow Academy Sinatra App"
date:       2020-05-29 12:12:01 -0400
permalink:  rainbow_academy_sinatra_app
---


Two years ago, I did a summer internship at [Rainbow Acres](https://rainbowacres.com/), "a [residential] Christian community with heart that empowers persons with developmental disabilities to live to their fullest potential with dignitiy and purpse." I was serving this community as an Information Technology intern, where I participated on a board of educators to brainstorm new computer/ IT methodolgies for launching and instituting an educational academy for adults with cognitive disabilities. 

Rainbow Acres was just starting to launch an educational program in which ranchers (residents of Rainbow Acres) could enroll in a wide variety of courses to enrich their experiences and pursue their own goals and interests. They called this program [Rainbow Academy](https://rainbowacres.com/rainbow-academy/), and hoped to utilize an enrollment system similar to what colleges use to register their students for courses. However, after doing some research into these class management software programs, they realized such programs were far too expensive for what a non-profit organization like Rainbow Acres could afford. 

At the time, I wanted to help but didn't know how. I had taken a handful of computer science courses at University of California, San Diego, but none of them were anything close to creating the sort of online enrollment system or web application that Rainbow Acres so desparatley needed. I tried watching different YouTube videos on how to create basic applications, but it was extremely overwhelming and I didn't know where to begin. I wasn't sure how to work with databases to get my student/ teacher/ course informatiton to persist, and I wasn't sure how to create and launch a website that could handle all the necessary functions. I also tried to see if I could use existing resources like Microsoft Power Apps to create my own application or even web development builders such as wix or wordpress to create this sort of enrollment system. 

However, I kept running into problems, and it seemed like these were just bandaid fixes and not a realistic solution to the course registration problem. Not only did this website or application need to do basic tasks such as student/teacher login, course creation, enrollment, etc., it also had to be user friendly and very self explanatory for those with developmental or cognitive disabilities. 

I needed a simple yet powerful application that could essentially handle student/teacher sign up, login, course creation, course enrollment, and scheduling, but was easy enough for people who are not as tech-savvy to use (both students and teachers) and also wouldn't require a ton of maintenance by one person (me, being the web developer). Some of the website options I was creating using Wix would have been great, but required hefty payments to have access to certain feautres and placed most of the course creation and input responsibilities on me as the website developer rather than the teachers and instructors of the courses. I knew that wouldn't be a feasible long-term solution because I was only a summer intern and would eventually go back to school and have other jobs and responsibilities that would prevent me from being able to update the website each quarter when new classes rolled around. 

Now, two years have gone by and I have enrolled in a programming bootcamp where I have actually learned the skills necessary to make a dynamic web application to handle course enrollment and class management.

The premise is simple: 

### Teachers
- [x] Can sign up, login, and logout with a name and password
- [x] Can create courses 
- [x] Can edit their own courses 
- [x] Can update teachers on their own courses 
- [x] Can update enrollments on their own courses
- [x] Can view their homepage, which includes their daily schedule, weekly schedule, list of courses they teach, and list of students they teach

### Students 
- [x] Can sign up, login, and logout with a name and password
- [x] Can select courses to enroll in from a course catalog of all courses available 
- [x] Can update their own enrollments (add or drop courses), as long as the course they enroll in does not exceed capacity
- [x] Can view their homepage, which includes their daily schedule, weekly schedule, list of courses they're enrolled in, and list of teachers they have

### Courses
- [x] Have a name, description, capacity, location, weekly schedule, start time, and duration 
- [x] Have a list of teacher(s) who teach this course 
- [x] Have a list of student(s) who are enrolled in this course

### Validation Features
- [x] Prevents people from seeing course details without logging in
- [x] Prevents duplicate teacher names 
- [x] Prevents duplicate student names 
- [x] Prevents duplicate course names 
- [x] Prevents courses from being created if they do not have a valid name, description, valid capacity, location, start time, and duration
- [x] Prevents courses from being enrolled beyond capacity 
- [x] Prevents students from editing, deleting, and updating any course 
- [x] Prevents teachers from editing, deleting, and updating courses they do not teach

### Future Features
- [ ] Waitlisting classes 
- [ ] Admin role (can CRUD all students, teachers, and courses)
- [ ] Attendance log 
- [ ] History of changes log (CRUD on all students, teachers, and courses)
- [ ] Schedule overlap prevention 
- [ ] Improving the schedule layout/ design

### Recommended Steps:

1. Sign up as a student or teacher
2. View all course listings 
3. View your homepage
4. Create courses as a teacher
5. Enroll in courses as a student
6. Manage your classes as a teacher
7. Manage your enrollments as a student 
8. Logout

## Planning Design and Implementation
The implementation of this application required applying many new skills, in particular following a Model-View-Controller software architecture/ design pattern and using ActiveRecord with Sinatra. 

### Models and Attributes
- Teacher (user login): name, password 
- Student (user login): name, password
- Course (resource): name, description, capacity, location, weekly schedule, start time, and duration
- Enrollments (join table) with foreign keys: student id, course id 
- TeacherCourses (join table) with foreign keys: teacher id, course id 

### Relationships 
- Teacher: has many courses through teacher_couses, has many students through courses
- Student: has many courses through enrollments, has many teachers through courses
- Course: has many enrollments, has many teacher_courses
- Enrollments: belongs to student, belongs to course 
- TeacherCourses: belongs to teacher, belongs to course 

### Tools to allow users to sign up, sign in, and sign out
- ActiveRecord 
- ActiveRecord's has_secure_password
- Sessions
- RESTful routes specific for sign up, login, and logout

### User Login
- name: will not allow duplicate teacher/ student/ course names in database
- password: will be encrypted through ActiveRecord's has_secure_password

### CRUD Action Model
- Courses, Enrollments, TeacherCourses

### Preventing deleting/ editing other user's resources
- ERB view pages are protected with helper methods such as #logged_in? #current_user #teacher? #student?
- I only allow users to edit and delete things they've created
- I only allow teachers to edit their own courses
- I do not allow students to edit any courses
- I only allow students to edit their own enrollments 


Stay tuned for a video demo/ walkthrough of my app on [GitHub](https://github.com/zchesny/rainbow-academy)! If you’d like to see how it works, clone or download the repo here, and try it out for yourself! I look forward to working on new coding projects and implementing a live version of this website for Rainbow Acres to use in the future :-)
