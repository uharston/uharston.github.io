---
layout: post
title:      "Sneaker Finder Ruby Gem "
date:       2020-05-30 19:36:25 -0400
permalink:  sneaker_finder_ruby_gem
---


After a dedicated 4 weeks of an eat, sleep, and code routine here at this amazing Flatiron program, I arrived a milestone, my first CLI project. Henceforth, the birth of the Sneaker_Finder Ruby Gem. I want to take some time to document my findings. I also want to share the key take away points in my learning journey thus far. I believe this gem summarizes it well. 

The purpose of this gem is to scrape the inventory from a well known e-commerce store that has a collection of rare, hard-to-find sneakers that collectors are looking for. I used `open-uri` to request this data, and `nokogiri` to parse the html into objects that could receive an iterator such as `.each` or `map`. 

```
def self.scrape_brand_index_page_one(index_url)
        doc = Nokogiri::HTML(open(index_url))
        shoe_collection_array = []
        doc.css(".ProductList li").map do |shoe|
            hash = {}
            hash[:shoe_name] = shoe.css(".ProductDetails a").first.text
            hash[:price] = shoe.css(".ProductPriceRating em").text
            hash[:shoe_href] = shoe.css(".ProductDetails a").first['href']
            hash
        end
    end

```  def self.scrape_brand_index_page_one(index_url)
        doc = Nokogiri::HTML(open(index_url))
        shoe_collection_array = []
        doc.css(".ProductList li").map do |shoe|
            hash = {}
            hash[:shoe_name] = shoe.css(".ProductDetails a").first.text
            hash[:price] = shoe.css(".ProductPriceRating em").text
            hash[:shoe_href] = shoe.css(".ProductDetails a").first['href']
            hash
        end
    end


As we well know, there is so much data available to us then ever before. Thanks to website scraping and/or APIs, us developers can access, iterate through, and pluck out the needed information to make useful applications. Gems like 'nokogiri' make it easier for us to access this information. I made good use of 'open-uri' as well as 'nokogiri' in this project.




1. Explain Nokogiri
2. Explain Class Methods vs Instance Methods
3. Explain how the .each differs from .map 






