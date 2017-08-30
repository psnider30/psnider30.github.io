---
layout: post
title:  "SINATRA MVC COFFEE HOUSE APP"
date:   2017-08-30 21:38:54 +0000
---


My Sinatra portfolio project App is for reviewing Coffee Houses. I started with a User model and a CoffeeHouse model. I knew I wanted a Coffee House to be reviewable by more than one user and certainly a user could create many reviews. Thus I needed a join table and for the models to have a many to many relationship. In Learn we had done something a little similar in NYC-Sinatra, but this functioned differently and was also different from Fwitter where a tweet belongs to a user.

I built up the file structure from scratch looking back some at Avi's video and past projects. The bcypt gem is used with the password_digest column in the users table and the has_secure_password macro in the User model, so all passwords are secured. Sessions is enabled, so there is a way to keep track who and if is logged in and what permissions they have, such as only being able to edit and delete their own posts.

I added name and location to the Coffee House slug, so the app could handle Coffee Houses with the same name, but different locations. In the view show_coffee_house.erb I needed code that could show the correct username for each Coffee House review and this required checking my join table for the match with nested each loops. I implemented the same pattern in the get '/coffee_houses/:slug' route to make sure only the user responsible for the exact Coffee House instance and review can view the edit page and form.

The app is working great and I enjoyed building a custom app with Sinatra from scratch!
