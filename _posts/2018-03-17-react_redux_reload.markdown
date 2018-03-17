---
layout: post
title:      "React, Redux, Reload!"
date:       2018-03-17 04:13:50 -0400
permalink:  react_redux_reload
---


Building this app has been an incredible journey starting with a fairly modest understanding of react and redux and ending with an awesome app that taught me an absolute ton. I'm now immensely more comfortable with the data flow of react and react & redux. I first built my app's basic framework with react and then converted to also use redux before I added some cool functionality and started to feel like  adding new features was only limited by my imagination.

My app fetches stock market data based on a symbol for five different layouts of quote/main, change summary (over different time periods), financials, fundamentals, and daily or monthly historical returns. One of my first challenges was fetching the stock data from 7 different API endpoints for each symbol and assigning the values to different object keys for an asset before passing the asset to my reducer. Luckily there was a perfect solution using Promise.all which can take in an array of fetches and then assign the data when the promise returns by array index from the fetch.

As my app expanded I found there were more and more reasons to implement conditional rendering and I found it interesting to find ways to make this happen, such as with simple view usage react state for button rendering or by checking the redux state, such as the layout or assetToUpdate, and of course mapping the state to props and using it in a conditional in the render method. As the app grew it became necessary to organize the app into more components including a Header, Navbar, and OptionsButton. The OptionsButton is used in each layout to see an asset’s returns, update or replace the asset, or remove the asset.

Before moving on to building the Rails API backend with the ability to persist data to a database I thought about why my app should have a backend and I decided I wanted users to be able to register, create sessions, and persist assets associated with themselves (the user). I wasn't exactly sure how to implement user registration and sessions with react and redux. I didn't find a package that made it extremely easy or a clear example of exactly what I needed. Instead I looked at many resources and started with a simple reducer that logged in a user (as far as redux state was concerned... but not really). I then built out a login system with the  redux-react-sessions npm package, but I didn’t keep it. Instead I found a better resource, which I will discuss next. I also decided against using the sessions package as it was too abstract for my first implementation in react with redux.. 

The next step was the rails API backend with my assets and users in a many to many relationship and the corresponding basic routes and controller set up.  I still needed to solve how how a front-end and backend would communicate for my users and luckily was pointed to http://www.thegreatcodeadventure.com.  This was a great resource for  using a JSON Web Token issued from rails for user sessions. I also set up user registrations and coded my assets controller to work with the user associations, model validations, and custom error messages.

With the client side connected to the rails API, the next question to solve was no persistence on the client side when the page was refreshed and redux sate was reset to initial values. I found a great way to store and re-fetch asset data in my app by creating assetsInMemory in redux state and linking it to a sessionStorage key which I kept in sync (in redux state) with assets as they were added, updated, or deleted. However, when the page was refreshed I would lose assets, but keep assetsInMemory, and when refreshed again I lost the assetsInMemory also due to syncing them with assets. 

Thus, I needed to add a window load event listener and re-fetch the assets from assetsInMemory if the two were out of sync (as in assets are cleared, but assetsIn Memory remain). You may have to view my code to follow that logic, but it gave me persisted client state that also fetched fresh price data for my stocks. I implemented this in componentWillReceiveProps and also added a button in each layout to refresh the stock data.

```
onLoad(event) {
    const { assets, assetsInMemory, actions, currentUser } = this.props;
    if (sessionStorage.assets && (assetsInMemory.length !== assets.length)) {
      sessionStorage.removeItem('assets')
      actions.loadUserAssets(currentUser)
    }
  }
```

I greatly enjoyed finding interesting ways to interweave fetching stock data, react state, redux state, dispatch actions, reducers, and a rails API backend into a very cool finance app that taught me a bunch and gives me the confidence that I'm now armed to build web apps with another powerful framework.

