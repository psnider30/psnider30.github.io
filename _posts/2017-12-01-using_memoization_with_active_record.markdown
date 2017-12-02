---
layout: post
title:      "Using memoization with Active Record"
date:       2017-12-01 18:10:10 -0500
permalink:  using_memoization_with_active_record
---


When I built my rails project app that tracks handicaps for golfers I ran into a speed problem on my home page, which calls several class methods that rely on several instance methods that set off a bunch of database calls and in total it was taking close to 6000 ms to load the page. That 6000 ms translated to probably 5 - 6 seconds, which is way too slow for an end user and for me as well. My app wasn't using any memoization at this point and I knew I had to fix that.

I came across some fairly simple patterns that could fix my problems, but it took a good amount of reading and experimentation to find and implement the best approach. The simple "||=" memoization technique was going to need some added methods in order to function correctly. The variables need to unmemoize correctly when an activerecord database change occurs, such as creating/updating/deleting a round, golfer, or golf course. 

I installed the [https://github.com/matthewrudy/memoist](memoist gem), which is an extraction of ActiveSupport::Memoizable that has been depreciated in base rails since 2011. The setup is simple and after you add the gem to your gemfile, you just require 'memoist' and extend Memoist in your desired class(es) as well as listing your methods to memoize as such - memoize :first_method, :second_method. The gem also provides code to check if the instance variables have been defined, so you don't get any unexpected errors. Then, since I want the memoization to be reset after changes to the database for each model, I set after_commit (which is called after the database transaction is completed) to the memoist gem provided method unmemoize_all. This worked great for all my instance methods!

The Memoist gem documentation also provides a way to memoize variables created by class methods. However, I could not get this to work correctly as my variables would not unmemoize after database transactions. Thus it took some experimenting, but I found a pattern that worked great for my needs. I simply set the class method created variables with "||=" and then I added to the controllers an after_action with a custom callback method to set desired memoized variables back to nil. A syntax example is below:


```
after_action :unmemoize_golf_course_variables, only: [:create, :update, :destroy] 
```

Thus when database changes are made the memoized values are removed, re-calculated and memoized again. It's important to remember to clear out any associated memoized variables from other classes if you implement this in each controller as opposed to just using it for everything in the application controller.

Adding memoization for my app brought down the page load speeds quite a bit. Most the pages load in under 100 ms. The slower home page now loads in about 1000 - 1500 ms when the server is first started and generally under 100 ms if no database changes occur between the next load. The first home page load is now fairly acceptable, but it likely can be optimized in my next update to include javascript by performing some of the work with asynchronous execution.

My repo implementing the above memoization is located here: [https://github.com/psnider30/golf-handicapper-rails-app](https://github.com/psnider30/golf-handicapper-rails-app)
