---
layout: post
title:      "Sneaker Finder Ruby Gem "
date:       2020-05-30 19:36:25 -0400
permalink:  sneaker_finder_ruby_gem
---



After a dedicated 4 weeks of an eat, sleep, and code routine here at this amazing Flatiron program, I arrived at a milestone, my first CLI project. Henceforth, the birth of the Sneaker_Finder Ruby Gem. I want to take some time to document my findings. I also want to share the key take away points in my learning journey thus far. I believe this gem summarizes it well. Let's begin... 

The purpose of this gem is to scrape the inventory from a well-known e-commerce store that has a collection of rare, hard-to-find sneakers that collectors are looking for. I used `open-uri` to make an https request, and I used the wonderful `nokogiri` gem to parse the HTML into iterable objects. That is where the real fun begins! Note an example of this code below:

`doc = Nokogiri::HTML(open(https://uharston.github.io/))`

The value of the `doc` variable is the entire web page's HTML. We can call `#css` on `doc` which allows us to specify which data on the web page we would like to isolate by using HTML tag, CSS classes, and/or id's as arguments. Once we do this we are set to iterate. 

`doc.css(".ProductList li")`

One thing I wanted to emphasize is how important it is to know the return values for iterators. Much time can be saved when we are aware of this. We can illustrate this well with `#map` vs `#each`.  In developing this gem I used both of these iterators. 

*  `#map` can be used to modify data, and it **always** returns this data in an array. Note the following method from the `scraper` class. 

```
    def self.scrape_brand_index_page_one(index_url) 
        doc = Nokogiri::HTML(open(index_url))
        doc.css(".ProductList li").map do |shoe|
            hash = {}
            hash[:shoe_name] = shoe.css(".ProductDetails a").first.text
            hash[:price] = shoe.css(".ProductPriceRating em").text
            hash[:shoe_href] = shoe.css(".ProductDetails a").first['href']
            hash
        end
    end
```

In the CLI,  when the user request to see a collection of shoes, this method would iterate with `#map`  to find the data, assign it to a new hash upon every iteration, and **(here is the point)** return each hash as an element into a newly created array. Each of these hashes in this array is used to instantiate shoe objects with unique attributes. The `#map` method is quite useful indeed. 

* `#each` can be used to modify data within the method but the return value specific to `#each`  will return the same class of object it was called on in an unmodified state. 

`#each` is quite useful when you only want to `puts` some data. With this gem, I did find another use for it that was a pleasant surprise. 

A key objective of this gem is to only scrape the necessary data. Only to look for information if it is requested. Therefore when a user would like to see more information on a single and specific shoe, it is necessary to scrape this data, update what we know on this shoe, and then display this data. So instead of retrieving information for a **collection** of shoes, we are retrieving information for a **single**  shoe. Programmatically, speaking instead of an array of hashes, we are working with a single hash. 

In the below example we see that `#each` is used to simply modify the content of our `hash` and the `# self.shoe_details` then returns this hash. 

```
    def self.shoe_details(index_url)
        details = Nokogiri::HTML(open(index_url))
        hash = {}
        details.css(".ProductDetailsGrid").each do |detail|
            hash[:shoe_name] = details.css("h1").text 
            hash[:brand] = details.css(".specs span")[0].text 
            hash[:condition] = details.css(".specs span")[1].text 
            hash[:colorway] = details.css(".specs span")[2].text 
            hash[:year] = details.css(".specs span")[3].text 
            hash[:productid] = details.css(".specs span")[4].text 
            hash[:sizes_available] = details.css(".list-horizontal .name").map { |sizes| sizes.text }.join(', ')
        end
        hash
    end
```

So it is quite amazing what you can do with common iterators like `#map` and `#each`. There are so many things I can share regarding this project. The key take away point for me is this: **Know what your methods return.**

I would also like to make a shout out to the `#send` method which greatly simplifies the instantiation of class instances. Below we can see the a common set up for this: 

```
    def initialize(hash)
       hash.each do |key, value|
        send("#{key}=", value)
       end 
       @@all << self 
    end 
```

There is really a wealth of knowledge to be had when starting your own project. It is quite encouraging and invigorating to create something out of nothing, so to speak. I am quite happy with the rigorous hours used to develop this gem. I hope you enjoyed my findings. 

Stay coding my friends =)








