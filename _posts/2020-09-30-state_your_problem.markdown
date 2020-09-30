---
layout: post
title:      "STATE your problem..."
date:       2020-09-30 01:33:15 -0400
permalink:  state_your_problem
---


*This post comes just before the brink of completing the Flatiron Full-Time Software Enginneering Course. Feels pretty good...anyways. *

Lets talk a little bit about Redux. If you don't know Redux, it is a method of managing all of a React application's state into one convenient place, or how they like to say, inside the Store. Think store like storehouse. Yes even simple applications can get cluttered quick, so instead of trying to find where you left your socks (maybe under the bed??), you can look inside the Redux storehouse. Your socks will always be nice and neatly placed in the storehouse. 

So enough metaphors, let's see some examples! 

# Too Much State?

```
state = { 
    pets: [], 
    loading: false, 
    api_token: {}, 
    breeds: [], 
    user: { 
        name: '', 
        email: '', 
        image_url: '', 
        logged_in: false, 
        favorite_ids: [], 
        favorite_pets: [] 
    } 
}


```

In my dog adoption project called Pooch, here is all of the state that I needed to keep track of. How can I manage all this state?

1. I could have made it live in the the `App.js` Component and pass it down as props (properties) to children components. 
2. I could have tried to think about in which Components each state should live in and then pass props to children components. 
3. Lastly, I could place all of this state inside the Store. By doing so, I will be able to access state from *any* Component with some help of the `connect` function. 

But first, an important question: **How should I decide which state should live in the Component verse the Store? **

Think dynamic data. Data that is returned from asynchronous calls, API data, user data, you name it. This information is likely to affect your entire application. So it will be convienant to be able to access from anywhere. 

The type of state that would be useful inside the Component would be unique to that Component. Perhaps a toggle button controlled by a simple boolean. Another example would be this flippable PetCard Component which uses the `ReactCardFlip` package. 

```
class PetCard extends Component {

    state = {
        isFlipped: false
    }

    handleFlip = () => {
        this.setState( prevState => ({
            isFlipped: !prevState.isFlipped
        }))
    }

    render() {
        return(
            <div className='pets'>
                <ReactCardFlip isFlipped={this.state.isFlipped} flipDirection="horizontal">
                    <PetCardFront pet={this.props.pet} handleFlip={this.handleFlip} />
                    <PetCardBack pet={this.props.pet} handleFlip={this.handleFlip} />
                </ReactCardFlip>            
            </div>
        )
    }
```

# Setup Redux
First you need to import a couple things in your `index.js` file. 

```
import React from 'react';

import { createStore } from 'redux';
import { Provider } from 'react-redux';
import usersReducer from './reducers/usersReducer';

import App from './App';

const store = createStore(usersReducer)

ReactDOM.render(
  <Provider store={store}>
      <App />
  </Provider>,
  document.getElementById('root')
);
```

We could break down what each thing is doing in detail but put simply we see the Provider Component from `react-redux` will wrap our `<App/>` , then allowing every the Component the *option* to connect to the store if they so choose. 

The `createStore` method take in a reducer as an argument, and is then passed into the Provider Component. But what is a reducer? 

Well I am glad you ask! The reducer is where all of the state lives. I guess you can say the reducer is the store, that is if we continue the storehouse metaphor...You get the point. So let's see an example below. 

```
const usersReducer = ( state = { user: { name: '', email: '', image_url: '', logged_in: false, favorite_ids: [], favorite_pets: [] } }, action ) => {
    
    switch(action.type) {
   case 'LOGIN_USER': 
            return {
                ...state, 
                user: {
                    name: action.response.data.name,
                    email: action.response.data.email,
                    image_url: action.response.data.image_url,
                    logged_in: true,
                    favorite_ids: action.response.data.pets,
                    favorite_pets: []
                }
            }
        case 'LOGIN_ERROR': 
        
            return {
                ...state, 
                user: action.response.data 
            }
        case 'LOGOUT_USER': 
        
            return{ 
                ...state, 
                user: {logged_in: action.logged_in}
            }
}
export default usersReducer
```

This reducer is has all of the state related to the user. The case statements handle the changes to the state. All of the component that rely on the state contained *here* will re-render when the state is modified. 

# Connect to the Store 
Lets say we want to map User's state to a Navigation Bar as props. 

1.  Place this in your boilerplate `import { connect } from 'react-redux'; `
2.  Change your `export default Navigation` to this `export default connect(mapStateToProps) (Navigation)`
3.  Now write this method 

```
const mapStateToProps = state => {
    return {
        user: state.user
    }
}
```

Result may look like this...

```
import React, { Component } from 'react';
import { connect } from 'react-redux'; 



class Navigation extends Component {
    
    render() {
        return(
            <div className='navigation'>
                 <img src={this.props.user.img_url} /> 
            </div>
        )
    }
}

const mapStateToProps = state => {
    return {
        user: state.user
    }
}

export default connect(mapStateToProps) (Navigation)
```

In the `mapStateToProps` function we are in effect saying: Return to me an object with the key of user and the value of state.user from the store. Pass it into this Component through props. Pretty amazing. Now we can render a profile image on the navigation of a website. 

Now all we have to learn is how do we modify state inside the store. Enter `dispatch`...

Stay coding :) 




