---
layout: post
title:      "Go Fetch in Ruby - Faraday Gem "
date:       2020-10-05 18:59:21 +0000
permalink:  go_fetch_in_ruby_-_faraday_gem
---


Working with APIs is a daily occurrence for developers. Most resources on the internet show how to retrieve this information with Javascript on the front end, I wanted to demonstrate how this can be done on the server-side with Ruby.  But you may ask, why do fetch calls on the server-side? 

This is a valid question. The answer has to do with making your application more secure. When api keys, client secrets, or client ids are stored in the front end of a web app, they can  be more easily discovered through clever manipulation with the browser's devtools. 

# Faraday Gem Setup 

You are going to need this in your Gemfile `gem 'faraday', '~> 0.9.2'`. 

Then...`bundle install` in the command line and don't forget to throw a little `require faraday` in your code. 

Now...throw everything that you ever had in mind on how to do fetch calls in javascript away. Let me know when you have finished that.  

# The Example 

Now let's examine a post request in Javascript, particularly in a React App. We are trying to get an Oauth token from the Petfinder API.  

```
 fetch("https://api.petfinder.com/v2/oauth2/token", {
                body: `grant_type=client_credentials&client_id=${process.env.REACT_APP_PETFINDER_CLIENT_ID}&client_secret=${process.env.REACT_APP_PETFINDER_CLIENT_SECRET}`,
                headers: {
                    "Content-Type": "application/x-www-form-urlencoded"
                },
                method: "POST"})
                .then(response => response.json() )
                .then(responseJSON => dispatch({ type: 'ADD_API_TOKEN', responseJSON }) )
```

As you can see `fetch` has 4 arguments. 

1. URL
2. Body
3. Headers
4. Method 

This is standard stuff. An asynchronous fetch call with 4 arguments, followed by consecutive `.then` functions  to parse the json response. 

Now let's look at the exact same thing done in Ruby with the Faraday Gem. 

```
            token_response = Faraday.post("https://api.petfinder.com/v2/oauth2/token" ) do |req| 
                req.headers['Content-Type'] = 'application/x-www-form-urlencoded'
                req.body = "grant_type=client_credentials&client_id=#{ENV['PETFINDER_CLIENT_ID']}&client_secret=#{ENV['PETFINDER_CLIENT_SECRET']}"
            end
            data = JSON.parse(token_response.body)
```

A couple of different things are happening here making it different from the Javascript way of doing it. 

1. First thing, we call `post` as a method and we pass the url as an argument to that method.

2. Next we notice `.headers` and `.body` are methods to `req` inside the do block. This is where we add our header and body information. The `req.body` is assigned a string. The `['Content-Type']` key of `req.header` is assigned the value of a string. 

3. The value of this response is made equal to `token_response`. 

4. Finally we assign `JSON.parse(token_response.body)` , which converts that data into hash, to the `data` variable.  


Wow, well that wasn't so hard! 

# Final Words 
There is a shortage of articles on how to do server-side API calls in Ruby so I hope this brief demonstration proves to be helpful. It is a good thing to learn in terms of exposing less of your applications logic to the front-end where it has the potential of being tampered with. Definitely look more into it if you don't already know how to do it. 

Stay Coding :)









