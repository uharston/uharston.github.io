---
layout: post
title:      "Sometimes we all need a helping hand. "
date:       2020-08-03 03:57:52 +0000
permalink:  sometimes_we_all_need_a_helping_hand
---


Programming can be tough. App development can be even tougher. There is so much to think about. The planning stage, the actually making it work stage, and finally refactoring. Thanks to Rails MVC and seperation of concerns, it makes it so much easier to keep your code readable for the next developer and even yourself down the road. What I want to highlight in this post is, *drumroll*, view helper methods! 

In case you don't know what I am talking about, helper methods "help" (kinda pun) the ruby logic out of your view (also kinda pun). Nobody wants too much logic in their view folders. 

Your helper folder lives nested in the app folder. If you created your models with `rails g controller` then you should automatically have a helper module for each corresponding view. 

A quick example. Perhaps you want to customize the title of a board, say on Pinterest. You want it to say "My Board" if it is your, and you want it to say "Kenneth's Board" if someone else is visiting your board.  You might find something like this in your view: 


```
<div class="w3-container">
    <div class="w3-section w3-bottombar w3-padding-16">
        <div class="w3-center w3-margin-top">
        <%if @user == current_user%>
            <h1 class="w3-jumbo w3-margin-top">My Garage</h1>
        <%else %>
            <%= @user.first_name + "'s Garage"%>
        <%end%>
        </div>
    </div>
</div>
```


We would like to take this if statement away. It doesn't need to be in our view. So we move its helper module (`users_helper.rb`) . Like this: 

```

    def garage_title(user)
        if @user == current_user
            "My Garage"
        else 
            @user.first_name + "'s Garage"
        end
    end 
```

Now we have replaced all that logic with a simple helper method with the @user instance. 

```
<div class="w3-container">
<br>
    <div class="w3-section w3-bottombar w3-padding-16">
        <div class="w3-center w3-margin-top">
            <h1 class="w3-jumbo w3-margin-top"><%= garage_title(@user) %></h1>
        </div>
    </div>
</div>
```

The above example is a small example for illustrative purposes, but helper methods can take alot of clutter out of your view. They should not be taken lightly! 

As always, thanks for reading and happy coding! =)

