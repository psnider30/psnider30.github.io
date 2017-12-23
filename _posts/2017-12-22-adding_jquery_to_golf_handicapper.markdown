---
layout: post
title:      "Adding jQuery to Golf Handicapper"
date:       2017-12-23 02:35:36 +0000
permalink:  adding_jquery_to_golf_handicapper
---


Adding the jQuery Front End to my rails handicapper web app was great for allowing the app to me more interactive without page reloads. It is nice how it is fairly easy to create a json backend for a Rails app by using the Active Model Serializer gem. I also made use of the to_json method in the GolfCourse index action, which is convenient for small api customizations without many attributes or nesting. The app uses handlebars templates for new elements that need to be rendered based on the json and/or js Objects from ajax requests. Once the json backend is set up, its very convenient to make ajax requests. 

Of course building the html template twice (once in html and once in handlebars) can't be avoided, but handlebars is great to reproduce the same html from the json. With the json back-end and handlebars template, it does require more thought when changing the rails views. For example, when I changed the rounds to be  ordered by created_at desc at the end of the project. This made wonder if I needed to change the json backend, but luckily it was easily solved by sorting the order of rounds in the js files, which only required calling reverse on the this.rounds attribute.

I rendered the Golfer show page using jQuery and an Active Model Serialization json backend via a next button click event. The code in golfer.js makes sure to re-loop when the last Golfer id is reached and begin again at the first Golfer id. I put these values in the data-attributes in my html.erb file, which is a pattern I made great use of throughout my web app to allow javascript to grab variables that originated in rails when needed. A handlebars helper method ifPermissionGolfer is added so only the golfer who owns the resource can do posting, editing, and deleting actions.

I added a jquery feature that allows the GolfCourse rounds index page to be rendered using jQuery via a button click event. I also added a feature for a Golfer to post a new Round on the GC show page via ajax. A new model GolfCourseComments was added to the rails stack to allow a Golfer to comment on courses. On the GC show page, a new comment can be created from the rails api and comments are rendered on the page via ajax. Comments can also be deleted via ajax. Since I'm not rendering a form for each comment I chose to make an edit link visible via an ajax dblclick event, but the edit itself does not happen on the same page using ajax as of now. 

In golfers.js, golf_course.js, rounds.js and golf_course_comments.js the json responses are translated into js model objects. I have 5 prototype methods on my js objects, for example, GolfCourse.prototype.renderGolfCourseRounds. The class level summary information on the home page is now done via json and ajax so the page load time is improved since fewer active record class level methods trigger less database requests.

I also made some great styling improvements to my home page in welcome.scss. Some of the  project jQuery was simple to implement and some had unexpected behavior such as disabled buttons and unattached click events which took some work to correct. I definitely learned a ton building the site and I'm happy with the result. I also get an idea why front end libraries or frameworks like React can be nice to make behavior more predictable on front end components and features.
