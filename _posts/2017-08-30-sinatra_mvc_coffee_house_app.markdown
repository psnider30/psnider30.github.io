---
layout: post
title:  "SINATRA MVC COFFEE HOUSE APP"
date:   2017-08-30 17:38:55 -0400
---


My Sinatra portfolio project App is for reviewing Coffee Houses. I started with a User model and a CoffeeHouse model. I knew I wanted a Coffee House to be reviewable by more than one user and certainly a user could create many reviews. Thus I needed a join table and for the models to have a many to many relationship. In Learn we had done something a little similar in NYC-Sinatra, but this functioned differently and was also different from Fwitter where a tweet belongs to a user.

I built up the file structure from scratch looking back some at Avi's video and past projects. The bcrypt gem is used with the password_digest column in the users table and the has_secure_password macro is used in the User model, so all passwords are secured. Sessions is enabled, so there is a way to keep track of if someone is logged in and who and what permissions they have, such as only being able to edit and delete their own posts.

I added name and location to the Coffee House slug, so the app could handle Coffee Houses with the same name, but different locations. In the view show_coffee_house.erb I needed code that could show the correct username for each Coffee House review and this required checking my join table for the match with nested each loops. I implemented the same pattern in the get '/coffee_houses/:slug' route to make sure only the user responsible for the exact Coffee House instance and review can view the edit page and form. If I re-wrote the models and app, I could have used a join table called reviews that handled more logic to simplify the app. This may have been an easier way to think about the models and their associations and interactions.

Update: I refactored the app to use the join table reviews with the fields: content, user_id, coffee_house_id. The review MVC structure is now responsible for creating and editing reviews by a user for a coffee house. Previously every time I created a review, I created an instance of CoffeeHouse and then I filtered the instances by name and location. Now, the app is much cleaner and only creates a new CoffeeHouse by using find_or_create as part of the form and action for creating a new review. This greatly simplified the code and logic of the app. I knew there was simpler way, and I'm glad I was able to implement it while leaving the code for users largley untouched. For example, now finding the correct user for a review simply requires calling @review.user as opposed to nested loops as described above.

The app is working great and I enjoyed building a custom app with Sinatra from scratch!
