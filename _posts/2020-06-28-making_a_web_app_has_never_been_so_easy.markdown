---
layout: post
title:      "Making a web app has never been so easy. "
date:       2020-06-28 23:51:30 +0000
permalink:  making_a_web_app_has_never_been_so_easy
---


Have you ever wanted to make a website? I mean like a *real* website with all the bells and whistles like a functioning database and a log in feature. If you have told be some time ago that this would be possible to create within a matter of days, I would have called you bonkers. With the power of Sinatra, I was able to create a powerful web application of my own within, drumroll please... yes, a matter of days. Let me take you through the 2 things that are still ringing in my head after it all. 

# A Word On The Initial Set Up



When making a web app from ground up it is important to think about the file structure, otherwise things can start to get messy really fast. We call this * seperation of concerns*. A good rule of thumb to follow is the **M**odel **V**iew **C**ontroller idea. 

* Model consist of your classes like User and whatever classes belong to the User, in the simplest of apps. This is your back-end. 
* View is everything that is presented to your user like HTML, CSS, and Javascript. Your front-end. 
* Controller is resposible of handling the many request that the user makes and it directs or redirects the user to various routes within the site. It links the front-end and the back-end. 

Instead of setting this up all these files one by one, I used the Corneal Gem which just did it for me.  So hooray to being a lazy programmer! This will save you alot of time, especially after doing it again and again. 


# I Love ActiveRecord 

ActiveRecord *actively* has your back. I had set out to implement a password confirmation field for users when signing up. The methods needed to accomplish this were made possible by one line of code. Upon further reading, I learned that password confirmation is also included. 

`has_secure_password` 

This is how it would be done on a basic sign up form: 

```

      <label class="w3-left">Password</label><br>
      <input type="password" class="w3-input w3-border" id="password" name="password" /><br>
      <label class="w3-left">Confirm Password</label><br>
      <input type="password" class="w3-input w3-border" id="password" name="password_confirmation" /><br>
      <input type="submit" value="Sign Up"/>
			
	```
			
Just add `name="password_confirmation"` on the password confirmation input and your done!

If you want the user to be able to change to password, the process is very similiar. Create another form with an input field for the old password, the new password, and confirmation of that new password. I may look like this: 

```
        <form method="POST" action="/users/<%=current_user.id%>">
            <label class="w3-left" for="name">Old Password:</label>
            <input type="password" class="w3-input w3-border" id="name" name="old_password" /><br>
            <label class="w3-left" for="name">New Password:</label>
            <input type="password" class="w3-input w3-border" id="name" name="password" /><br>
            <label class="w3-left" for="location">Confirm New Password:</label>
            <input  type="password" class="w3-left w3-input w3-border" id="location" name="password_confirmation" /><br>
            <input style="margin-top: 10px;" type="submit" value="Submit"/><br>
        </form>
```

In the controller, you can safely verify the old password with  `#authenticate` before setting a new password. 


```
  post '/users/:id' do 
    if current_user.authenticate(params[:old_password])
      current_user.update(password: params[:password], password_confirmation: params[:password_confirmation])
      redirect "/users/#{current_user.id}"
    else
      redirect "/users/#{current_user.id}/password"
    end
  end 
```
	

	
	
No one said it had to be hard! I could have built these methods out myself, but in general it is best to use these higher level methods. I mean, they are there for a reason. 
	
# 	Final Words 
Having a proper set up and good tools is half of the battle in making useful real-world applications. Knowing how to use them is the other half of the battle. Of course, we are all on an seemingly endless journey on how to best use the tools, and even creating some of our own tools along the way. In one month I will have a created an another application, but with Rails. I eagerly anticipate telling you about it. 

Happy Coding = )



	





