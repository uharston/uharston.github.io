---
layout: post
title:      "Latest Trick: Go Fetch!"
date:       2020-08-30 22:55:04 +0000
permalink:  latest_trick_go_fetch
---



Knowledge is only powerful if you know what to do with it. Wisdom is actually knowing what to do with that knowledge once you aquire it. This applies to so many areas of life, and for us programmers, getting data and knowing what to do with it is a daily occurence. Let's talk about how we fetch data from endpoints, and then how we actually do something useful with that data. 

Let's say your building an automotive website, and you need to keep this select tag up-to-date with all the current vehicule makes. 

```
            <select name="make" id="make" >
                    <option id='make-option' value="">Make</option>
            </select>
						
```

Let's see how we could us a fetch call to accomplish this task. 

# Fetch Request
Once your page starts to load we want make our fetch call. 

```
    fetchAllMakes() {
        fetch(https://vpic.nhtsa.dot.gov/api/vehicles/getallmakes?format=json)
        .then(resp => resp.json())
        .then(renderMakes)
    }
		
```

**Line 1:** The fetch method takes in an API endpoint's URL as it's argument. 

**Line 2:** `.then` takes the response from the fetch call, or the return value of what it is called on, and will forward the response to a call back. 

*Remember, JSON is a key and value string-type data structure that can be converted and accepted by many programming languages. `.json()` converts the response into a Javascript Object. *

**Line 3:** By the time we reach line 3 our requested data has been converted into a Javascript Object and can be passed into the function of choice for rendering on the DOM. 

*Note: I will show how to directly render endpoint data to the DOM for simplicty. You may want to first instansiate data into OOJS as well as integrate into the database on the back end.* 

The cool thing about fetch requests is that it is done asynchronously, which is a fancy way of saying that it doesn't prevent the rest of the page from rendering content for your users to look at while the fetch is processing. 

# Let's Render

Our `resp` is going to be an object with many keys and values. 

```
function renderMakes(resp) {
    const makeOptions = resp.Results.map( car => `<option id="${car.MakeId}">${car.MakeName}</option>`).join(' ')
    const makes = document.getElementById('makes')
    makes.innerHTML = makeOptions 
}

```

**Line 1:** Iterating through our `resp` and making `option` tags with the make name and unique id, then joining all of these makes into one long string. 

**Line 2:** Grabbing the select tag by its id attribute. 

**Line 3:** Inserting our all our option tags with our precious date to the select tag. 


# Conclusion 
And there you have it. That is all you need to get started with the basics of fetching and rendering great data for personal projects, or future customers. The options are endless. If it is fuzzy at first, don't worry. Practice makes perfect. Now go fetch!



