---
layout: post
title:      "Course Clicker Ruby on Rails App"
date:       2020-08-31 00:50:42 +0000
permalink:  course_clicker_ruby_on_rails_app
---


Wow things have been busy these past couple of months! I actually finished the meat of this application in late June, right before I started working full-time as a Platform Engineer at Illumina July 6th! I was trying to push through and get everything done before then, but I didn't manage to meet all the project requirements in time. However, I was able to finish this project and get it up and running in a working state for Rainbow Acres to use in early July! This application now functions as a live website at https://course-clicker.herokuapp.com/ and is currently being used by Rainbow Acres for their course registration and enrollment!

This has been a very in-depth collaborative process where I have been in constant contact with members of the Rainbow Acres education team in the development of this web application. This initially started with an internship I did two years ago, and got a kickstart with my last project, [Rainbow Academy Sinatray App](https://youtu.be/BLv3gZwSkYI). You can read more about it in my previous [blog post](https://zchesny.github.io/rainbow_academy_sinatra_app). Anyway, after showing the education team the demo of my [Sinatra project](https://www.youtube.com/watch?v=BLv3gZwSkYI&feature=youtu.be), they were actually super interested and wondered if I could really implement this for them to use as part of their educational program. I definitely wanted to, but wasn't quite sure how. After having my project review for the Rainbow Academy app, I asked for help on making this a "live" web application. My instructor said that this isn't explicitly taught in this coding bootcamp, but many students have used [Heroku](https://www.heroku.com/) to host their applications in the past. 

At first this sounded like a great idea and right away I tried to see if I could easily upload my [Rainbow Academy](https://github.com/zchesny/rainbow-academy) code to Heroku and then log in anywhere using any computer! However, it was that easy. I ran into numerous problems, the first one being Heroku requires postgresql or comparable and does not run using sqlite. The only thing I learned in the class was sqlite and I wasn't quite sure how to switch in between! So my first brilliant idea was to simply replace all the references to "sqlite3" with "postgresql" and of course add the 'pg' gem to my Gemfile. Again. NOT THAT SIMPLE! I tried googling a few different ways to change from sqlite3 to postgresql, but none of them seemed to work for me. I would constantly run into build errors and found myself staring blankly at hundreds of lines of heroku logs and wasn't quite sure where to begin/ debug. After an honest attempt at hosting my Sinatra app using Heroku, I decided maybe it would be better to start fresh anyway. Besides, I had just started learning this great new thing called Ruby on Rails, and could already appreciate it's power ease-of-use for creating dynamic, robust, and larger-scale web applications (in comparison to Sinatra). Instead of writing my own routes each time, Rails had so many built in features (especially relating to CRUD!) and a lot of the grunt-work/ file architecture and setup was taken care of for me with a few quick commands to the terminal! However, I'm definitely grateful this bootcamp had us learn Sinatra first, because that way I could truly appreciate everything Rails has to offer. It also helped me gain a deeper understanding of what Rails does instead of "magically" typing commands into the terminal and seeing routes and resources appear out of seemingly nowhere. Anyway, I then was slightly conflicted but easily convinced between trying a little more and a little harder at getting my original Sinatra code up and running onto Heroku or just diving head-first into Rails, recreating my application and starting it out with Heroku. 

Once I started learning more about Rails and all it could do, I was easily convinced this was the way to go. I could create a lot cleaner code, refactor much of the logic I had written before, and I had a solid base/ foundation of what I wanted the application to do/ how to implement it. Now it was just a matter of integrating it with the newly learned Rails framework and watching it soar!

I had my first phase of the project done within about 5 days and made a video demo to show Rainbow Acres. I also started out using Heroku and Postgresql and followed the [Heroku documentation](https://devcenter.heroku.com/articles/getting-started-with-rails5) on how to get a Ruby on Rails web application up and running on a live website right out of the box. I still ran into some hiccups along the way, but overall this process was MUCH smoother than trying to change my Sinatra code after the fact. 

HOWEVER, another issue arose! Rainbow Acres was not sure if they could actually use my project due to HIPPA considerations. Resident's rights on Rainbow Acres is *extremely* important, and using their names on this website along with reference to "Rainbow Acres" was a violation of [Geographical Identifier Information](https://en.wikipedia.org/wiki/Protected_health_information). Therefore, this required some brainstorming. After several discussions between me, the director of education, and the CFO at Rainbow Acres, we decided the best way to meet HIPPA compliance was to not use full names and also have no reference to Rainbow Acres. 

## ENTER: COURSE CLICKER! 
I renamed the application to "Course Clicker," inspired by ["Cookie Clicker"](https://orteil.dashnet.org/cookieclicker/) because I liked the alliteration and I thought the name made sense: you click the courses you're interested in and click around to create courses, take attendance, enroll, etc. 

Anyway, after my first draft of the web application and [YouTube demo](https://youtu.be/6zCuxjJ9s2w), the director of education was thrilled and also had some suggestions. My original version of the application basically did what the Sinatra application did, but with the added feature of an admin user and the ability to track attendance. Another feature I addded (which was a new concept I wasn't sure at first how to implement) was using a registration code during sign up. The intention was to protect this application from random people on the internet signing up and becoming admins (scary!), teachers, and students. Originally I thought I should just protect the admin/ teacher feature, but then I realized it makes the most sense to protect all the 3. Therefore, I used [Heroku config variables](https://devcenter.heroku.com/articles/config-vars) to do the job of storing a "password/ registration code" but keeping it hidden from my public GitHub code base. As a result, I was able to implement requiring separate registration codes for student, teacher, and admin sign up. 

#### Side note
It required a lot of thought and contemplation as to how to implement students, teachers, and admins into the app: using either one database table "users" or one table per user group: "students", "teachers", "admins." In my Sinatra web application, I did the latter, but adding another user type "Admin" made me more inclined to choose the former. I went back and forth a few times (even with the code), but I eventually decided to use one database table, "users" because I thought it would keep my code cleaner and more manageable, and also make for a nicer user experience where people don't have to identify what type of user they are when the log in... they can just log in!

## Improvements
Numerous back and forth with the director of education lead to the following improvements/ new features to my application: 
1. Addition of a course location drop down menu (instead of manually typing the location in)
2. Addition of a "print" button to specifically print the weekly schedule of each individual 
3. Addition of class times to the weekly schedule 
4. Deletion of "Saturday/ Sunday" columns in weekly schedule
5. Addition of Rancher's name to the weekly schedule (for printing)
6. Fixed confidentiality breech in attendance: changed access so a student can only view their own attendance and not the attendance for the rest of the class 
7. Deletion of "Present" column in attendance record. It is more important to know the names of those who are absent instead, especially as class sizes grow
8. Teachers now have the ability to view attendance for any class/ student 
9. Fixed bug where courses and attendances index wouldn't work if no time was entered (made a new validation to require a start time for courses and attendance date for attendances)
10. Addition of "print" button to attendance report
11. Location now shown on weekly schedule 
12. Added 50 minute time duration to courses 
13. Admins can now sort users by roles 
14. Addition of location to course catalog page 
15. Added filtering options for courses (by schedule, by time, by location, by teacher)

This has definitely been a learning adventure, but it has been so much fun to be able to update and cater this web application to the specific needs of the Rainbow Acres educational team. I have loved working with them and being able to see the changes go live with every line of code I add. 

## Final Updates
The reason it took so long for me to finish this project is that I still needed to add a couple of features to the version required for the coding bootcamp. The additional features are as follows: 
1. OmniAuth Facebook login (for students only) 
2. Grading scale option (because this is a user-submittable attribute for a "through" part (join-table) of a has-many-through relationship
2. #most_courses (Admins can now view the users with the most courses) 

Thanks for reading this blog, and I hope you enjoyed my walkthrough journey of how Course Clicker came to be :-). Here's the links for more info: 
- [GitHub Repo](https://github.com/zchesny/course-clicker)
- [Final Demo](https://youtu.be/DAmorecbMBQ)
- [Live Web App](https://course-clicker.herokuapp.com/)
- [Planning Sheet with Access Policy](https://docs.google.com/spreadsheets/d/1uzNqOYMaMAswLWx8K2aFnDoHcXpznIHj13SrWpwtvlE/edit?usp=sharing)
- [Initial Database Map](https://drive.google.com/file/d/1nqOUt0jxlQwb7jJyi2XY87KSinA4W8Vs/view?usp=sharing) note: hass been refined since then 


