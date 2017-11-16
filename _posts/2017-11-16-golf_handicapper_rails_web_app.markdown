---
layout: post
title:      "Golf Handicapper Rails App"
date:       2017-11-16 15:48:31 -0500
permalink:  golf_handicapper_rails_web_app
---


This web app allows users to sing up, create golf courses, tag golf courses, post rounds, and track their handicap among other views and features. I used the devise gem for golfer registration and session authentication. The devise views folder is included in my app, so I could add customization, such as the name field for new and edit registration. Golfers also have the option to sign in with facebook or google using omniauth and the dotenv-rails gem allows client keys to be hidden in .env. Custom authentication methods logged_in?, authenticate_user, authenticate_owner(resource), and authenticate_admin are in the ApplicationHelper module to provide security such as only the golfer who owns a round may delete that round.

The models are Golfer, GolfCourse, Round, Tag, and GolfCourseTag. Rounds is the join table for golfers and golf_courses;  golf_course_tags is the join table for tags and golf_courses. The Rounds resources are nested under golfers and golf_courses. Tags don't need any routes as they are only displayed in golf_course views. The app contains many model validations such as presence of all golf_course required attributes and only courses with 18 holes, total_par > 62, course_rating > 60, and course_slope > 99 are allowed to be saved. Error messages are shown in the div with id = "error_explanation" and that css is provided in application.scss. The bootstrap-sass gem is used and bootstrap is imported in application.scss. In navbar.scss the bootstrap variable are imported to give more customization options.

The class level active record methods displayed on the home page are GolfCourse.highest_course_slope, GolfCourse.lowest_course_slope, Round.low_round_score, Round.low_round_from_par. Round.low_round_net will be added later to be run async with javascript. Also GolfCourse.course_lowest_round is displayed on the golf_course show page.

The custom attribute writer golf_course_attributes=  is in the Round model, nested at form URL: /golfers/:golfer_id/rounds/new, and allows adding a new golf course when posting a new round. The custom attribute writer tags_attributes= is in  the GolfCourse model, nested at form(s) URL(s):  golf_courses/new and golf_courses/edit, and allows adding and removing tags to golf_courses when creating or editing a golf_course.

I tried make the code for my app as dry as possible while providing useful features and views. The view presentation is nice with minimal styling by using bootstrap with some customization and many tables to display data. I think the app has well designed views and navigation and there is some fairly complex logic, especially in the Round model, that allows for great functionality. I also look forward to making improvements in javascript and by refactoring.
