---
layout: post
title:      "CLI Recipe Finder Gem"
date:       2020-04-23 06:47:12 +0000
permalink:  cli_recipe_finder_gem
---


It's been about 2.5 weeks since I started Flatiron's Self-Paced Online Software Engineering program, and it's already time for my first portfolio project. Despite COVID-19 wrecking most of my senior-year plans and activities, I'm actually very grateful to have this opportunity to learn software engineering and web development, which is much more than I was able to do at the University of California, San Diego. This program has been a great opportunity for me to learn new software development skills and pursue an interest that I previously have not had the time or resources to learn. I am super excited to present and talk about my project! 

**The project's requirements are as follows: **
1. Provide a CLI
2. CLI application must provide access to data from a web page.
3. The data provided must go at least one level deep. A "level" is where a user can make a choice and then get detailed information about their choice. 
4. Use good OO design patterns. Should create a collection of objects, not hashes, to store your data. Pro Tip: Avoid scraping data more than once per web page - utilize objects that have already been created to speed up the program!

### Step 1: Choosing a topic 

I wasn't quite sure what to do my project on. I actually really liked the Command Line Music Player that we had been working on throughout the course, but I knew I wasn't allowed to create a project too similar to the assignments. 

Then, my friend had a great idea to come up with a recipe finder! This would be especially helpful since so many restaurants are closed during the Coronavirus pandemic. I liked the idea, and thought it would be useful for my own life as well for quickly searching categories and recipes without sifting through all the advertisements and unecessary images. 

I decided to base my project off of scraping the allrecipes.com website for recipe categories and recipe information. 

### Step 2: Project Planning

With my idea in mind, I started to plan out my project design and also watch the walk-through video posted in the course. I built a flow chart to help me decide which Ruby classes I would need, what each one would do, and how they would interact with each other as well as the user. 

![](https://github.com/zchesny/recipe_finder/blob/master/CLI%20Project%20Planning.png)

I watched Avi's video, which was recommended in the CLI Project Planning Resources lesson. This video detailed how to set up a gem using bundler. Another video helped me set up a GitHub repo and clone it to my local computer so I could work on the project and then commit and push my changes throughout the process. 

### Step 3: Creating my skeleton app and repo on [GitHub](https://github.com/zchesny/recipe_finder)

Watching the CLI Walkthrough videos helped me create my project skeleton by opening Sandbox and using the `bundle gem` command. I entered `bundle gem recipe_finder` using underscores to create proper namespacing for my project instead of using dashes. This was important to note because using dashes creates additional folders for each dashed word, but underscores works great when creating a gem_name.

I then cloned my GitHub repository to my local computer to work on my project in Atom using the PlatformIO IDE Terminal package. I had to use the HTTPS link rather than the SSH version because I kept getting the following error: 

```
git clone git@github.com:zchesny/recipe_finder.git
Cloning into 'recipe_finder'...
Warning: Permanently added the RSA host key for IP address '192.30.255.112' to the list of known hosts.
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

After trying to Google this error, I decided I would just use HTTPS because I know that version has worked in the past and I wanted to get started on my project. 

I added dependecies such as pry, nokogiri, and colorize in the gemspec file and updated my environment file to require these gems (as well as open-uri). I ran `bundle install` to ensure the gems were there in development, staging, and production. I initially had some issues running `./lib/console` because my version of Ruby wasn't allowing me to install all the necessary Gems, but I was able to Google and resolve this issue by installing another version of Ruby (not the standard one that comes with Mac). I also quickly got a pull request regarding security vulnerability to update rake requirement from ~> 10.0 to ~> 13.0. I made sure to make and commit Git messages early and often!

The setup part was definitely a little tricky, but once it was done, I was ready to start building my classes. 

### Step 4: Defining my classes and their functionality

I knew from crearting the flow chart that I needed a CLI, Category, Scraper, and Recipe class. 

I began witth the executable file ./bin/recipe-finder to call `RecipeFinder::CLI.new.call` (which at this point was code I wished I had, but had not yet created) to create an entry point for the user to interact with my app and allow everything else to stem from there. I was closely following Avi's instructions, when he recommeded this as workflow for building a CLI gem: 

1. Plan your gem, imagine your interface
2. Start with project structure - Google
3. Start with the entry point - the file run
  - Start programming from where the user will interact with it
  - The executable
4. force that to build the CLI interface
5. stub out the interface
6. start making things real
7. discover objects
8. program

I stubbed out the methods in each class, and began mostly building the CLI class to see it working with fake data and basic methods. I knew that I wanted everything to originate from a call method, which would start by printing a greeting, a menu of options for the user to choose from, and finally an exit call. I then tackled the Category class, again creating fake categories just to see if my CLI menu methods could interact with my fake Category objects. This was to see if the CLI class could correctly call and list all Category objects. Finally, I started working on Scraper class to see if I could scrape the [real webpage](https://www.allrecipes.com/) and extract the correct information for each Category name, url, and recipes. Category recipes was initialized to an empty array because I didn't want to scrape each recipe unless I actually had to (requested by the user) in order to keep my program running efficiently. 

I finally had a basic structure of a working CLI class that could interact with and create Category objects, which were produced from scraping the webpage. I followed a similar style of scraping that I learned in this course where `RecipeFinder::Scraper.scrape_categories(base_url)` would return an array of hashes for each category. The hash keys would be name, url, and index. I instantiated the categories by iterating over the array of hashes and adding the details from the hashes using `self.send("#{k}=", v)` for each k,v pair in the hash. 

```
class RecipeFinder::Category

  attr_accessor :name, :url, :recipes, :index

  @@all = []

  def initialize(category_hash)
    category_hash.each{|k, v| self.send("#{k}=", v)}
    self.class.all << self
  end

  def self.create_from_collection(category_array)
    category_array.each{|category_hash| self.new(category_hash)}
  end

  def self.all
    @@all
  end

  def self.count
    self.all.size
  end

  def self.list
    puts "\nCategories List:".colorize(:light_magenta)
    puts "----------------------".colorize(:light_magenta)
    self.all.each.with_index(1) do |category, i|
      puts "#{i}. #{category.name}".colorize(:light_magenta)
    end
  end

  def self.find_by_index(index)
    self.all.detect{|category| category.index == index}
  end
```

I next wanted to explore *one level deeper*, and look at the recipes in each category. I created a Recipe class which would hold recipe objects. Similar to the Category class creation process, I stubbed out my Recipe objects and mehods with fake data so I could ensure the interaction between classes was correct before moving on to the scraping portion. I then scraped an individual category webpage to gain preliminary data for each recipe on the "Most Made Today" recipe list from that [page](https://www.allrecipes.com/recipes/78/breakfast-and-brunch/?internalSource=top%20hubs&referringContentType=Homepage). 

Once I was satisfied with my working CLI, Category, and Recipe class interaction, I added the code for the actual scraping of recipe preliminary information (name, rating, url). Once I was satisfied that everything to that point worked, I decided to go *an additional level deeper* and scrape the actual [webpage for the recipe](https://www.allrecipes.com/recipe/279285/ranch-zucchini-chips/?internalSource=streams&referringId=76&referringContentType=Recipe%20Hub&clickId=st_recipes_mades) itself for additional atributes such as author, description, ingredients, directions, and nutrition. This part was actually a bit easier because I was finally getting a hang of the css selectors and how to use classes, id's, and tags to select for the information I needed. However, once I got this part mostly working, I was dismayed to see that some of my recipes would not load the additional information. After some intense debugging with pry, I found out that this was due to the fact that some recipe urls actually had a different HTML setup! One version called author using the class `.author-name` whereas others referred to this same piece of information as `.submitter__name`! This was initially very frustrating as it required me to write an entirely new case for scraping a website depending on which HTML tags and styles it followed. I realize that it can be very tricky/ annoying if a website you're scraping changes, because your entire application may not work!

I also originally wanted to add information like cook time, prep time, total cook time, etc., but after some HTML inspection and scraping, I realized these attributes were too difficult to select and weren't always in the same format across webpages. I settled for the attributes I listed above, and also found out that "nutrition facts" for my other version of the [HTML webpages ](https://www.allrecipes.com/recipe/162760/fluffy-pancakes/?internalSource=streams&referringId=78&referringContentType=Recipe%20Hub&clickId=st_recipes_mades) (with 'submitter__name') was too difficult extract.

### Step 5: Refining my menu, fixing bugs, and integrating user feedback

Once I had everything working, I decided to redesign my CLI menu and interface to create a bettter user experience. My first pass attempt was rather janky and didn't quite have the flow I would expect from a well-polished recipe finding app. I closely took this [lesson on CLI Interfaces](https://github.com/learn-co-curriculum/cli-interfaces-readme#program-loop) to heart, and wanted my program to mimic this user interface: 

```
$ bin/jukebox

Hi! Welcome to Command Line Music.

What would you like to do?
I accept: list, play, help, and quit.
help↵

Main Menu Commands:
  help - Brings up this dialog.
  list - Will list all the songs in my collection.
  play - Will prompt for a song to play and play that song.
  quit - Will exit this program

What would you like to do?
I accept: list, play, help, and quit.
list↵

My songs are:
1. Shake It Off, by Taylor Swift
2. In An Aeroplane Over the Sea, by Neautral Milk Hotel
3. Reality Check, by Binary Star
4. Hey boy, hey girl, by the Chemical Brothers

What would you like to do?
I accept: list, play, help, and quit.
play↵

Please enter the song number you would like to play:
3↵

Reality Check, by Binary Star is currently open in your browser.

What would you like to do?
I accept: list, play, help, and quit.
```

I added some color for aesthetic appeal using the colorize package, refactored all my menu and CLI class code to more closely match the style shown, and then asked that same friend who helped me come up with my idea to test my app out! I was very excited to show it, and everything seemed to be working beautifully until he entered a number out of range! AHH! The whole program came to a crashing stop. I realized I needed to implement checks to check if the user entered a valid category or recipe number before trying to look it up. A simple if statement fixed this issue: `if input.to_i.between?(1, category.recipe_count)`, so that I would no longer try to look up recipes that did not exist! I also took his feedback and added a "back" option, so that if a user inputs an invalid number, they can either try again and enter a valid number, or go back to the main menu for more options. 

I am very pleased with my final result. The overall Program Loop looks something like this (but with colors, of course!)

```
$ ./bin/recipe-finder 
Hi! Welcome to Command Line Recipe Finder.

Welcome to the Main Menu!
I accept: help, cook, list, and quit.
What would you like to do?
help

Main Menu Commands:
  help - Brings up this dialog.
  cook - Will prompt for a category and find recipes from that category.
  list - Will list all the recipe categories.
  quit - Will exit this program.

Welcome to the Main Menu!
I accept: help, cook, list, and quit.
What would you like to do?
list

Categories List:
----------------------
1. Quarantine Cooking
2. Appetizers and Snacks
3. Breakfast and Brunch
4. Cake Recipes
5. Chicken Recipes
6. Cookies
7. Instant Pot® Recipes
8. Shrimp Recipes
9. Slow Cooker Recipes
10. Soups, Stews and Chili

Welcome to the Main Menu!
I accept: help, cook, list, and quit.
What would you like to do?
cook

Please enter the category number you'd like to search:
4

Recipes for Cake Recipes: 
----------------------
1. Pecan Sour Cream Coffee Cake
2. Banana Cake VI
3. Our Best Cheesecake
4. Banana-Nut Cake with Caramel Icing
5. Italian Lemon Coffee Cake
6. Chocolate Cupcakes
7. Red Velvet Cupcakes
8. Chef John's Carrot Cake
9. Easy PHILLY OREO Cheesecake
10. Lemon Meringue Cheesecake

You are currently in the Cake Recipes Category.
I accept: help, cook, list, back, and quit.
What would you like to do?
help

Cake Recipes Menu Commands:
  help - Brings up this dialog.
  cook - Will prompt for a recipe and return information about that recipe.
  list - Will list all the recipes in Cake Recipes category.
  back - Will list all the recipe categories and bring you back to the main menu.
  quit - Will exit this program.

You are currently in the Cake Recipes Category.
I accept: help, cook, list, back, and quit.
What would you like to do?
cook

Which recipe number would you like to know how to make?
2

----------------------
BANANA CAKE VI
  author: Cindy Carnes
  rating: Rated 4.76 out of 5 stars
  description: This cake was first made for me by a friend while I was visiting her after she had delivered her 11th child. I told her, 'I should have baked for you!'
  ingredients:
    - ¾ cup butter
    - 2 ⅛ cups white sugar
    - 3  eggs
    - 2 teaspoons vanilla extract
    - 3 cups all-purpose flour
    - 1 ½ teaspoons baking soda
    - ¼ teaspoon salt
    - 1 ½ cups buttermilk
    - 2 teaspoons lemon juice
    - 1 ½ cups mashed bananas
    - ½ cup butter, softened
    - 1 (8 ounce) package cream cheese, softened
    - 3 ½ cups confectioners' sugar
    - 1 teaspoon vanilla extract
  directions:
    1. Preheat oven to 275 degrees F (135 degrees C). Grease and flour a 9x13 inch pan. In a small bowl, mix mashed bananas with lemon juice, set aside. In a medium bowl, mix flour, baking soda and salt. Set aside.
    2. In a large bowl, cream 3/4 cup butter and 2 1/8 cups sugar until light and fluffy. Beat in the eggs one at a time, then stir in 2 teaspoons vanilla.  Beat in the flour mixture alternately with the buttermilk. Stir in banana mixture. Pour batter into prepared pan.
    3. Bake in preheated oven for 1 hour, or until a toothpick inserted into the center of the cake comes out clean. Remove from oven and place directly into freezer for 45 minutes. This will make the cake very moist.
    4. For the frosting: In a large bowl, cream 1/2 cup butter and cream cheese until smooth. Beat in 1 teaspoon vanilla. Add confectioners sugar and beat on low speed until combined, then on high until frosting is smooth. Spread on cooled cake.
  nutrition facts (per serving): 453 calories; 18.4 g total fat; 79 mg cholesterol; 299 mg sodium.68.6 g carbohydrates; 5.2 g protein;
  url: https://www.allrecipes.com/recipe/8333/banana-cake-vi/
----------------------

You are currently in the Cake Recipes Category.
I accept: help, cook, list, back, and quit.
What would you like to do?
back

Categories List:
----------------------
1. Quarantine Cooking
2. Appetizers and Snacks
3. Breakfast and Brunch
4. Cake Recipes
5. Chicken Recipes
6. Cookies
7. Instant Pot® Recipes
8. Shrimp Recipes
9. Slow Cooker Recipes
10. Soups, Stews and Chili

Welcome to the Main Menu!
I accept: help, cook, list, and quit.
What would you like to do?
cook

Please enter the category number you'd like to search:
11
Sorry, invalid entry. Please enter a valid category number from the list or enter 'back'.

Please enter the category number you'd like to search:
1   

Recipes for Quarantine Cooking: 
----------------------
1. Simple Tomato Soup
2. World's Best Lasagna
3. Air Fryer Chicken Taquitos
4. The Best Chili
5. The Best Baked Rice and Beans
6. Slow-Cooker Corned Beef and Cabbage
7. Italian Sausage Ravioli Bake
8. Spaghetti Sauce with Ground Beef
9. Healing Cabbage Soup
10. Instant Pot(R) Hamburger Soup

You are currently in the Quarantine Cooking Category.
I accept: help, cook, list, back, and quit.
What would you like to do?
cook

Which recipe number would you like to know how to make?
9

----------------------
HEALING CABBAGE SOUP
  author: JGCASE
  rating: Rated 4.67 out of 5 stars
  description: "My body craves this soup whenever I have a cold, but it's good anytime. Due to the garlic, however, it might be a good idea to be sure that everyone around you eats it, too!"
  ingredients:
    - 3 tablespoons olive oil
    - 1/2 onion, chopped
    - 2 cloves garlic, chopped
    - 2 quarts water
    - 4 teaspoons chicken bouillon granules
  directions:
    1. In a large stockpot, heat olive oil over medium heat. Stir in onion and garlic; cook until onion is transparent, about 5 minutes.
    2. Stir in water, bouillon, salt, and pepper. Bring to a boil, then stir in cabbage. Simmer until cabbage wilts, about 10 minutes.
    3. Stir in tomatoes. Return to a boil, then simmer 15 to 30 minutes, stirring often.
  nutrition facts (per serving): unavailable
  url: https://www.allrecipes.com/recipe/82923/healing-cabbage-soup/
----------------------

You are currently in the Quarantine Cooking Category.
I accept: help, cook, list, back, and quit.
What would you like to do?
quit

See you later for more recipe finding!!!

```

Stay tuned for a video demo/ walkthrough of my app on GitHub! If you'd like to see how it works, clone or download the repo [here](https://github.com/zchesny/recipe_finder), and try it out for yourself! I look forward to working on new coding projects :-)






